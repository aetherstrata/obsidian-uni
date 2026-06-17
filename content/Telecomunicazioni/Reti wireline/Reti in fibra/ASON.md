---
tags:
  - fibra
  - physical
  - wireline
  - reti
---

Le reti [[Optical Transport Network|OTN]] standard dispongono di un piano di trasporto e di un piano di gestione, ma **mancano di un piano di controllo distribuito**: ogni volta che un cliente richiede una nuova connessione o più banda, il gestore deve configurare manualmente nodo per nodo la rete, il che richiede tempo e spreca risorse.

> [!info] Vantaggi rispetto a OTN statica
>
> | Caratteristica          | OTN Statica               | ASON/GMPLS                     |
> | ----------------------- | ------------------------- | ------------------------------ |
> | **Provisioning**        | Manuale, nodo per nodo    | Automatico end-to-end, secondi |
> | **Ripristino guasti**   | Manuale (minuti/ore)      | Automatico < 50 ms             |
> | **Banda on demand**     | Non supportata            | SC e SPC su richiesta          |
> | **Optical VPN**         | Richiede intervento OSS   | Attivazione dinamica via UNI   |
> | **Topologia discovery** | Configurazione statica    | Automatica con LMP/OSPF-TE     |
> | **Multi-dominio**       | Difficile                 | I-NNI / E-NNI standardizzate   |
> | **Traffic Engineering** | Nessuno                   | CSPF con vincoli di banda      |
> | **QoS**                 | Non gestita dinamicamente | Priorità e SLA enforcement     |

**ASON** (_Automatically Switched Optical Network_) nasce per colmare questo limite, introducendo un terzo livello funzionale, il **piano di controllo**, capace di creare, modificare e rilasciare connessioni ottiche in modo automatico e su richiesta.

> [!tip] ASON e ASTN
>
> Esistono due standard correlati ma distinti:
>
> - **ASTN** (_Automatically Switched Transport Network_) - framework architetturale ITU-T G.8080, indipendente dalla tecnologia sottostante, applicabile a [[Optical Transport Network|OTN]], [[Gerarchia Digitale#SDH - Synchronous Digital Hierarchy|SDH]], WDM e reti packet
> - **ASON** - applicazione specifica di ASTN alle reti ottiche, focalizzata sull'[[Optical Transport Network|OTN]] e sullo strato fotonico
>
> Per convenzione, nella comunità degli operatori il termine **ASON** viene usato per indicare entrambi.

---

## Piani Funzionali

L'architettura ASON separa nettamente tre livelli, che funzionano secondo un modello client-server:

![[Pasted image 20260617204441.png]]

### Piano di Trasporto (Transport Plane)

Contiene tutta la parte fisica: [[Collegamenti in fibra ottica#OXC (Optical Cross Connect)|OXC]] (_Optical Cross-Connect_), [[Collegamenti in fibra ottica#ROADM (Reconfigurable Optical Add-Drop Multiplexer)|ROADM]], amplificatori [[Collegamenti in fibra ottica#EDFA (Erbium Doped Fiber Amplifier)|EDFA]], transponder, fibra ottica. Realizza il **switching ottico**, le connessioni OCh/ODU e la protezione fisica. I nodi si interconnettono tramite **PI** (_Physical Interface_). Questo livello esiste anche nelle reti [[Optical Transport Network|OTN]] standard.

### Piano di Controllo (Control Plane)

È il livello **nuovo e distintivo di ASON**. È costituito dagli **OCC** (_Optical Connection Controller_), uno per nodo, interconnessi tra loro tramite NNI. Ogni OCC è un agente intelligente che svolge in modo distribuito le seguenti funzioni:

- Analisi della topologia di rete e delle risorse disponibili
- Routing dinamico con **Traffic Engineering (TE)**
- Segnalazione end-to-end per la prenotazione delle risorse
- Instaurazione e rilascio delle connessioni
- Protezione e ripristino automatico delle connessioni
- Assegnazione delle lunghezze d'onda

I protocolli utilizzati sono **OSPF-TE**, **RSVP-TE** e **LMP**, implementati dal framework **[[GMPLS]]**.

### Piano di Gestione (Management Plane)

Supervisione operativa della rete tramite OSS (Operation Support System), che comprende:

- **NMS** (Network Management System): gestione della rete
- **EMS** (Element Management Systems): gestione dei singoli apparati
- **Service Assurance Tools**: controllo delle prestazioni e degli SLA

Il piano di gestione configura le risorse, partiziona la rete in aree di routing, gestisce i guasti, le prestazioni, l'accounting e la sicurezza. Comunica con il piano di controllo tramite le interfacce **NMI-A** e con il piano di trasporto tramite **NMI-T**.

---

## Interfacce ASON

Le interfacce sono il meccanismo chiave che rende ASON interoperabile in ambienti multi-vendor e multi-operatore:

![[Pasted image 20260617204619.png]]

| Interfaccia | Nome                                     | Descrizione                                                                                                                             |
| ----------- | ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| **UNI**     | User-to-Network Interface                | Tra il cliente e la rete dell'operatore. Permette all'utente di richiedere SC dinamicamente                                             |
| **I-NNI**   | Internal NNI                             | Tra nodi dello stesso dominio di controllo (stesso operatore); condivide topologia e segnalazione completa                              |
| **E-NNI**   | External NNI                             | Tra operatori diversi; espone solo la raggiungibilità, non la topologia interna (riservatezza commerciale)                              |
| **CCI**     | Connection Control Interface             | Tra OCC (piano di controllo) e [[Collegamenti in fibra ottica#OXC (Optical Cross Connect)\|OXC]] (piano di trasporto) nello stesso nodo |
| **NMI-A**   | Network Management Interface (ASON)      | Dal piano di gestione verso il piano di controllo                                                                                       |
| **NMI-T**   | Network Management Interface (Transport) | Dal piano di gestione verso il piano di trasporto                                                                                       |
| **PI**      | Physical Interface                       | Tra [[Collegamenti in fibra ottica#OXC (Optical Cross Connect)\|OXC]] adiacenti nel piano di trasporto                                  |

---

## Tipi di Connessione

ASON definisce tre modalità di connessione, che si differenziano per **quale piano avvia e gestisce il circuito**:

![[Pasted image 20260617210720.png]]

### Permanent Connection (PC)

Equivalente alla connessione di una rete [[Optical Transport Network|OTN]] standard: il **piano di gestione** invia comandi direttamente agli [[Collegamenti in fibra ottica#OXC (Optical Cross Connect)|OXC]] tramite NMI-T, senza coinvolgere il piano di controllo. Il circuito resta attivo fino a rimozione esplicita dell'operatore. Non vi è routing automatico né ripristino autonomo.

![[Pasted image 20260617211127.png]]

### Soft Permanent Connection (SPC)

La richiesta parte dal **piano di gestione** (NMS via NMI-A) verso il piano di controllo, il quale calcola in automatico il percorso completo, instrada la segnalazione hop-by-hop e configura gli [[Collegamenti in fibra ottica#OXC (Optical Cross Connect)|OXC]] tramite CCI. L'operatore decide quando creare o rimuovere il circuito, ma il calcolo del percorso e il ripristino in caso di guasto sono **automatici**. È la modalità **più usata in pratica** perché combina il controllo operativo con l'automazione.

![[Pasted image 20260617211156.png]]

### Switched Connection (SC)

Il **cliente** richiede direttamente la connessione al piano di controllo tramite UNI, senza intervento del piano di gestione. Il piano di controllo verifica le autorizzazioni e il rispetto dell'SLA, calcola il percorso, riserva le risorse e configura gli [[Collegamenti in fibra ottica#OXC (Optical Cross Connect)|OXC]]. Quando il cliente non ne ha più bisogno, il circuito viene rilasciato e le risorse tornano disponibili. È il modello che abilita **servizi on-demand**, **bandwidth on demand** e _Optical VPN_ (**OVPN**).

![[Pasted image 20260617211214.png]]

---

## Riepilogo

- **Separazione dei piani**: gestione, controllo e trasporto sono indipendenti e comunicano tramite interfacce standardizzate
- **Intelligenza distribuita**: ogni nodo (OCC) possiede la propria copia del TED e calcola i percorsi in autonomia
- **Indipendenza dalla tecnologia**: ASON si applica a OTN, SDH, WDM e reti packet
- **Supporto multi-dominio**: E-NNI permette di connettere operatori diversi senza esporre la topologia interna
- **Scalabilità**: il piano di controllo può gestire reti nazionali e internazionali con centinaia di nodi
