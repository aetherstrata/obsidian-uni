---
tags:
  - datacenter
  - fibra
  - wireline
  - reti
---

Le reti di trasporto ottiche hanno attraversato quattro generazioni tecnologiche ben distinte:

| Periodo   | Tecnologia                      | Piano di controllo    | Velocità tipica |
| --------- | ------------------------------- | --------------------- | --------------- |
| 1990–2000 | Reti statiche DWDM punto-punto  | NMS manuale           | 10G             |
| 2000–2015 | Reti agili GMPLS/ASON con ROADM | Distribuito (RSVP-TE) | 100G            |
| 2015–2020 | Reti elastiche Flex-Grid        | PCE centralizzato     | 400G–800G       |
| 2020–2026 | Reti cognitive SDN + AI         | AI-Driven / Open API  | 1.2T+           |

Nella prima generazione i sistemi WDM erano completamente statici: non esisteva un piano di controllo, ogni modifica richiedeva intervento fisico in centrale, e i protocolli di trasporto erano [[Gerarchia Digitale#SDH - Synchronous Digital Hierarchy|SONET/SDH]], standardizzati in modo differente in Europa e negli USA. Con [[GMPLS]]/[[ASON]] è nato il **piano di controllo distribuito** che automatizza la creazione dei lightpath, sostituito poi da SDN con controller centralizzati programmabili tramite API aperte (OpenConfig, NETCONF/YANG, OpenROADM).

> [!info] Reti Elastiche Flex-Grid
>
> Nelle reti WDM classiche la griglia di frequenze è **fissa** (es. 50 GHz ITU G.694.1): se un canale trasporta solo 1 Gbit/s è inutile assegnargli una banda di 100 GHz. Le reti **Flex-Grid** risolvono questo problema rendendo la banda assegnata a ogni canale adattiva, con slot multipli di **6,25 GHz**. I transponder moderni usano modulazioni adattive dipendenti dalla distanza e dall'SNR: su link corti con buon OSNR si usa **[[Quadrature Amplitude Modulation|DP-64QAM]]** (6 bit/simbolo × 2 polarizzazioni), su link lunghi si degrada a **[[Phase Shift Keying|DP-QPSK]]** (2 bit/simbolo) per maggiore robustezza.

---

## Guasti nelle Reti Ottiche

Un singolo cavo ottico può trasportare fino a $200 \text{ fibre} \times 160\,\lambda \times 10\,\text{Gb/s} = 320\,\text{Tb/s}$, equivalente a 5 miliardi di linee telefoniche. Un taglio di cavo è quindi una catastrofe che richiede meccanismi di ripristino nell'ordine del millisecondo.

### Tipologie di Guasto

- **Guasto di link**: fibra tagliata, connettore danneggiato, [[Collegamenti in fibra ottica#EDFA (Erbium Doped Fiber Amplifier)|EDFA]] guasto -> segnalato da _Loss of Signal_ (**LoS**) o _Loss of Frame_ (**LoF**) nell'overhead [[Optical Transport Network|OTN]]
- **Guasto di nodo**: un intero [[Collegamenti in fibra ottica#OXC (Optical Cross Connect)|OXC]] o router cade, interrompendo tutti i collegamenti che vi transitano
- **Guasto di canale**: una singola $\lambda$ DWDM si degrada o si interrompe per laser difettoso o filtro ottico deteriorato
- **Soft failure (degradazione)**: nessuna interruzione netta, ma aumento del [[Trasmissione Digitale#Sistema privo di errori|BER]] e riduzione dell'OSNR sotto soglia; rilevata solo dal monitoraggio continuo
- **Guasto multiplo / catastrofico**: due o più guasti contemporanei, o eventi che distruggono un'area geografica intera

### Rilevamento nel Piano di Controllo

Il piano di controllo [[GMPLS]] rileva i guasti attraverso tre meccanismi:

- **Hello / Keep-alive RSVP-TE**: messaggi periodici tra nodi adiacenti; l'assenza di risposta dichiara il nodo irraggiungibile
- **BFD** (Bidirectional Forwarding Detection): protocollo dedicato al rilevamento in pochi _millisecondi_, molto più veloce degli hello dei protocolli di routing
- **Notifiche LMP**: il Link Management Protocol notifica immediatamente il piano di controllo quando rileva un'interruzione fisica sul link

---

## Resilienza della Rete

La **resilience** è la capacità dell'infrastruttura di mantenere attivo il servizio nonostante guasti fisici o logici. Richiede la pre-allocazione di risorse aggiuntive non utilizzate in condizioni normali, con conseguente aumento dei costi. Si distingue in due macro-categorie:

### Protection - Resilienza Statica

I cammini di ripristino sono **prestabiliti e riservati prima** del guasto; non serve alcuna segnalazione aggiuntiva a guasto avvenuto. Tempo di attuazione: **≤ 50 ms**.

#### Schemi di protezione dedicata

- **1+1**: il segnale viene diviso con uno splitter e inviato simultaneamente su due percorsi fisicamente disgiunti; la destinazione misura l'OSNR su entrambi e seleziona il migliore; nessun ritardo di switching
- **1:1**: un cammino di backup per ogni cammino attivo; il backup trasporta traffico a bassa priorità in assenza di guasti; al guasto il traffico low-priority viene rilasciato e il backup attivato

#### Schema di protezione condivisa

- **1:N**: un solo cammino di backup condiviso tra N cammini working; efficiente in risorse ma non protegge da guasti multipli simultanei
- **M:N**: M cammini di backup condivisi tra N working (M < N)

#### Livelli di protezione nelle reti OTN

- **1+1 OTS** (Optical Line Protection - OLP): protezione a livello dell'intera sezione di trasporto ottica tra due amplificatori
- **1+1 OMS**: protezione a livello di sezione di multiplexing ottico
- **1+1 OCH**: protezione del singolo canale ottico ($\$)

### Restoration - Resilienza Dinamica

I cammini di recupero vengono calcolati e allocati **solo all'insorgere del guasto**. Offre maggiore flessibilità, risparmio di risorse e gestione di guasti non previsti a priori, ma richiede più tempo (ordine dei **secondi**). I cammini statici (preassegnati) offrono velocità ma scarsa scalabilità; i cammini dinamici superano questa limitazione a costo di latenza maggiore.

---

## Sistemi di Storage

Esistono tre architetture principali per la memorizzazione dei dati:

### DAS - Directly Attached Storage

Storage collegato direttamente al server tramite bus locale (SCSI, SAS). Semplice ma non condivisibile tra più server e difficile da espandere.

### NAS - Network Attached Storage

Unità di storage connessa alla **rete LAN Ethernet** e accessibile come file server condiviso tramite protocolli **SMB** (Windows) o **NFS** (Unix/Linux). Il NAS presenta un collo di bottiglia critico: tutto il traffico storage passa per lo switch Ethernet, saturandolo rapidamente in ambienti ad alto carico. Non scala bene in ambienti enterprise.

### SAN - Storage Area Network

Rete **dedicata esclusivamente** al trasporto di dati tra server e dispositivi di storage, **separata fisicamente dalla LAN** aziendale. I server si collegano alla SAN tramite **HBA** (Host Bus Adapter) e vedono lo storage come se fosse un **disco locale**, anche se fisicamente distante.

> [!tip] Vantaggi della SAN rispetto a NAS
>
> - Scalabilità indipendente di CPU (server) e memoria (storage)
> - Backup centralizzato ad alte prestazioni senza impatto sulla LAN
> - Latenza ultra-bassa e throughput garantito
> - Gestione centralizzata tramite tool dedicati
> - Configurazioni cluster facilmente espandibili
> - Traffico storage non carica lo switch Ethernet

---

## Fibre Channel

**Fibre Channel (FC)** è un protocollo di rete ad alta velocità standardizzato dall'**ANSI (X3.230-1994)** per il trasporto di dati a blocchi nelle SAN, combinando le caratteristiche di un **canale hardware** (bassa latenza, alta affidabilità) con quelle di una **rete** (grandi distanze, ampia connettività).

**Caratteristiche fondamentali**:

- **Lossless**: meccanismo a buffer credit garantisce zero perdita di frame (non è TCP/IP che usa window e ritrasmissione)
- **Bassa latenza**: forwarding gestito quasi interamente dall'**hardware** (ASIC), senza caricare la CPU del server
- **Sicurezza**: rete fisicamente isolata dalla LAN aziendale, inaccessibile dall'esterno
- **Consegna ordinata** dei frame con controllo del flusso
- **Rilevamento errori via hardware** per ridurre il carico di rete
- **Compatibilità** con supporti misti rame e fibra ottica
- Distanze fino a **10 km** su fibra [[Fibra ottica#Fibre Monomodo (SMF)|SMF]] senza ripetitori

### Stack

Lo stack FC si articola in cinque livelli:

text

`FC-4  │ Mapping ULP (SCSI/FCP-4, IPv4/IPv6, VI, SBCCS...) FC-3  │ Common Services (multicast, hunt groups, striping) FC-2  │ Framing Protocol (frame, addressing, flow control, classi di servizio) FC-1  │ Encoding (8b/10b o 64b/66b, ordered sets, scrambling) FC-0  │ Physical Layer (mezzi, velocità, segnale ottico/elettrico)`

#### FC-0 - Physical Layer

Definisce il mezzo fisico, i connettori (LC, SFP, QSFP), le velocità (1->256 Gb/s), le lunghezze d'onda (850 nm per [[Fibra ottica#Fibre Multimodo (MMF)|fibra multimodale]], 1310/1550 nm per [[Fibra ottica#Fibre Monomodo (SMF)|fibra monomodale]]) e i livelli ottici/elettrici del segnale.

#### FC-1 - Encoding Layer

Gestisce la codifica e la sincronizzazione dei bit:

- **8b/10b** (fino a 8GFC): ogni 8 bit di dati diventano 10 bit trasmessi -> efficienza del **77,7%** (overhead 25%)
- **64b/66b** (da 16GFC in poi): ogni 64 bit di dati diventano 66 bit -> efficienza del **94,2%** (overhead ~3%)
- **Ordered Sets**: sequenze speciali per delimitare frame, segnalare errori e sincronizzarsi
- **Scrambling**: nelle versioni recenti per migliorare la qualità del segnale

#### FC-2 - Framing e Network Layer

È il cuore del protocollo; gestisce:

- **Struttura del frame**: `SOF (4B) | Header (24B) | Payload (0–2112B) | CRC (4B) | EOF (4B)`
- **Indirizzamento a 24 bit** (N_Port ID -> fino a $2^{24} \approx 16,7$ milioni di porte)
- **Flow control** via Buffer-to-Buffer Credits (lossless)
- **Sequenze ed Exchange** per organizzare le operazioni I/O
- **Classi di servizio** (Class 1, 2, 3, F)
- **Routing** dei frame attraverso la fabric

> [!info] Struttura Header
> L'**Header del frame FC** (24 byte) contiene:
>
> - `R_CTL`: tipo di routing/controllo
> - `D_ID` e `S_ID`: indirizzo destinazione/sorgente a 24 bit
> - `CS_CTL / Type / F_CTL`: controllo del frame
> - `SEQ_ID / SEQ_CNT`: identificativo e numero sequenziale della sequenza
> - `OX_ID / RX_ID`: identificatori dell'Exchange originator/responder
> - `Parameter`: dipendente dal tipo di frame

#### FC-3 - Common Services

Servizi di livello superiore condivisi tra più N_Port: **multicast**, **hunt groups** (gruppo di porte che rispondono allo stesso indirizzo) e **striping** (distribuzione di una singola sequenza su più link N_Port simultaneamente).

#### FC-4 - ULP Mapping

Mappa i protocolli di livello superiore (Upper Level Protocol) sul trasporto FC:

- **FCP-4** (Fibre Channel Protocol): incapsula i comandi **SCSI** - è il mapping più diffuso nelle SAN
- **RFC 4338**: trasporto di IP (IPv4/IPv6) su FC
- **FC-VI**: Virtual Interface per cluster ad alte prestazioni
- **FC-SB-5**: Single Byte Command Code Set (per mainframe IBM)

### Buffer-to-Buffer Credits (Flow Control Lossless)

Il meccanismo di flow control FC opera **a livello di link fisico** tra due porte adiacenti, non end-to-end:

1. Durante il **processo di login**, ogni porta comunica alla controparte il proprio **BB_Credit**: il numero di buffer allocati in ricezione
2. Il trasmettitore mantiene un **contatore locale** dei crediti disponibili; ogni frame inviato decrementa il contatore di 1
3. Quando il contatore raggiunge 0, la porta **sospende la trasmissione**
4. Il ricevitore, dopo aver processato un frame e liberato il buffer, trasmette un **Primitive Signal R_RDY** (Ready) che incrementa di 1 il contatore del trasmettitore
5. Poiché ogni frame viene trasmesso **solo in presenza di un buffer garantito** sul ricevitore, il protocollo è intrinsecamente **lossless**

I BB_Credit sono per-hop: in una fabric multi-switch esistono crediti separati su ogni segmento link.

### Sequenze ed Exchange

**Frame** -> **Sequence** -> **Exchange** è la gerarchia delle unità dati in FC:

- **Frame**: unità elementare, max 2112 byte di payload; identificato da `SEQ_ID` e `SEQ_CNT`
- **Sequence**: insieme ordinato di frame trasmessi unidirezionalmente da Originator a Responder (o viceversa); lo `SEQ_ID` identifica univocamente la sequenza all'interno di un Exchange
- **Exchange**: l'intera operazione I/O bidirezionale (es. una lettura SCSI completa); identificato dalla coppia `OX_ID` (assegnato dall'Originator) e `RX_ID` (assegnato dal Responder); contiene più sequenze alternate nelle due direzioni

Un'operazione di lettura disco, ad esempio, è un Exchange composto da: sequenza SCSI Read Command (host->storage) + sequenza dati (storage->host) + sequenza Status (storage->host).

### Topologie

FC supporta tre topologie:

| Caratteristica               | Point-to-Point | Arbitrated Loop     | Switched Fabric                   |
| ---------------------------- | -------------- | ------------------- | --------------------------------- |
| **Max porte**                | 2              | 127 (7 bit)         | ~16,7 milioni (2242^{24}224)      |
| **Dimensione indirizzo**     | N/A            | 8 bit               | 24 bit (FCID)                     |
| **Guasto a un nodo**         | -              | Interrompe l'anello | Nodo escluso automaticamente      |
| **Porte a velocità diverse** | N/A            | No                  | Sì                                |
| **Consegna frame**           | Ordinata       | Ordinata            | Non garantita (ma frame numerati) |
| **Accesso al mezzo**         | Dedicato       | Arbitrato           | Dedicato (commutato)              |

La topologia **Switched Fabric** è quella più usata nelle SAN aziendali: uno o più switch FC interconnessi creano una fabric a cui si collegano i nodi tramite N_Port.

### Tipi di Porte FC

- **N_Port**: porta di un nodo (server, storage); si connette alla fabric o direttamente a un'altra N_Port
- **F_Port** (_Fabric Port_): porta di uno switch FC che si connette a una `N_Port`
- **E_Port** (_Expansion Port_): connette due switch FC tra loro (Inter-Switch Link)
- **FL_Port**: connette uno switch a una topologia ad anello (Arbitrated Loop)
- **G_Port**: porta generica, si auto-configura in E o F
- **NL_Port**: `N_Port` con capacità di loop

## Processo di Login nella Fabric FC

Il processo di login avviene in tre fasi successive prima che un host possa accedere allo storage:

1. **FLOGI (Fabric Login)**: l'HBA invia un frame FLOGI all'indirizzo predefinito `0xFFFFFE` (Fabric Login Server), incluso il suo **WWPN** (World Wide Port Name, identificatore univoco a 64 bit hardcoded nell'HBA); lo switch assegna un **FCID a 24 bit** (Fibre Channel ID) all'N_Port e restituisce ACC con i parametri di servizio (BB_Credits, classi di servizio)
2. **PLOGI (Port Login)**: dopo FLOGI l'N_Port si registra nel **Name Server** della fabric (indirizzo `0xFFFFFC`), comunicando WWNN, WWPN, FCID, tipo di porta e classi di servizio; poi esegue PLOGI verso le N_Port target per scambiare parametri di servizio e stabilire la comunicazione peer-to-peer
3. **PRLI (Process Login)**: stabilisce la comunicazione tra i layer FC-4 (es. SCSI) ai due estremi; initiator e target si accordano sul protocollo specifico (FCP per SCSI), abilitando il trasferimento effettivo di comandi I/O

### Classi di Servizio FC

FC definisce tre classi di servizio principali:

|                       | Class 1                                 | Class 2                          | Class 3                   |
| --------------------- | --------------------------------------- | -------------------------------- | ------------------------- |
| **Modalità**          | Connection-oriented (circuit switching) | Connectionless (frame switching) | Connectionless (datagram) |
| **Banda**             | Dedicata, full bandwidth                | Multiplexed                      | Multiplexed               |
| **Routing**           | Fisso                                   | Adattivo                         | Adattivo                  |
| **Consegna ordinata** | Garantita                               | Non garantita                    | Non garantita             |
| **Conferma consegna** | Sì (ACK)                                | Sì (ACK)                         | No                        |
| **Flow control**      | End-to-end + link                       | End-to-end + link                | Solo link                 |

**Class F** è riservata alla comunicazione intra-fabric tra switch (scambio di informazioni di controllo).

### Zoning

Lo **zoning** è la partizione logica della fabric FC in sottoinsiemi per aumentare la sicurezza e semplificare la gestione:

- Un dispositivo in una zona può comunicare **solo con altri dispositivi della stessa zona**; dispositivi in zone diverse sono completamente invisibili l'uno all'altro
- Una porta può appartenere a più zone contemporaneamente

#### WWN Zoning (Soft Zoning)

I membri della zona sono identificati dal loro **WWPN** (World Wide Port Name). Vantaggio: se una porta è difettosa si usa un'altra porta senza riconfigurare la fabric. Svantaggio: se l'HBA si guasta e viene sostituito (nuovo WWPN), la zona deve essere riconfigurata. Le best practice raccomandano di usare **WWPN** (non WWNN) per evitare conflitti di multipathing.

#### Port Zoning (Hard Zoning)

I membri sono identificati dalla **porta fisica dello switch** a cui sono connessi. Vantaggio: sicurezza più rigida, nessun accesso da porte non autorizzate. Svantaggio: se il dispositivo viene spostato su una porta diversa, perde l'accesso alla zona.

### Velocità di Trasmissione Fibre Channel

Le generazioni FC si sono evolute raddoppiando la velocità ogni ciclo, cambiando anche la codifica a partire da 16GFC:

| Generazione | Line Rate (Gb/s) | Codifica | Efficienza | Throughput reale |
| ----------- | ---------------- | -------- | ---------- | ---------------- |
| 1GFC        | 1,0625           | 8b/10b   | 77,7%      | 100 MB/s         |
| 2GFC        | 2,125            | 8b/10b   | 77,7%      | 200 MB/s         |
| 4GFC        | 4,25             | 8b/10b   | 77,7%      | 400 MB/s         |
| 8GFC        | 8,5              | 8b/10b   | 77,7%      | 800 MB/s         |
| 16GFC       | 14,025           | 64b/66b  | 94,2%      | 1.600 MB/s       |
| 32GFC       | 28,05            | 64b/66b  | 94,2%      | 3.200 MB/s       |
| 128GFC      | 4 × 28,05        | 64b/66b  | 94,2%      | 12.800 MB/s      |

Da 128GFC in poi la singola lane seriale non basta e si usano **4 canali paralleli** in un modulo **QSFP** (Quad Small Form-factor Pluggable), ciascuno a 28 Gb/s, per raggiungere 100 Gb/s aggregati. I connettori MPO (Multi-fiber Push-On) supportano più fibre ottiche in un singolo connettore.

### Servizi della Fabric FC

La fabric FC distribuita offre tre servizi fondamentali:

- **Management Service**: registrazione e discovery di switch, porte fabric e loro attributi; configuration management; zone management (configurazione e distribuzione delle zone); fabric device management; distribuito all'interno della fabric
- **Simple Name Service** (Name Server): directory service per il discovery di nodi e loro attributi connessi alla fabric; interrogato durante il PLOGI per scoprire tutti i target disponibili; integrato con la fabric e distribuito
- **Discovery Service**: discovery della topologia fisica e delle associazioni logiche tra dispositivi; acquisisce informazioni dal Name Server e dal Management Service

---

## Confronto

Le SAN si implementano con tre protocolli principali:

| Caratteristica           | Fibre Channel                    | iSCSI                           | FCoE                             |
| ------------------------ | -------------------------------- | ------------------------------- | -------------------------------- |
| **Trasporto**            | FC nativo                        | SCSI su TCP/IP/Ethernet         | FC su Ethernet lossless (DCB)    |
| **Latenza**              | Ultra-bassa (hardware)           | Moderata (TCP/IP overhead)      | Bassa                            |
| **Costo infrastruttura** | Alto (HBA FC, switch FC)         | Basso (rete Ethernet esistente) | Medio (NIC 10G DCB)              |
| **Lossless**             | Sì (BB Credits)                  | No (TCP ritrasmissioni)         | Sì (PFC - Priority Flow Control) |
| **Complessità**          | Alta (rete dedicata)             | Bassa                           | Media                            |
| **Uso tipico**           | Enterprise / banche / DC tier IV | PMI, cloud storage              | Convergenza rete+storage         |

**FCoE** permette di trasportare frame FC nativi su una rete Ethernet **lossless** (10G/25G/40G con Data Center Bridging - DCB), eliminando la rete FC fisica dedicata ma mantenendo la semantica FC; richiede NIC speciali chiamate **CNA** (Converged Network Adapter).
