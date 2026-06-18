---
tags:
  - reti
  - fibra
  - wireline
  - internet
---

[[MPLS]] nasce nel dominio elettrico per accelerare il forwarding IP sostituendo la lookup dell'indirizzo di destinazione con una semplice lettura di **etichette locali** (label) di 32 bit.

Il meccanismo è efficiente nelle reti a pacchetto, ma inapplicabile direttamente alle reti ottiche poiché ogni nodo dovrebbe convertire il segnale da _ottico a elettrico_ (**O-E-O**) per leggere l'etichetta, vanificando il vantaggio fondamentale delle reti ottiche, ovvero mantenere il dato nel dominio fotonico il più a lungo possibile.

**GMPLS** (_Generalized MPLS_, RFC 3945) risolve questo problema estendendo MPLS con **etichette fisiche ottiche**, permettendo al piano di controllo di operare sull'intera gerarchia di risorse senza richiedere conversioni elettroniche a ogni nodo.

---

## Architettura

GMPLS introduce una netta separazione tra piano di controllo e piano dati:

- **Piano di dati** (_Transport Plane_): la rete ottica fisica, [[Collegamenti in fibra ottica#OXC (Optical Cross Connect)|OXC]], [[Collegamenti in fibra ottica#ROADM (Reconfigurable Optical Add-Drop Multiplexer)|ROADM]], fibra - trasporta il traffico reale e si occupa delle connessioni fisiche
- **Piano di controllo (Control Plane)**: implementato da GMPLS, distribuito su ogni nodo - gestisce routing, segnalazione e discovery topologico, completamente **separato e indipendente** dal piano dati

Questa separazione funziona secondo un modello **client-server**: il piano di controllo fa richieste alla rete di trasporto come un client, mentre la rete ottica risponde come server. I due piani comunicano attraverso un canale di controllo dedicato, spesso implementato tramite l'**OSC** (_Optical Supervisory Channel_) nelle reti [[Optical Transport Network|OTN]].

---

## Switching Capabilities

Il contributo fondamentale di GMPLS rispetto a MPLS è l'introduzione di cinque tipi di _Interface Switching Capability_ (**ISC**), che definiscono il tipo di risorsa usata come etichetta:

| ISC                            | Acronimo | Dominio          | Descrizione                                                                                                              |
| ------------------------------ | -------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------ |
| **Packet Switch Capable**      | PSC      | Elettrico (L3)   | Commutazione basata su pacchetti IP / etichette [[MPLS]] classiche                                                       |
| **Layer-2 Switch Capable**     | L2SC     | Elettrico (L2)   | Commutazione su frame Ethernet/ATM (es. MAC, VPI/VCI)                                                                    |
| **Time-Division Multiplexing** | TDM      | Elettrico/ibrido | Commutazione su **time slot** (es. ODU time slot in [[Optical Transport Network\|OTN]])                                  |
| **Lambda Switch Capable**      | LSC      | **Ottico**       | Commutazione su **lunghezza d'onda** $\lambda$ (OADM, [[Collegamenti in fibra ottica#OXC (Optical Cross Connect)\|OXC]]) |
| **Fiber Switch Capable**       | FSC      | **Ottico**       | Commutazione su **fibra/porta** fisica (patch panel automatici)                                                          |

Le etichette sono **nested** (annidate gerarchicamente): all'interno di una fibra (FSC) ci sono più lunghezze d'onda (LSC), all'interno di una $\lambda$ ci sono più time slot [[Time Division Multiplexing|TDM]], all'interno di un time slot ci possono essere pacchetti (PSC). L'assegnazione di un'etichetta deve rispettare questa gerarchia: non è possibile assegnare una $\lambda$ appartenente a una fibra diversa da quella richiesta.

![[Pasted image 20260617220409.png]]

## Etichette Ottiche vs Etichette MPLS Classiche

| Caratteristica            | Etichette MPLS (PSC)       | Etichette GMPLS ottiche (LSC/FSC)                      |
| ------------------------- | -------------------------- | ------------------------------------------------------ |
| **Formato**               | 32 bit (shim header)       | Caratteristica fisica del collegamento                 |
| **Lettura**               | Elettronica, in ogni nodo  | Ottica/fisica, senza conversione O-E-O                 |
| **Significato**           | Locale al nodo, arbitrario | Associata a una $\lambda$, time slot o porta specifica |
| **Conversione richiesta** | No (già elettrico)         | No (se LSC/FSC, rimane in ottico)                      |

---

## Richiamo MPLS: Basi per Comprendere GMPLS

### Meccanismo di Forwarding MPLS

All'ingresso della rete un **LER** (Label Edge Router) legge l'indirizzo IP del pacchetto e gli assegna un'etichetta, definendo la **FEC** (Forwarding Equivalence Class): tutti i pacchetti della stessa FEC ricevono la stessa etichetta e seguono lo stesso **LSP** (Label Switched Path).

I nodi intermedi, detti **LSR** (Label Switch Router), leggono solo l'etichetta (non l'IP), eseguono un **label swap** e inoltrano il pacchetto senza mai aprire l'header IP. All'uscita, il LER esegue il **pop** dell'etichetta e riprende il routing IP normale.

![[Pasted image 20260617215511.png]]

### Label Stacking

MPLS permette di **impilare più etichette** (label stack): un nodo legge solo l'etichetta più in alto (top label), ignorando quelle sottostanti. Questo meccanismo permette di creare **tunnel gerarchici** e **VPN**: si aggiunge un'etichetta esterna su un LSP già etichettato, creando un tunnel nel tunnel.

![[Pasted image 20260617215319.png]]

---

## Piano di Controllo

GMPLS riutilizza ed estende tre protocolli MPLS con nuove funzionalità per il dominio ottico:

### OSPF-TE - Routing con Traffic Engineering

**OSPF-TE** (_RFC 3630_, _RFC 4203_) è l'estensione di [[OSPF]] per la distribuzione delle informazioni di **Traffic Engineering** (TE). Ogni nodo genera **TE LSA** (Opaque LSA di tipo 10, con scope di area) che contengono, oltre alla normale topologia, attributi aggiuntivi dei link:

- **Bandwidth disponibile** per priorità (0–7)
- **Interface Switching Capability Descriptor (ISCD)**: indica il tipo di commutazione supportato (PSC/L2SC/TDM/LSC/FSC)
- **Link Protection Type**: protezione disponibile (1+1, shared, unprotected)
- **Shared Risk Link Group (SRLG)**: identifica link che condividono risorse fisiche e potrebbero cedere insieme
- **WSON/Flexi-Grid extensions** (RFC 8363): per reti ottiche con griglie flessibili

Ogni nodo raccoglie questi LSA e costruisce un **Traffic Engineering Database (TED)** locale, su cui applica l'algoritmo **CSPF** (_Constrained Shortest Path First_) per calcolare percorsi che rispettano i vincoli richiesti.

### RSVP-TE - Segnalazione e Prenotazione Risorse

**RSVP-TE** (_RFC 3209_, _RFC 3473_) è il protocollo di segnalazione che realizza il **setup dell'LSP** con prenotazione esplicita delle risorse hop-by-hop:

1. Il nodo sorgente (**ingress LSR/LER**) calcola il percorso con CSPF e costruisce un **ERO** (_Explicit Route Object_): la lista ordinata dei nodi/interfacce che l'LSP deve attraversare
2. Invia un messaggio **PATH** hop-by-hop lungo il percorso indicato nell'ERO, che include anche il **Label Request Object** (tipo di etichetta richiesta: $\lambda$, time slot, porta)
3. Il nodo destinazione (**egress LSR**) alloca la risorsa (es. la $\lambda$), crea l'etichetta e risponde con un messaggio **RESV** che viaggia in direzione opposta verso la sorgente
4. Ogni nodo intermedio, ricevendo il RESV, **installa l'LSP nella sua tabella di forwarding** e riserva la risorsa ottica corrispondente
5. Quando il RESV arriva alla sorgente, l'LSP è attivo end-to-end e le risorse sono prenotate

In GMPLS il Label Request Object può contenere un'etichetta di tipo **lambda** ($\lambda$ specifica), **time slot** TDM, o **banda flessibile** per flex-grid.

### ERO: Strict vs Loose Hop

L'ERO può specificare percorsi con due modalità:

- **Strict hop**: il nodo successivo deve essere **direttamente connesso**; se il link non esiste, l'LSP viene abbattuto
- **Loose hop**: il nodo successivo è **non necessariamente diretto**; ogni nodo può scegliere il percorso locale verso il prossimo loose hop

### LMP - Link Management Protocol

LMP gestisce la **connettività fisica** tra coppie di nodi adiacenti nel piano di trasporto. Le funzioni principali sono:

- **Neighbor discovery**: rileva automaticamente i nodi adiacenti e i link fisici tra essi
- **Verifica della connettività**: controlla che i link fisici siano funzionanti e correla il piano di controllo con il piano dati
- **Fault localization**: isola il segmento guasto in un link con più canali ottici (es. distingue un guasto su una $\lambda$ da un guasto sull'intera fibra)
- **TE link bundling**: aggrega più link fisici paralleli in un unico **TE link** logico per semplificare il routing

![[Pasted image 20260617220908.png]]

LMP è fondamentale nelle reti ottiche perché il piano di controllo e il piano dati viaggiano spesso su **infrastrutture fisicamente separate** (il segnale di controllo usa l'OSC, non le $\lambda$ dati).

---

## Tipi di LSP

Un LSP GMPLS può essere di tipo diverso secondo la risorsa che usa come etichetta:

![[Pasted image 20260617220527.png]]

Ogni livello può contenere LSP dei livelli sottostanti. Un **LSP di tipo lambda** occupa un'intera $\lambda$ WDM end-to-end, mantenendo il segnale completamente ottico senza mai convertirlo. Un **LSP di tipo TDM** occupa uno o più time slot [[Optical Transport Network#ODU - Optical Data Unit|ODU]] all'interno di una $\lambda$.

---

## Modelli di Integrazione

Esistono due modelli architetturali per l'integrazione tra GMPLS e la rete OTN sottostante:

### Modello Overlay

Il piano di controllo GMPLS e la rete [[Optical Transport Network|OTN]] fisica hanno **routing separati** e scambiano informazioni minime attraverso interfacce standardizzate (UNI). La rete OTN funge da **server** per il piano di controllo client, che vede la rete come una black-box con un insieme di end-point raggiungibili. È il modello usato in ambienti multi-operatore dove la riservatezza della topologia interna è critica.

![[Pasted image 20260617221200.png]]

### Modello Peer (PIR - Peer Integration Routing)

Il piano di controllo GMPLS e gli [[Collegamenti in fibra ottica#OXC (Optical Cross Connect)|OXC]]/nodi OTN partecipano agli **stessi protocolli di routing** (OSPF-TE condiviso). Ogni nodo ha piena conoscenza della topologia dell'intera rete e può calcolare percorsi ottimali end-to-end. Offre migliore ottimizzazione delle risorse ma minore isolamento operativo.

![[Pasted image 20260617221549.png]]

---

## Estensioni GMPLS per OTN

L'integrazione tra GMPLS e le reti OTN (G.709) richiede estensioni specifiche per la gerarchia ODU:

- **OTN-TDM Switching Capability** (ISC = 110): etichette corrispondenti a time slot ODU (ODU0, ODU1, ODU2, ODU3, ODU4, ODUflex)
- **TE LSA con ISCD OTN**: pubblicazione della gerarchia ODU disponibile su ciascun link e del numero di tributary slot liberi
- **Flexible Grid (Flex-Grid)** per LSC: estensioni OSPF-TE definite in RFC 8363 per reti con spaziatura variabile dei canali ottici (multipli di 6,25 GHz)
- **SRLG** (Shared Risk Link Group): pubblicazione dei gruppi di link che condividono lo stesso percorso fisico (stessa fibra o canalizzazione) per calcolare percorsi di protezione disgiunti

---

## Flusso Completo di Setup di un Lightpath GMPLS

A titolo esemplificativo, il setup di un **lightpath lambda** da nodo S1 a nodo S6 in una rete OTN con GMPLS avviene così:

1. **OSPF-TE**: ogni nodo pubblica TE LSA con le $\lambda$ disponibili sui propri link -> ogni nodo costruisce il TED
2. **CSPF su S1**: S1 calcola il percorso S1->S2->S3->S4->S5->S6 che soddisfa il vincolo (una $\lambda$ disponibile su ogni tratta)
3. **RSVP-TE PATH**: S1 invia PATH con ERO = \[S2, S3, S4, S5, S6\] e Label Request = LSC (lambda); ogni nodo intermedio riceve il PATH e verifica la disponibilità
4. **RSVP-TE RESV**: S6 alloca $\lambda$\_x, risponde con RESV contenente l'etichetta $\lambda$\_x; ogni nodo installa $\lambda$\_x nella propria tabella OXC e riserva la risorsa
5. **Lightpath attivo**: quando il RESV arriva a S1, il lightpath è configurato end-to-end e il traffico può fluire otticamente da S1 a S6 su $\lambda$\_x senza mai lasciare il dominio ottico

---

## Riepilogo

| Caratteristica              | MPLS                        | GMPLS                                             |
| --------------------------- | --------------------------- | ------------------------------------------------- |
| **Tipi di etichetta**       | Shim header 32 bit (packet) | PSC, L2SC, TDM, LSC, FSC                          |
| **Dominio**                 | Solo elettrico/pacchetto    | Packet + ottico + TDM                             |
| **Switching**               | Packet switching            | Packet, $\lambda$, time slot, fibra               |
| **Protocollo routing**      | OSPF-TE / IS-IS-TE          | OSPF-TE / IS-IS-TE estesi con ISCD, SRLG          |
| **Protocollo segnalazione** | RSVP-TE / LDP               | RSVP-TE esteso (label ottiche, generalized label) |
| **Discovery**               | Non presente                | LMP (link fisici)                                 |
| **Conversione O-E-O**       | Richiesta a ogni nodo       | Non richiesta per LSC/FSC                         |
| **RFC principali**          | RFC 3031, 3209              | RFC 3945, 3471, 3473, 4203                        |

## WSON - Wavelength Switched Optical Networks

Le **WSON** (Wavelength Switched Optical Networks) sono reti ottiche in cui le connessioni end-to-end, dette **lightpath**, vengono stabilite, mantenute e rilasciate dinamicamente a **granularità di lunghezza d'onda** ($\lambda$) nel dominio fotonico. In pratica, una WSON è l'applicazione concreta di ASON al layer ottico WDM: ogni nodo è un **OXC** o **ROADM** con switching capability **LSC** (Lambda Switch Capable), e il piano di controllo è implementato da **GMPLS**. WSON e GMPLS sono complementari: GMPLS fornisce i protocolli, WSON definisce i modelli di informazione e i vincoli specifici del dominio fotonico.

### Struttura

Una WSON è strutturata su **due piani** distinti:

- **Piano dati**: link in fibra WDM (tipicamente DWDM) che collegano [[Collegamenti in fibra ottica#OXC (Optical Cross Connect)|OXC]]/[[Collegamenti in fibra ottica#ROADM (Reconfigurable Optical Add-Drop Multiplexer)|ROADM]]; le connessioni fisiche trasportano segnali a 10, 40, 100 o 400 Gbit/s per $\lambda$; i lightpath vengono commutati otticamente senza conversioni _O-E-O_
- **Piano di controllo**: basato su GMPLS (OSPF-TE + RSVP-TE + LMP); un **OCC** (Optical Connection Controller) per ogni nodo del piano dati gestisce routing, segnalazione e discovery

### Routing and Wavelength Assignment (RWA)

Il problema centrale nelle WSON è il **Routing and Wavelength Assignment (RWA)**: dato un insieme di richieste di lightpath, assegnare a ciascuna un percorso fisico e una lunghezza d'onda, rispettando due vincoli fondamentali:

1. **Continuità della lunghezza d'onda** (_wavelength continuity constraint_): se la rete è **trasparente** (senza conversione di $\lambda$ nei nodi intermedi), lo stesso $\lambda$ deve essere disponibile su tutti i link del percorso end-to-end
2. **Unicità**: due lightpath che condividono lo stesso link fisico non possono usare la stessa $\lambda$

Il vincolo di continuità non esiste nelle reti con **wavelength converter** (reti **translucide** o **opache**), dove un OXC può cambiare la $\lambda$ del segnale, al costo però di una conversione O-E-O.

#### Impairment-Aware RWA (IA-RWA)

Nelle reti ottiche reali, la qualità del segnale si degrada lungo il percorso a causa di effetti fisici accumulati:

- **Attenuazione** e rumore degli amplificatori [[Collegamenti in fibra ottica#EDFA (Erbium Doped Fiber Amplifier)|EDFA]] (OSNR, Optical Signal-to-Noise Ratio)
- **Dispersione cromatica** (CD) e di polarizzazione (PMD)
- **Cross-talk** tra canali adiacenti (effetti non lineari: XPM, FWM, SRS)
- **Interferenza** accumulata in reti con [[Collegamenti in fibra ottica#ROADM (Reconfigurable Optical Add-Drop Multiplexer)|ROADM]] a gradi elevati

Il semplice RWA che considera solo la disponibilità della $\lambda$ non è sufficiente: un percorso che "esiste" topologicamente potrebbe risultare inutilizzabile perché il BER (Bit Error Rate) supera la soglia ammissibile. L'**IA-RWA** (Impairment-Aware RWA) estende il problema includendo i vincoli fisici come ulteriori criteri nel calcolo del percorso, considerando l'OSNR accumulato, la lunghezza dei link, il numero di amplificatori e di ROADM attraversati. Le informazioni sugli impairment vengono distribuite tramite estensioni OSPF-TE (definite in RFC 7446 e RFC 7579) e elaborate dal PCE.

### PCE - Path Computation Element

Nelle WSON complesse, il calcolo del percorso con vincoli ottici diventa computazionalmente oneroso per essere eseguito distribuito su ogni nodo. Il **PCE** (Path Computation Element, RFC 4655) è un server centralizzato (o distribuito per gerarchia) dedicato al calcolo dei percorsi con piena visibilità del TED:

- Il **PCC** (Path Computation Client), tipicamente il nodo ingresso, invia una richiesta al PCE tramite il protocollo **PCEP** (Path Computation Element Communication Protocol)
- Il PCE riceve la richiesta con i vincoli (sorgente, destinazione, banda, impairment threshold, SRLG da evitare), interroga il proprio TED esteso con le informazioni ottiche e restituisce il percorso calcolato con l'assegnazione della $\lambda$
- Il nodo ingresso usa il percorso ricevuto per popolare l'**ERO** nel messaggio RSVP-TE PATH e segnalare il lightpath

Il PCE può essere **stateless** (calcola ogni path indipendentemente) o **stateful** (mantiene lo stato di tutti i lightpath attivi per ottimizzare globalmente le risorse).

> [!tip] Estensioni GMPLS per WSON
>
> GMPLS originale non modellava i vincoli specifici delle reti WDM ottiche. Sono state definite estensioni standardizzate dall'IETF:
>
> - **RFC 6163**: framework generale per il controllo WSON con GMPLS e PCE; definisce i modelli di nodo ottico (fisso vs configurabile), le restrizioni di connettività e i casi d'uso
> - **RFC 7446/7579/7581**: information model e encoding per RWA; definisce come le informazioni sulla topologia ottica (disponibilità $\lambda$, vincoli di conversione, capacità di switching) vengono codificate e distribuite tramite OSPF-TE
> - **RFC 8363**: estensioni OSPF-TE per il **Flexible Grid** (Flex-Grid); sostituisce la griglia fissa ITU da 50 GHz con slot variabili multipli di **6,25 GHz** per supportare super-channel e modulazioni adattive
> - **RFC 6205**: definisce le etichette GMPLS per reti ottiche con switching a $\lambda$ (label = numero del canale ITU-T G.694.1)

---

## Evoluzioni future

- _Spectrum Switched Optical Network_ - **SSON**: Evoluzione del WSON per gestire la Flex-Grid. Label elastiche: Le etichette definiscono un intervallo di frequenze variabile (es. n x 12.5 GHz) per massimizzare l'efficienza spettrale.
- _Software Defined Networking_ - **SDN**: Spostamento dell'intelligenza dai nodi a un controller centrale, rendendo la rete programmabile e adattiva.
