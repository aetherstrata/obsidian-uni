---
tags:
  - reti
  - physical
  - fibra
  - wireline
aliases:
  - OTN
---

Le **OTN** (_Optical Transport Network_) sono lo standard attuale per il trasporto digitale su fibra ottica, nate come evoluzione delle reti [[Gerarchia Digitale#SDH - Synchronous Digital Hierarchy|SDH]] per superarne i limiti intrinseci.

Il problema fondamentale di SDH era che **non gestiva il dominio ottico**: multiplexava flussi elettrici e poi demandava all'operatore la trasmissione in fibra senza standardizzare lunghezze d'onda, gestione multi-operatore o integrazione WDM.

Le OTN sono definite principalmente dallo standard **ITU-T G.709** e sono usate nelle reti dorsali di tutti gli operatori globali; in Italia TIM, come gli altri carrier, le adotta per il trasporto sia del traffico mobile ([[Sistemi mobile#4G - LTE (Long Term Evolution)|4G]]/[[Sistemi mobile#5G NR - New Radio|5G]]) sia fisso a banda larga.

> [!important] Limiti di SDH superati da OTN
>
> - **Nessuna gestione ottica**: SDH operava esclusivamente in elettrico; ogni rigenerazione richiedeva una conversione ottico->elettrico->ottico (**3R**)
> - **Nessuno standard WDM**: le lunghezze d'onda usate da ciascun operatore non erano coordinate
> - **Impossibilità multi-dominio**: difficile controllare la qualità del segnale attraverso operatori e vendor diversi
> - **Bit rate fisso a 125 μs**: il frame SDH era rigido e legato alla vecchia telefonia PCM

## Architettura Generale

Le OTN sono strutturate su **due gerarchie parallele** che si sovrappongono:

1. **Dominio digitale (elettrico)**: OPU -> ODU -> OTU
2. **Dominio fotonico (ottico)**: OCh -> OMS -> OTS

L'obiettivo principale è mantenere il segnale nel dominio ottico il più a lungo possibile, riducendo al minimo le conversioni elettro-ottiche costose. Il tutto è integrato con **[[Forward Error Correction|FEC]] nativo**, gestione **OAM end-to-end** e supporto a **client eterogenei** (Ethernet, [[Gerarchia Digitale#SDH - Synchronous Digital Hierarchy|SDH]], Fibre Channel, IP/MPLS).

![[Pasted image 20260616221112.png]]

### Caratteristiche

- **Scalabilità**: bitrate fino a 400G
- **Affidabilità**: monitoraggio delle connessioni; funzionalità integrate per la protezione e la garanzia della disponibilità del servizio.
- **Flessibilità**: provisioning per l'aggiunta di nuovi servizi (on demand) o per le variazioni nei servizi esistenti.
- **Sicurezza**: hard partitioning del traffico su circuiti dedicati garantiscono un alto livello di privacy. _AES_ permette connessioni sicure ad elevato throughtput.
- **Ottimizzazione**: si utilizza una struttura di frame standardizzata per trasportare più segnali client su un singola lunghezza d'onda. Il costo complessivo per il trasporto dei dati si riduce e si utilizza la banda in maniera efficiente.

### Interfacce

Le OTN standardizzano le interfacce tra operatori e vendor, risolvendo il problema della frammentazione proprietaria pre-OTN:

- **UNI** (User-to-Network Interface): tra cliente e rete OTN
- **NNI** (Network Node Interface): tra nodi della stessa rete
- **IaDI** (Intra-Domain Interface): all'interno del dominio di un singolo operatore, può includere elementi proprietari
- **IrDI** (Inter-Domain Interface): tra domini di operatori diversi, **completamente standardizzata** per garantire interoperabilità

![[Pasted image 20260616220813.png]]

Prima delle OTN, la **visibilità end-to-end** era impossibile nelle reti DWDM proprietarie: il segnale poteva essere verificato solo ai punti di accesso del segnale client. Con OTN, i wrapper digitali possono essere monitorati **in transito** attraverso qualsiasi dominio.

---

## Dominio Digitale

I tre livelli digitali corrispondono ai tre overhead SDH, ma con funzioni estese:

### OPU - Optical Payload Unit

Trasporta i bit del servizio client. Effettua l'**incapsulamento del segnale cliente** e l'adattamento di velocità tramite tecniche di mapping (AMP – Asynchronous Mapping Procedure, BMP – Bit-synchronous, GMP – Generic Mapping Procedure). Non contiene informazioni di monitoraggio: è il corrispettivo del **Path** [[Gerarchia Digitale#SDH - Synchronous Digital Hierarchy|SDH]] nel dominio del payload.

### ODU - Optical Data Unit

È il livello di servizio e di gestione, corrisponde al livello **Line** di SONET/SDH. Contiene:

- **Path Monitoring (PM)**: supervisione end-to-end della connessione
- **Tandem Connection Monitoring (TCM)**: 6 livelli indipendenti di monitoraggio
- **OAM** (Operation, Administration & Maintenance): controllo SLA, fault management

### OTU - Optical Transport Unit

Effettua la trasmissione affidabile **nodo-nodo**, corrisponde al livello **Section**. Contiene:

- **[[Forward Error Correction|FEC]]** (_Forward Error Correction_): calcolato sull'intera trama
- **FAS** (_Frame Alignment Signal_): 6 byte con pattern fisso `0xA1 0xA1 0xA1 0xA2 0xA2 0xA2`
- **MFAS** (_Multiframe Alignment Signal_): 1 byte per identificare la posizione nel multiframe e permettere al ricevitore di ricostruirecorrettamente la struttura sincronizzare informazioni distribuite su più frame

#### Calcolo del FEC

1. I **3.824 byte** di ciascuna delle 4 righe vengono suddivisi in **16 sotto-frame** da 239 byte ciascuno
2. Con RS(255,239) vengono aggiunti **16 byte di parità** per ogni sotto-frame -> totale FEC = 256 byte/riga
3. I sotto-frame vengono rimultiplexati -> frame finale = 4080 byte/riga
4. Infine si applica lo **scrambling** per evitare lunghe sequenze di bit tutti uguali

Il guadagno del FEC si traduce in un miglioramento del **budget ottico di circa 6 dB**, permettendo di estendere la portata dei collegamenti o aumentare la capacità senza rigeneratori aggiuntivi. Il FEC consente di operare **vicino al limite di Shannon** per le reti OTN.

## Struttura del Frame OTN

Il frame OTN è una **matrice di 4 righe x 4080 colonne = 16.320 byte**, diversa dalla matrice $9\times90$ di [[Gerarchia Digitale#SDH - Synchronous Digital Hierarchy|SDH]].

Il frame **ODUk** viene costruito partendo dal frame **OPUk** con l'aggiunta delle **14 colonne di overhead ODUk** (colonne 1–14) che si affiancano alle colonne 15–3824 già occupate dall'OPUk.

In dettaglio, quelle 14 colonne contengono:

- **Colonne 1–6 (Riga 1)**: Frame Alignment Signal (**FAS**, 6 byte: `0xA1 0xA1 0xA1 0xA2 0xA2 0xA2`) e **MFAS** (1 byte), che servono per l'allineamento e la sincronizzazione del frame
- **Colonne 7–14 (Riga 1)**: overhead **OTU** (SM – Section Monitoring, GCC0, RES) — anticipati già a questo livello nella struttura della matrice
- **Colonne 1–14 (Righe 2–4)**: overhead **ODUk** vero e proprio, suddiviso in tre sezioni indipendenti:
  - **PM** (Path Monitoring): supervisione end-to-end del percorso
  - **TCM1–TCM6** (Tandem Connection Monitoring): 6 livelli indipendenti per il monitoraggio multi-dominio
  - **OAM** (GCC1, GCC2, APS/PCC, EXP, RES): canali di gestione e manutenzione

La struttura risultante dell'ODUk è quindi una matrice di **4 righe × 3824 colonne = 15.296 byte** (escluso il FEC), con i 14 byte di overhead all'inizio di ogni riga che "avvolgono" il contenuto OPUk.

![[Pasted image 20260616222952.png]]

Il frame **OTUk** viene costruito partendo dal frame **ODUk**, con l'aggiunta dei byte di overhead e, in coda alla trama, il risultato dell'elaborazione dell'algoritmo di [[Forward Error Correction]] applicato all'intera trama. La struttura dettagliata per OTUk è:

![[Pasted image 20260616222753.png]]

Il frame viene trasmesso riga per riga, come in SDH. La **posizione iniziale del payload** all'interno del frame è indicata da un **puntatore** nell'overhead OPU, poiché il segnale client non è necessariamente in fase con il frame OTN.

## Gerarchia ODU e Bit Rate

OTN definisce **cinque livelli TDM** (k = 0, 1, 2, 3, 4) con granularità fissa, più livelli flessibili:

| Tipo             | Bit Rate (Gbit/s) | Durata frame | Capacità tributaria tipica          |
| ---------------- | ----------------- | ------------ | ----------------------------------- |
| **ODU0**         | 1,244             | 98,354 μs    | 1 × 1GbE                            |
| **ODU1**         | 2,498             | 48,971 μs    | 2 × ODU0, STM-16                    |
| **ODU2**         | 10,037            | 12,191 μs    | 8 × ODU0, 4 × ODU1, STM-64, 10GbE-W |
| **ODU2e**        | 10,399            | 11,767 μs    | 10GbE LAN, Fibre Channel 10G        |
| **ODU3**         | 40,319            | 3,035 μs     | 32 × ODU0, 4 × ODU2, STM-256        |
| **ODU4**         | 104,794           | 1,168 μs     | 80 × ODU0, 10 × ODU2, 100GbE        |
| **ODUflex(GFP)** | variabile         | -            | 400GbE, pacchetti IP/MPLS           |
| **OTUCn**        | n × 100G          | -            | Super-channel per 400G+             |

A differenza di SDH che aveva un **frame rate fisso** di 8000 frame/s (125 μs), in OTN il frame rate **varia** in funzione del livello ODU, mantenendo fisso il numero di byte (16.320).

### Multiplexing ODU gerarchico

Il multiplexing avviene attraverso tributary slot\* (**TS**): un ODU di ordine superiore (Higher Order, HO) contiene multiple slot che possono essere occupati da ODU di ordine inferiore (Lower Order, LO) tramite **ODTUjk containers**:

- ODU2 -> contiene fino a **8 ODU0** o **4 ODU1**
- ODU3 -> contiene fino a **32 ODU0** o **16 ODU1** o **4 ODU2**
- ODU4 -> contiene fino a **80 ODU0** o **40 ODU1** o **10 ODU2** o **2 ODU3**

---

## Dominio Fotonico

Il dominio ottico definisce come i segnali vengono trasportati fisicamente sulla fibra, aspetto completamente assente in SDH:

| Layer                           | Acronimo | Elementi di rete                      |
| ------------------------------- | -------- | ------------------------------------- |
| **Optical Channel**             | OCh      | Transponder coerenti, ROADM           |
| **Optical Multiplex Section**   | OMS      | MUX/DEMUX, DWDM ROADM                 |
| **Optical Transport Section**   | OTS      | Transponder, amplificatori EDFA/Raman |
| **Optical Supervisory Channel** | OSC      | Separato dal traffico OTN             |

![[Pasted image 20260616221216.png]]

### OCh - Optical Channel

L'_Optical Channel_ rappresenta il singolo canale ottico end-to-end, cioè una lambda specifica che trasporta il segnale OTUk. Controlla la qualità del segnale ottico (OSNR, CD, PMD). Coinvolge ransponder coerenti e ROADM (add/drop del singolo canale).

### OMS - Optical Multiplex Section

L'_Optical Multiplex Section_ gestisce l’insieme dei OCh che viaggiano insieme sulla stessa fibra. Effettua il multiplexing/demultiplexing WDM. Gestisce lo spettro ottico (potenza complessiva, flex grid, allocazione dinamica delle lambda). Coinvolge MUX/DEMUX e DWDM ROADM.

### OTS - Optical Transmission Section

L'_Optical Transmission Section_ descrive la fibra e la propagazione della luce. Trasmissione fisica del segnale (amplificazione, attenuazione, dispersione, rumore) Coinvolge transceiver e amplificatori.

### OSC - Optical Supervisory Channel

L'_Optical Supervisory Channel_ è un canale ottico ausiliario utilizzato per il controllo, la gestione e la supervisione della rete ottica, separatamente dal traffico utente. Non trasporta traffico cliente (OTN, Ethernet, IP, ..).

### ROADM

Il [[Collegamenti in fibra ottica#ROADM (Reconfigurable Optical Add-Drop Multiplexer)|ROADM]] (_Reconfigurable Optical Add/Drop Multiplexer_) è l'elemento chiave del layer fotonico: permette di **aggiungere, estrarre o instradare lunghezze d'onda dinamicamente** via software, senza interventi fisici. I ROADM moderni supportano il **Flexible Grid (Flex-Grid)** definito da ITU-T G.694.1, che suddivide lo spettro in slice da **12,5 GHz** (con granularità di 6,25 GHz), sostituendo la griglia fissa da 50/100 GHz usata in precedenza. Questo consente di allocare larghezze di banda variabili ai **super-channel** ad alta capacità (100G, 400G, 800G).

### Tandem Connection Monitoring (TCM)

Il **TCM** (_Tandem Connection Monitoring_) è una delle caratteristiche più innovative di OTN rispetto a [[Gerarchia Digitale#SDH - Synchronous Digital Hierarchy|SDH]]: consente di **monitorare in modo indipendente fino a 6 segmenti** (tandem) di una connessione end-to-end, anche attraverso più operatori e domini:

| Livello TCM | Utilizzo tipico                                     |
| ----------- | --------------------------------------------------- |
| **TCM1**    | QoS supervision utente finale                       |
| **TCM2**    | QoS supervision operatore principale (lead carrier) |
| **TCM3**    | Supervisione backbone e interconnessione domini     |
| **TCM4**    | Supervisione protezione (working/protection path)   |
| **TCM5**    | Boundary tra vendor diversi                         |
| **TCM6**    | Riservato                                           |
| **PM**      | Path Monitoring end-to-end                          |

![[Pasted image 20260616220938.png]]

Per ogni tratta monitorata è possibile misurare: **BER** (Bit Error Rate), **ES** (Errored Seconds), **SES** (Severely Errored Seconds) e **Availability**. I vantaggi pratici sono isolamento rapido dei guasti, responsabilità chiara tra operatori, riduzione del **MTTR** (Mean Time To Repair) e supporto nativo per accordi SLA in ambienti **carrier-grade** (disponibilità ≥ 99,999%).

---

## Piano di Controllo: ASON e GMPLS

Le reti OTN di prima generazione erano **statiche**: la configurazione doveva essere richiesta al management dell'operatore, con scarsa flessibilità. Il successivo standard **[[ASON]]** (_Automatically Switched Optical Network_) introduce un **piano di controllo distribuito** basato su **[[GMPLS]]** (_Generalized [[MPLS]]_) per aggiungere flessibilità:

- **LMP** (Link Management Protocol): discovery dei vicini, gestione dei link fisici
- **OSPF-TE** (estensione Traffic Engineering): routing con informazioni sulle risorse ottiche disponibili
- **RSVP-TE**: segnalazione per il setup e il tear-down dinamico dei path ottici

ASON/GMPLS realizza il **provisioning end-to-end dinamico** dei lightpath: un operatore può configurare una connessione ottica su richiesta, con ripristino automatico in caso di guasto, senza intervento manuale. Il piano di controllo è separato dal piano dati, consentendo la gestione multi-dominio e multi-vendor.

---

## Funzioni Principali OTN

La rete OTN si articola in cinque funzioni operative fondamentali:

1. **OTN Adaptation**: accetta segnali client (10GbE, STM-64, Fibre Channel, ecc.) e li incapsula in ODU tramite GFP o AMP/BMP
2. **OTN Switching**: cross-connect ODU -> ODU; riconfigurazione dinamica e **grooming** dei flussi
3. **OTN Multiplexing**: aggregazione gerarchica di più ODUj in un ODUk
4. **OTN DWDM Transport**: incapsulazione dell'OTUk nel dominio fotonico su canali DWDM (flex grid, super-channel)
5. **Control Plane & OAM**: provisioning, protezione, ripristino con GMPLS/ASON; monitoraggio performance tramite TCM]

### Protezione OTN

OTN supporta meccanismi di protezione automatica con **switch-over < 50 ms** (requisito carrier-grade):

- **1+1 Linear Protection**: traffico inviato simultaneamente su percorso lavorativo e di protezione; il ricevitore seleziona il migliore
- **1:N Linear Protection**: un percorso di protezione condiviso tra N percorsi lavorativi
- **Ring Protection**: protezione su topologie ad anello

---

## Confronto OTN vs SONET/SDH

| Caratteristica               | OTN (G.709)                          | SONET/SDH                         |
| ---------------------------- | ------------------------------------ | --------------------------------- |
| **Bit rate massimo**         | 400 Gbit/s (OTUCn oltre)             | ~40 Gbit/s (STM-256)              |
| **FEC**                      | Nativo RS(255,239), ~6 dB gain       | Opzionale/proprietario            |
| **Struttura frame**          | Fissa: 4 righe × 4080 col = 16.320 B | Variabile (9 righe, 90×n colonne) |
| **Frame rate**               | Variabile (1,168 – 98,354 μs)        | Fisso: 125 μs (8000 frame/s)      |
| **Gestione ottica**          | Integrata (OCh/OMS/OTS)              | Assente                           |
| **Monitoring multi-dominio** | 6 livelli TCM indipendenti           | Solo path/line/section monolitici |
| **Client supportati**        | Ethernet, SDH, FC, IP/MPLS, video    | Principalmente PDH/voce           |
| **Interoperabilità**         | IaDI/IrDI standardizzate             | Limitate ai livelli SDH           |
| **Piano di controllo**       | ASON/GMPLS opzionale                 | Nessuno (gestione statica)        |
