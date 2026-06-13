---
tags:
  - mobile
  - reti
  - infrastruttura
  - internet
  - wireless
---

Le reti mobili si sono evolute con un ciclo generazionale di circa 10 anni, ognuna caratterizzata da un cambiamento radicale nella tecnica di accesso al mezzo. La tabella seguente riassume le principali caratteristiche:

| Gen    | Anno  | Standard principali | Tecnica di accesso   | Bit rate             |
| ------ | ----- | ------------------- | -------------------- | -------------------- |
| **1G** | ~1980 | AMPS, TACS          | FDMA analogico       | ~64 kb/s (voce)      |
| **2G** | ~1990 | GSM, GPRS, EDGE     | TDMA + FDMA          | 9.6 kb/s -> 384 kb/s |
| **3G** | ~2000 | UMTS, WCDMA, HSPA   | CDMA                 | 384 kb/s -> 42 Mb/s  |
| **4G** | ~2010 | LTE, LTE-Advanced   | OFDMA / SC-FDMA      | 100 Mb/s -> 1 Gb/s   |
| **5G** | ~2020 | NR (New Radio)      | OFDMA + Massive MIMO | fino a 20 Gb/s       |
| **6G** | ~2030 | AIAC, ISAC, UC      | AI-Native            | fino a 100 Gb/s      |

---

## 1G - Analogico (AMPS/TACS)

La prima generazione usava esclusivamente segnale analogico e trasmissione voce, esattamente come i vecchi [[Rete Telefonica#POTS (Plain Old Telephone System)|POTS]] (_Plain Old Telephone System_) su doppino di rame. L'accesso al mezzo era in **FDMA** (_Frequency Division Multiple Access_): a ogni utente veniva assegnata una frequenza dedicata di 30 kHz, con UL nella banda 824-849 MHz e DL nella banda 869-894 MHz. Le celle avevano un raggio tra 10 e 20 km e al loro centro si trovava una **Base Station (BS)** che collegava l'utente alla PSTN.

L'operatore manteneva un database con informazioni sugli abbonati, sulle configurazioni e sullo stato della rete (_network management_). La comunicazione tra i database garantisce l’indipendenza della posizione dell'utente dalla rete.

> [!danger] Impersonificazione
> Il sistema fu abbandonato principalmente per mancanza di sicurezza: il numero identificativo del dispositivo e il numero di telefono venivano trasmessi **in chiaro**, rendendo possibile la clonazione dei dispositivi.

---

## 2G - GSM (Global System for Mobile Communications)

Con il 2G si passa alla trasmissione **digitale**, introducendo l'accesso ibrido **TDMA** + **FDMA**: a ogni utente viene assegnato un _time slot_ su una portante radio. Le innovazioni principali rispetto all'1G sono:

- **SIM** (_Subscriber Identity Module_): identifica l'abbonato separatamente dal dispositivo fisico
- **SMS** (_Short Message Service_) e cifrazione della comunicazione
- **Roaming** automatico tra reti GSM di diversi operatori
- **Frequency Hopping (FH)** a 217 salti/s, FEC e interleaving per robustezza
- Modulazione **GMSK** (_Gaussian Minimum Shift Keying_)

### Struttura del frame GSM

La struttura temporale GSM è organizzata in frame da 4.615 ms, divisi in 8 slot da 577 μs, comprensivo di un guardband di 30 μs (8.25 bit). Ogni slot contiene 148 bit:

| Larghezza  | Uso                                          |
| ---------- | -------------------------------------------- |
| **3 bit**  | Delimitazione, sempre **0**                  |
| **57 bit** | Dati                                         |
| **1 bit**  | Controllo                                    |
| **26 bit** | Sincronizzazione del ricevitore (_training_) |
| **1 bit**  | Controllo                                    |
| **57 bit** | Dati                                         |
| **3 bit**  | Delimitazione, sempre **0**                  |

Il **raggio massimo** della cella è fissato a **31 km** non dalla potenza del segnale, bensì dalla tolleranza di sincronizzazione temporale (**100 μs**). Poiché le onde EM impiegano circa ~3.3 μs per percorrere un km, si ottiene: 100/3.3 ≈ 31 km.

Le bande operative sono:

- **GSM-900**: 890-915 MHz (UL) / 935-960 MHz (DL)
- **GSM-1800**: 1710-1785 MHz (UL) / 1805-1880 MHz (DL)

### GPRS ed EDGE (2.5G)

Quando fu necessario integrare la trasmissione dati a pacchetto sulla rete GSM, fu creato il **GPRS** (_General Packet Radio Service_), che aggiunge un dominio _packet switched_ alla rete GSM. I due nuovi nodi della core network sono:

- **SGSN** (_Serving GPRS Support Node_): instrada i pacchetti IP verso le stazioni base
- **GGSN** (_Gateway GPRS Support Node_): è il gateway verso Internet, assegna indirizzi IP dinamici

L'**EDGE** (_Enhanced Data Rates for GSM Evolution_) portò il bit rate fino a 384 kb/s, permettendo la transizione alla cosiddetta **2.5G**.

### Architettura Rete 2G

L'architettura è divisa in tre macro-blocchi:

#### Mobile Station (MS)

- **ME** (Mobile Equipment) identificato da codice **IMEI** (verificabile con `*#06#`)
- **SIM**: contiene PIN, PUK, chiave di cifratura Ki

#### Access Network (BSS)

- **BTS** (Base Transceiver Station): antenna, conversione segnale radio ↔ digitale
- **BSC** (Base Station Controller): assegna risorse, gestisce handover

#### Core Network (NS)

- **MSC** (Mobile Switching Center): commutazione circuitale per la voce
- **GMSC** (Gateway MSC): collegamento con la PSTN
- **VLR** (Visitor Location Register): posizione attuale dell'utente
- **HLR** (Home Location Register): profilo abbonato e ultima VLR nota
- **AuC** (Authentication Center): genera le chiavi di cifratura
- **EIR** (Equipment Identity Register): verifica il codice IMEI

---

## 3G - UMTS / WCDMA

La terza generazione rivoluziona l'accesso al mezzo abbandonando TDMA e FDMA in favore del [[Code Division Multiplexing#Code Division Multiple Access (CDMA)|CDMA]] (_Code Division Multiple Access_), in cui a ogni utente viene assegnato un codice ortogonale, non una frequenza o un time slot.

Il sistema usa una banda di **5 MHz per portante** (il CDMA2000 americano usava 1.25 MHz), con frequenze UL a 1920-1980 MHz e DL a 2110-2170 MHz.

> [!info] Parametri di WCDMA
> | Parametro | Valore |
> | -------------------- | -------------------------- |
> | Banda per portante | 5 MHz |
> | Chip rate | 384 Mcps (fisso) |
> | Spreading Factor | 4 - 512 (adattivo per QoS) |
> | Bit rate per utente | 15 kb/s - 960 kb/s |
> | Controllo potenza TX | 1500 aggiornamenti/s |
> | Duplexing | FDD |

> [!tip] Bit Rate
> Il **Bit Rate** si calcola dividendo il _Chip Rate_ per lo _Spreading Factor_.
>
> Uno Spreading Factor piccolo _aumenta_ il bit rate (fino a 960 kb/s).
>
> Uno Spreading Factor grande _diminuisce_ il bit rate (fino a 15 kb/s), ma maggiore robustezza all'interferenza.

A differenza del [[#2G - GSM (Global System for Mobile Communications)|GSM]], nel WCDMA tutte le celle riusano le stesse frequenze (no frequency planning), poiché gli utenti sono distinguibili tramite codici ortogonali.

### Soft Handover

Il 3G introduce il **Soft Handover**: mentre nel [[#2G - GSM (Global System for Mobile Communications)|GSM]] il passaggio tra celle causava una breve interruzione, perchè il dispositivo doveva rilasciare risorse fisiche e risincronizzarsi, con il CDMA il terminale mantiene attivi più codici simultaneamente, passando da una cella all'altra senza interruzione.

### HSPA e HSPA+ (3.5G)

L'evoluzione HSPA prolunga la vita del 3G con tre varianti:

| Standard | Direzione | Modulazione         | Velocità  |
| -------- | --------- | ------------------- | --------- |
| HSDPA    | Downlink  | 16-QAM + AMC + HARQ | 14.4 Mb/s |
| HSUPA    | Uplink    | E-DCH + HARQ        | 5.76 Mb/s |
| HSPA+    | DL/UL     | 64-QAM + MIMO 2×2   | 42 Mb/s   |

L'**HARQ** (Hybrid Automatic Repeat reQuest) è un meccanismo ibrido che combina ARQ e FEC: i pacchetti non corretti non vengono scartati, ma bufferizzati e ritrasmessi parzialmente.

### Architettura Rete 3G (UTRAN)

La Radio Access Network diventa **UTRAN** (Universal Terrestrial Radio Access Network):

- **Node B**: equivalente della BTS, converte dati digitali in segnale WCDMA
- **RNC** (Radio Network Controller): gestisce più Node B, coordina Soft Handover e cifratura

La Core Network mantiene la divisione in due domini:

- **CS Domain** (Circuit Switched): MSC/VLR, GMSC -> telefonia
- **PS Domain** (Packet Switched): SGSN, GGSN -> dati/Internet

---

## 4G - LTE (Long Term Evolution)

LTE rappresenta una svolta importante per l'evoluzione dei sistemi di telecomunicazioni mobile perchè **elimina completamente la commutazione di circuito**, operando esclusivamente su **pacchetti IP**. La voce è gestita tramite **VoLTE** (_Voice over LTE_) sull'**IMS** (_IP Multimedia Subsystem_).

### Caratteristiche principali

- Velocità fino a **150 Mb/s** (LTE) e **1 Gb/s** (LTE-Advanced)
- Latenza ridotta a **< 5 ms** per i pacchetti (idle->active < 50 ms)
- Mobilità ottimizzata per 0-15 km/h, supporto fino a 350 km/h (treni AV)
- Bande operative: 800, 1500, 1800, 2100, 2600 MHz con **banda variabile** 1.4/3/5/10/15/20 MHz
- Modulazione: [[Phase Shift Keying|QPSK]], [[Quadrature Amplitude Modulation|QAM]] 16, 64 e **256**
- MIMO **4×4** e MIMO multi-utente collaborativo

### Architettura EPC (Evolved Packet Core)

La Core Network LTE è l'**EPC**, un'evoluzione radicale rispetto all'UMTS che supporta esclusivamente commutazione a pacchetti. I nodi principali sono:

- **eNodeB** (evolved Node B): la stazione base, con funzionalità ampliate rispetto al Node B 3G
- **MME** (Mobility Management Entity): gestisce autenticazione, sessioni, mobilità
- **S-GW** (Serving Gateway): instrada i pacchetti tra eNodeB e core
- **P-GW** (Packet Data Network Gateway): assegna indirizzi IP, interfaccia con Internet
- **HSS** (Home Subscriber Server): evoluzione dell'HLR, database abbonati centralizzato

### Nodi EPC

#### MME (Mobility Management Entity)

L'**MME** è l'elemento principale del Control Plane, non trasporta traffico dati:

- Gestisce il signaling NAS per mobilità (EMM) e sessioni (ESM)
- Autentica l'UE tramite HSS e distribuisce le chiavi di sicurezza all'eNodeB
- Gestisce il **paging** (segnalazione verso UE in stato idle)
- Seleziona dinamicamente SGW e PGW ottimali

#### SGW (Serving Gateway)

Il nodo **SGW** è ancora locale del piano utente:

- Instrada i pacchetti tra E-UTRAN e PGW
- Bufferizza i pacchetti DL per UE in stato di risparmio energetico, notificando l'MME per attivare il paging

#### PGW (Packet Data Network Gateway)

II nodo **PGW** è il punto di terminazione verso Internet:

- Assegna indirizzi IPv4/IPv6 agli UE
- Applica le regole di QoS tramite QCI (QoS Class Identifier) e gestisce la tariffazione (PCRF)

#### HSS (Home Subscriber Server)

Il nodo **HSS** contiene il database centrale degli abbonati:

- Fornisce vettori di autenticazione all'MME
- Mantiene il profilo IMSI, gli APN autorizzati e l'MME corrente

### OFDM e OFDMA

LTE usa [[Orthogonal Frequency Division Multiplexing|OFDM]] (_Orthogonal Frequency Division Multiplexing_) per dividere la banda disponibile in sottoportanti da **15 kHz** tra loro ortogonali. L'ortogonalità è garantita dal fatto che la spaziatura tra le sottoportanti è uguale all'inverso del tempo di simbolo (66.67 μs).

[[Orthogonal Frequency Division Multiplexing#OFDMA - Orthogonal Frequency Division Multiple Access|OFDMA]] estende [[Orthogonal Frequency Division Multiplexing|OFDM]] all'accesso multiplo: invece di assegnare tutte le sottoportanti a un singolo utente, le risorse radio vengono ripartite in **Resource Block (RB)** assegnati dinamicamente:

- 1 RB = **12 sottoportanti × 1 slot** = 180 kHz × 0.5 ms = 84 Resource Elements
- Ogni slot contiene **7 simboli OFDM** (Normal CP) o **6 simboli** (Extended CP)

| Banda (MHz) | N. Resource Block | N. sottoportanti | Dim. DFT |
| ----------- | ----------------- | ---------------- | -------- |
| 1.4         | 6                 | 72               | 128      |
| 5           | 25                | 300              | 512      |
| 10          | 50                | 600              | 1024     |
| 20          | 100               | 1200             | 2048     |

**Vantaggi di OFDMA**:

1. **Resistenza al multipath fading selettivo**: se una frequenza è attenuata, si perdono solo i bit di quelle sottoportanti, recuperabili con [[Forward Error Correction|FEC]]
2. **Link Adaptation**: l'eNodeB evita di usare RB su frequenze disturbate per uno specifico utente
3. **Semplicità del ricevitore**: la demodulazione avviene tramite FFT, operazione nativa nei chip moderni

### Prefisso Ciclico

Il [[Orthogonal Frequency Division Multiplexing#Prefisso Ciclico (CP)|Prefisso Ciclico]] viene inserito prima di ogni simbolo [[Orthogonal Frequency Division Multiplexing|OFDM]] come _guard interval_ per evitare l'**ISI** (_Inter-Symbol Interference_) causata dal multipath:

- **Normal CP**: 4.69-5.21 μs -> 7 simboli/slot (celle piccole)
- **Extended CP**: 16.67 μs -> 6 simboli/slot (celle grandi, maggiori ritardi multipath)

### SC-FDMA

Nell'uplink LTE si usa [[Orthogonal Frequency Division Multiplexing#SC-FDMA|SC-FDMA]] (_Single Carrier FDMA_) invece di [[Orthogonal Frequency Division Multiplexing#OFDMA - Orthogonal Frequency Division Multiple Access|OFDMA]] per ridurre il **PAPR (Peak-to-Average Power Ratio)** e risparmiare la batteria dei dispositivi.

|                      | OFDMA (DL) | SC-FDMA (UL) |
| -------------------- | ---------- | ------------ |
| Efficienza spettrale | Alta       | Alta         |
| PAPR                 | Elevato    | Basso        |
| Consumo batteria     | Alto       | Ridotto      |
| Complessità TX       | Minore     | Maggiore     |

### FDD vs TDD in LTE

LTE supporta entrambe le modalità di duplexing:

- **FDD** (Frequency Division Duplex): bande separate per UL e DL, trasmissione simultanea e continua, ideale per traffico simmetrico (voce)
- **TDD** (Time Division Duplex): stessa banda con slot temporali alternati, allocazione flessibile UL/DL, ideale per traffico asimmetrico (streaming)

Struttura temporale: **Radio Frame = 10 ms**, diviso in **10 Subframe da 1 ms**, ognuno composto da **2 Slot da 0.5 ms**.

In TDD il subframe speciale (S) contiene: **DwPTS** (con P-SS), **Guard Period (GP)** e **UpPTS** (con PRACH e SRS).

### Interfacce principali

| Interfaccia | Tra             | Funzione                                         |
| ----------- | --------------- | ------------------------------------------------ |
| **Uu**      | UE ↔ eNodeB     | Air interface (PHY/MAC/RLC/PDCP/RRC)             |
| **X2**      | eNodeB ↔ eNodeB | Handover diretto senza passare per il core       |
| **S1-MME**  | eNodeB ↔ MME    | C-Plane: segnalazione S1AP su SCTP               |
| **S1-U**    | eNodeB ↔ SGW    | U-Plane: tunneling GTP-U su UDP/IP               |
| **S11**     | MME ↔ SGW       | Controllo: creazione/modifica tunnel             |
| **S5/S8**   | SGW ↔ PGW       | S5 (stesso operatore), S8 (roaming); usa GTPv2-C |
| **S6a**     | MME ↔ HSS       | Autenticazione e profili abbonati                |
| **SGi**     | PGW ↔ Internet  | Interfaccia verso la rete dati pubblica          |

L'interfaccia **X2** è fondamentale per gli handover rapidi: gli eNodeB si coordinano direttamente tra loro senza coinvolgere il core, riducendo la latenza del handover.

### EPS Bearer

Il modello di qualità del servizio di LTE si basa sul concetto di **EPS Bearer**: un canale logico con caratteristiche di QoS garantite tra UE e PGW. Ogni bearer è identificato da un **QCI** (_QoS Class Identifier_) che codifica: priorità di scheduling, **PDB** (_Packet Delay Budget_), PELR (Packet Error Loss Rate) e tipo GBR/non-GBR.

Esistono due tipi di bearer:

- **Default Bearer** (non-GBR): instaurato automaticamente alla registrazione, fornisce connettività best-effort (navigazione web, email)
- **Dedicated Bearer** (GBR): creato su richiesta per applicazioni che necessitano QoS specifica, es. VoLTE (QCI=1, PDB=100 ms), gaming real-time

### Procedura di Attach LTE

Quando un dispositivo si accende e si connette alla rete LTE, segue questa sequenza:

1. **Cell Search**: ricerca delle frequenze e sincronizzazione con il segnale DL dell'eNodeB tramite segnali **PSS** (Primary Synchronization Signal) e **SSS** (Secondary Synchronization Signal)
2. **Cell Selection**: acquisizione del **MIB** (Master Information Block) e **SIB** (System Information Block) trasmessi sul canale PBCH; contengono il Physical Cell ID e i parametri di sistema
3. **Random Access**: il UE trasmette un **preamble** sull'uplink; se l'eNodeB risponde, la connessione è approvata; altrimenti il preamble viene ritrasmesso più volte
4. **RRC Connection**: instaurazione della connessione radio tra UE e eNodeB
5. **Autenticazione**: il MME interroga l'HSS per autenticare l'utente e negoziare le chiavi di sicurezza
6. **Bearer Establishment**: creazione del canale logico EPS tra UE e Internet; l'UE riceve un indirizzo IP dal PGW

## LTE-Advanced (4G vero)

L'LTE base (Release 8) non soddisfaceva i requisiti ITU per il 4G vero (1 Gbps). L'**LTE-Advanced** (Release 10) introduce tre tecnologie chiave:

### Carrier Aggregation (CA)

Aggrega fino a **5 Component Carrier (CC)** da 20 MHz ciascuno, raggiungendo una banda totale di **100 MHz**. Le CC possono essere nelle stesse bande (intra-band) o in bande diverse (inter-band). Con CA, MIMO 8×8 e 256-QAM si raggiungono teoricamente **2 Gbps**.

### CoMP - Coordinated Multi-Point

Permette a **più eNodeB coordinati** di trasmettere verso lo stesso UE simultaneamente, riducendo le interferenze intercellulari e migliorando le prestazioni ai bordi della cella. Funziona sia in DL (Joint Transmission) che in UL (Reception CoMP).

### Reti Eterogenee (HetNet)

LTE-Advanced introduce celle di dimensioni sempre minori per aumentare la densità di copertura:

| Tipo cella | Raggio | Potenza TX  | Uso tipico                       |
| ---------- | ------ | ----------- | -------------------------------- |
| Macrocella | ~km    | Alta        | Copertura rurale/urbana generale |
| Microcella | ~200 m | Media       | Aree urbane dense                |
| Picocella  | ~50 m  | Bassa       | Indoor, hotspot                  |
| Femtocella | ~10 m  | Molto bassa | Residenziale/ufficio             |

## LTE-Advanced Pro (4.5G)

Introduce tecnologie ponte verso il 5G:

- **Massive MIMO / FD-MIMO**: fino a 64 antenne attive, beamforming 3D (Full Dimension MIMO) con MU-MIMO
- **256-QAM in uplink**: maggiore efficienza spettrale in condizioni di canale favorevole
- **LAA (License Assisted Access)**: utilizzo dello spettro non licenziato a 5 GHz con meccanismo LBT (Listen Before Talk)
- **NB-IoT ed eMTC**: tecnologie per IoT a basso consumo e costo ridotto
- **D2D (Device-to-Device)**: comunicazione diretta tra terminali (proximity services - ProSe)

| Parametro               | LTE (R8)    | LTE-A (R10)  |
| ----------------------- | ----------- | ------------ |
| Throughput DL picco     | 150 Mb/s    | 1 Gb/s       |
| Throughput UL picco     | 75 Mb/s     | 500 Mb/s     |
| Banda massima           | 20 MHz      | 100 MHz (CA) |
| Latenza RTT             | ~10–15 ms   | <10 ms       |
| Efficienza spettrale DL | ~4 bit/s/Hz | ~8 bit/s/Hz  |
| Antenne                 | 4×4 MIMO    | 8×8 MIMO     |

### Stack Protocollare RAN (LTE)

La Radio Access Network è fisicamente divisa in due unità:

- **Radio Unit (RU)**: gestisce la parte RF analogica, la conversione A/D e D/A
- **Baseband Unit (BBU)**: elabora digitalmente i segnali, ospita tutti i livelli protocollari

I protocolli sono organizzati in stack, separati in **Control Plane** (segnalazione) e **User Plane** (dati):

| Livello | Acronimo | Funzione principale                                                  |
| ------- | -------- | -------------------------------------------------------------------- |
| L1      | **PHY**  | Modulazione OFDMA/SC-FDMA, MIMO, controllo potenza, AMC              |
| L2      | **MAC**  | Scheduling dinamico, HARQ, multiplexing canali logici                |
| L2      | **RLC**  | Segmentazione/riassemblaggio PDU, ARQ, riordino sequenze             |
| L2      | **PDCP** | Cifratura, integrità, compressione header IP (ROHC)                  |
| L3      | **RRC**  | Assegnazione risorse radio, handover, messaggi di sistema, sicurezza |
| L3      | **NAS**  | Interfaccia diretta UE <--> MME; autenticazione e gestione sessioni  |

Il **NAS (Non-Access Stratum)** è particolare: non transita per l'eNodeB ma costituisce un tunnel di segnalazione diretto tra lo User Equipment e il **MME** nella Core Network.

#### Scheduling e Assegnazione delle Risorse

L'algoritmo di scheduling dell'eNodeB decide **ogni millisecondo** a quale UE assegnare i Resource Block, basandosi su tre criteri principali:

- **CQI (Channel Quality Indicator)**: feedback riportato dall'UE che indica la qualità del canale radio per ciascun gruppo di frequenze
- **QoS e priorità del Bearer**: traffico GBR (Guaranteed Bit Rate, es. VoLTE) ha priorità su traffico non-GBR (es. navigazione web)
- **Fairness tra utenti**: algoritmi come **Proportional Fair** bilanciano l'efficienza spettrale complessiva con l'equità di accesso tra tutti gli utenti connessi

---

## MIMO (Multiple Input Multiple Output)

MIMO utilizza antenne multiple sia in trasmissione che in ricezione, sfruttando il **multipath per interferenza costruttiva** invece di combatterlo. Le tre modalità operative sono:

- **Spatial Multiplexing**: trasmette flussi dati indipendenti su ogni antenna, moltiplicando il throughput (MIMO $4\times4$ -> throughput x 4)
- **Beamforming**: usa le antenne per formare un fascio direzionale, migliorando il rapporto segnale/rumore
- **Transmission Diversity**: usa codici spazio-temporali per aumentare l'affidabilità

Le configurazioni antenna sono: SISO ($1\times1$), SIMO ($1\times \text{N}$), MISO ($\text{N}\times1$), MIMO ($\text{N}\times\text{M}$).

---

## HARQ - Hybrid Automatic Repeat reQuest

HARQ è il meccanismo di correzione degli errori a livello MAC, che combina due approcci:

- [[Forward Error Correction|FEC]] (_Forward Error Correction_): aggiunge ridondanza al pacchetto trasmesso per permettere la correzione autonoma di alcuni errori senza ritrasmissione
- [[Automatic Repeat Request|ARQ]] (_Automatic Repeat reQuest_): richiede la ritrasmissione quando gli errori non sono correggibili

La differenza chiave rispetto al semplice ARQ è che in HARQ **i pacchetti errati non vengono scartati ma bufferizzati**. Quando arriva la ritrasmissione, il ricevitore **combina** le due versioni (Chase Combining o Incremental Redundancy), migliorando progressivamente la probabilità di decodifica corretta. Questo riduce il numero totale di ritrasmissioni e abbassa significativamente la latenza effettiva.

$$
\begin{align}
\text{ARQ classico}:\quad & (\text{scarta e ritrasmette})\vphantom{\int}\\
&P_1a \rightarrow \text{NACK} \rightarrow P_1a \rightarrow \text{NACK} \rightarrow P_1a \rightarrow \text{ACK} \\
\text{HARQ}:\quad & (\text{combina P1a+P1b+P1c})\vphantom{\int}\\
&P_1a \rightarrow \text{NACK} \rightarrow P_1b \rightarrow \text{NACK} \rightarrow P_1c \rightarrow \text{ACK}
\end{align}
$$

---

## 5G NR - New Radio

Il 5G migliora il 4G su tutti i parametri chiave e introduce nuovi paradigmi architetturali:

| Parametro IMT-2020    | LTE-A    | 5G NR                   |
| --------------------- | -------- | ----------------------- |
| Peak data rate DL     | ~3 Gbps  | **20 Gbps**             |
| User-experienced rate | ~10 Mb/s | **100 Mb/s**            |
| Latenza               | ~10 ms   | **1 ms**                |
| Efficienza spettrale  | base     | **3× rispetto a LTE-A** |
| Densità dispositivi   | ~10⁵/km² | **10⁶/km²**             |
| Mobilità              | 350 km/h | **500 km/h**            |
| Efficienza energetica | base     | **100× migliore**       |

> [!info] Frequenze 5G in Italia
>
> Il 5G opera su **tre bande** con caratteristiche molto diverse:
>
> | Banda             | Frequenza | Bit rate     | Raggio cella | Note                           |
> | ----------------- | --------- | ------------ | ------------ | ------------------------------ |
> | **Bassa**         | 700 MHz   | ~100 Mb/s    | Decine di km | Penetrazione edifici ottima    |
> | **Media**         | 3.7 GHz   | 100–900 Mb/s | Pochi km     | Compromesso capacità/copertura |
> | **Alta (mmWave)** | 26 GHz    | ~1 Gbps      | Decine di m  | Ostacolata da muri e finestre  |

### Numerologia Scalabile 5G

Una delle innovazioni fondamentali del 5G NR è la **numerologia scalabile**: la spaziatura tra le sottoportanti non è fissa come in LTE (15 kHz), ma varia secondo la formula:

$$
\Delta f = 15 \times 2^{\mu} \text{ kHz}
$$

dove $\mu$ è l'indice di numerologia. All'aumentare di $\mu$, la spaziatura raddoppia e la durata del simbolo si dimezza. Ogni slot contiene sempre **14 simboli OFDM**, ma la durata dello slot cambia:

| μ   | SCS (kHz) | Durata simbolo (μs) | Durata slot (ms) | Uso principale                       |
| --- | --------- | ------------------- | ---------------- | ------------------------------------ |
| 0   | 15        | 66.67               | 1                | Bande basse, celle grandi (come LTE) |
| 1   | 30        | 33.3                | 0.5              | Bande medie (3.5 GHz), eMBB          |
| 2   | 60        | 16.7                | 0.25             | Bande medie/alte, URLLC              |
| 3   | 120       | 8.3                 | 0.125            | Onde millimetriche (mmWave)          |
| 4   | 240       | 4.2                 | 0.0625           | Segnali di sincronizzazione speciali |

> [!tip] Vantaggi
>
> - **Effetto Doppler**: ad alte frequenze o con dispositivi in rapido movimento (treni), allargare la spaziatura rende il segnale più robusto alle distorsioni
> - **Latenza**: con μ=3, lo slot dura solo 0.125 ms -> il dispositivo può trasmettere quasi istantaneamente, fondamentale per URLLC
> - **Flessibilità per slot dinamici**: ogni slot può avere simboli configurati dinamicamente come DL, UL o _Flexible_ (guard period), tramite lo **SFI** (_Slot Format Indicator_) trasmesso nel canale di controllo

### Network Slicing

Il network slicing permette di **partizionare virtualmente la stessa infrastruttura fisica** in più reti logiche indipendenti, ciascuna ottimizzata per un tipo di servizio:

- **RAN slicing**: il gNodeB alloca dinamicamente i Physical Resource Block tra le diverse slice
- **Transport slicing**: i pacchetti viaggiano sulla rete di trasporto IP/Ottica tramite **VPN** [[MPLS]]
- **Core slicing**: il core 5G istanzia funzioni di rete (NF) dedicate o condivise per ciascuna slice

Un esempio concreto: sulla stessa torre 5G possono coesistere simultaneamente:

- Una slice per **smartphone** (alta banda, eMBB)
- Una slice per **veicoli autonomi** (latenza <1 ms, URLLC)
- Una slice per **sensori IoT** (basso consumo, mMTC)

### SDN - Software Defined Networking

Il 5G adotta l'architettura **SDN** che separa nettamente **Control Plane** e **Data Plane**:

- **Data Plane**: nodi di commutazione (fisici o virtuali) che eseguono operazioni primitive su Flow Table (Forward, Drop, Modify Header) tramite ASIC
- **Control Plane**: controller centralizzato con visione globale della topologia; calcola i percorsi ottimali e programma i nodi
- **Application Layer**: implementa logiche complesse come policy di sicurezza, bilanciamento del carico, orchestrazione delle _Network Slices_

Questo approccio trasforma l'hardware di rete in **nodi programmabili** gestibili via software, rendendo la rete dinamica e adattabile.

### Architettura 5G Core - Service Based Architecture (SBA)

Il core 5G sostituisce l'EPC con una **Service Based Architecture (SBA)**, dove le funzioni di rete sono **microservizi** che si espongono tramite interfacce REST/HTTP. Le funzioni principali sono:

**AMF (Access and Mobility Management Function)** - sostituisce l'MME:

- Termina il signaling NAS, gestisce registrazione, autenticazione e mobilità UE
- Gestisce il security context e la cifratura NAS
- Supporta più tecnologie di accesso (5G NR, WiFi, ecc.)

**SMF (Session Management Function)** - sostituisce parte di MME/SGW/PGW:

- Crea, modifica e rimuove sessioni PDU
- Alloca indirizzi IP agli UE (funzione DHCP)
- Seleziona e controlla la UPF; abilita il **Mobile Edge Computing** scegliendo una UPF vicina al bordo della rete

**UPF (User Plane Function)** - sostituisce SGW+PGW per il traffico dati:

- Instradamento e inoltro dei pacchetti, ispezione e QoS enforcement
- **Totalmente disaccoppiata dal Control Plane**: può essere posizionata ai bordi della rete (Edge Computing), minimizzando il Round Trip Time

**AUSF (Authentication Server Function)**: autentica gli UE e gestisce le chiavi

**UDM (Unified Data Management)**: equivalente dell'HSS, database degli abbonati

**NSSF (Network Slice Selection Function)**: seleziona la slice di rete appropriata per ogni UE

Il disaccoppiamento tra AMF/SMF (control) e UPF (data) è la chiave per il **Multi-access Edge Computing (MEC)**: l'UPF può essere istanziata vicino all'utente, abbattendo la latenza a livelli non raggiungibili con l'architettura LTE.

### Massive MIMO

Le stazioni base 5G (gNB) integrano pannelli con decine o centinaia di elementi radianti, come configurazioni **32T32R** o **64T64R** (64 canali in trasmissione e 64 in ricezione). L'evoluzione dell'antenna è passata da:

1. **Macro BS tradizionale**: antenna passiva separata dalla Baseband Unit (BBU)
2. **Distributed BS**: Remote Radio Unit (RRU) vicina all'antenna, BBU separata
3. **AAU (Active Antenna Unit)**: radio e antenne integrate in un unico pannello, usato nel 5G

Con la **FD-MIMO (Full Dimension MIMO)**, variando la fase e l'ampiezza di ciascun elemento del phased array, si formano fasci sia sul piano verticale (**elevation beamforming**) che orizzontale (**azimuth beamforming**), coprendo lo spazio tridimensionale e servendo contemporaneamente più UE con **MU-MIMO**.

---

## Field Test

Per accedere ai parametri tecnici reali della rete a cui si è connessi:

- **iPhone**: codice `*3001#12345#*` -> Field Test Mode
- **Android**: codice `*#*#4636#*#*` -> Informazioni telefono

I parametri principali visualizzati sono:

| Parametro | Descrizione                                                          | Valori tipici                         |
| --------- | -------------------------------------------------------------------- | ------------------------------------- |
| **Band**  | Banda di frequenza fisica (es. n78 = 3.5 GHz, n28 = 700 MHz)         | -                                     |
| **PCI**   | Physical Cell ID: identificativo fisico dell'antenna                 | 0–503                                 |
| **RSRP**  | Potenza del segnale ricevuto                                         | -70 dBm (ottimo) -> -110 dBm (scarso) |
| **RSRQ**  | Qualità del segnale, tiene conto del rumore                          | -                                     |
| **SINR**  | Rapporto segnale/rumore+interferenza; determina la modulazione usata | >20 dB -> 256-QAM                     |

Un SINR alto (>15–20 dB) permette all'eNodeB/gNB di utilizzare modulazioni complesse come **256-QAM**, massimizzando l'efficienza spettrale e la velocità di connessione.
