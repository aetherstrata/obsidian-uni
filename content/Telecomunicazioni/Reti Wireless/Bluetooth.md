---
aliases:
  - BT
tags:
  - wireless
  - reti
  - iot
  - protocollo
---

Il Bluetooth nasce come tecnologia per le _Personal Area Network_ (**PAN**), cioè reti di breve raggio concepite per sostituire i cavi e connettere dispositivi smart senza infrastruttura fissa. Si colloca nel più ampio contesto dell'_Internet of Things_ (**IoT**), dove sensori e dispositivi trasmettono dati in modo quasi autonomo, occupando circa il **40% del mercato delle telecomunicazioni wireless** nelle applicazioni PAN/LAN.

I protocolli wireless per IoT si distinguono principalmente per range, data rate e consumo energetico:

| Tecnologia         | Range        | Data Rate       | Banda           | Uso tipico               |
| ------------------ | ------------ | --------------- | --------------- | ------------------------ |
| Bluetooth Classic  | 10-100 m     | 1-3 Mb/s        | 2,4 GHz ISM     | Audio, dati, periferiche |
| Bluetooth LE (BLE) | 10-400 m     | 125 kb/s-2 Mb/s | 2,4 GHz ISM     | Sensori, beacon, IoT     |
| Wi-Fi              | <100 m       | fino a 9,6 Gb/s | 2,4 / 5 / 6 GHz | LAN wireless             |
| LoRaWAN            | decine di km | ~50 kb/s        | <1 GHz ISM      | Smart city, agricoltura  |

La **banda ISM** (_Industrial, Scientific and Medical_) a 2,4 GHz è libera da licenza in tutti i paesi: questo spiega perché sia Bluetooth che Wi-Fi la condividano, rendendo necessaria la coesistenza.

## Standard e Versioni Bluetooth

Il Bluetooth è standardizzato come **IEEE 802.15.1** ed è gestito dal **Bluetooth SIG** (_Special Interest Group_), un consorzio di aziende fondato a fine anni '90.

> [!info] Trivia
> Il nome viene dal re vichingo Harald Blåtand (_"Bluetooth"_), che unificò le tribù scandinave - metafora della capacità del protocollo di unire dispositivi diversi.

Le versioni principali sono:

| Versione  | Anno      | Innovazione chiave                                                   |
| --------- | --------- | -------------------------------------------------------------------- |
| 1.x       | 1999-2003 | Basic Rate (BR), 1 Mb/s                                              |
| 2.x / 3.x | 2004-2009 | Enhanced Data Rate (EDR), fino a 3 Mb/s                              |
| **4.0**   | **2010**  | **Bluetooth Low Energy (BLE)** - protocollo incompatibile con BR/EDR |
| 5.x       | 2016-2023 | BLE 2 Mb/s, range esteso, mesh, localizzazione                       |
| **6.0**   | **2024**  | 3 Mb/s Classic, direzione segnale, canale da 300 m                   |

> [!caution] Protocolli incompatibili
> Bluetooth Classic (BR/EDR) e BLE sono **due protocolli fisicamente incompatibili**. I dispositivi moderni implementano entrambi, ma si tratta di stack radio distinti.

---

## Bluetooth Classic (BR/EDR)

### Topologia

#### Piconet

Una **piconet** è la rete base del Bluetooth Classic, con topologia **obbligatoriamente a stella**:

- **1 Master**: il dispositivo con più risorse (es. smartphone), che controlla tutti gli accessi.
- **Fino a 7 Slave attivi**: identificati da un _Active Member Address_ (**AMADDR**) a 3 bit assegnato dal master.
- **Fino a 256 Parked Slave**: dispositivi inattivi ma parcheggiati nella rete, riattivabili in ~2 ms con un _Parked Member Address_ (**PMADDR**) a 8 bit.

#### Scatternet

Una **scatternet** è l'unione di più piconet: un dispositivo può partecipare a piconet multiple, ma solo come **slave** in ciascuna.

![[Pasted image 20260622223533.png]]

### Modulazione

Il Bluetooth Classic supporta due modalità:

- **Basic Rate (BR):** modulazione **GFSK** (Gaussian Frequency Shift Keying). Si trasmettono due segnali sinusoidali a frequenze $f_1$ e $f_2$​ (separate di $\pm185$ kHz dalla portante) per rappresentare bit 0 e bit 1, filtrati con un filtro gaussiano per limitare l'occupazione di banda. Il bit rate è **1 Mb/s** (1 bit per simbolo, durata del simbolo = 1 μs).
- **Enhanced Data Rate (EDR):** per aumentare il throughput, si usano costellazioni di fase più dense:
  - **π/4-DQPSK** -> 2 bit per simbolo -> **2 Mb/s**
  - **8-DPSK** -> 3 bit per simbolo -> **3 Mb/s**
  - L'header del pacchetto è sempre trasmesso in GFSK, così il ricevitore capisce quale modulazione aspettarsi nel payload.

### Accesso al mezzo

L'accesso è regolato da _Time Division Duplexing_ (**TDD**). Il full-duplex non è possibile perché master e slave condividono l'intera banda:

- Ogni **time slot** dura **625 μs** (scelto appositamente: 1/625 μs = 1600 slot/s, equivalente a 8000 simboli/s per la voce PCM a 64 kb/s).
- Un pacchetto può durare **1, 3 o 5 slot** (sempre dispari).
- Il **master trasmette negli slot pari** (0, 2, 4, ...), gli **slave negli slot dispari** (1, 3, 5, ...).
- Solo lo slave che ha appena ricevuto un pacchetto dal master può rispondere nello slot successivo.

![[Pasted image 20260622224626.png]]

### Frequency Hopping Spread Spectrum (FHSS)

Per convivere con il Wi-Fi (che usa la stessa banda ISM a 2,4 GHz), Bluetooth usa il **FHSS** - tecnica inventata da **Hedy Lamarr** durante la Seconda Guerra Mondiale per comunicazioni militari sicure, evitando le intercettazioni:

- Banda disponibile: **2402 MHz <--> 2480 MHz**
- **79 canali** da 1 MHz ciascuno: $f_c = 2402 + k \times 1$ MHz, con $k = 0, 1, \ldots, 78$
- La frequenza cambia ad **ogni fine trasmissione di pacchetto**, in modo pseudo-casuale.
- La sequenza di hopping è generata da un algoritmo **Pseudo-Random** che usa come seed:
  - **UAP** (_Upper Address Part_) e **LAP** (_Lower Address Part_) del MAC del master
  - I **bit 1-26 del clock Bluetooth** (contatore a 28 bit, incrementato ogni 312,5 μs, ciclo ~23,3 ore)
- Il master comunica questi parametri agli slave durante la connessione per sincronizzare tutti.

![[Pasted image 20260622224734.png]]

| Durata pacchetto | Frequenza di cambio canale |
| ---------------- | -------------------------- |
| 1 slot (625 μs)  | 1600 volte/s               |
| 3 slot (1875 μs) | ~533 volte/s               |
| 5 slot (3125 μs) | 320 volte/s                |

### Adaptive Frequency Hopping (AFH)

Il master monitora continuamente la qualità dei canali tramite: **RSSI** (Received Signal Strength Indication), **PER/BER** (Packet/Bit Error Rate), **SNR**. I canali considerati degradati (_"bad channels"_) vengono marcati ed esclusi dalla sequenza di hopping, tipicamente quelli sovrapposti ai canali Wi-Fi (es. canale Wi-Fi 6 a 2437 MHz con bandwidth 20 MHz).

![[Pasted image 20260622225133.png]]

Il clock (**CLK**) è realizzato con un contatore a 28 bit che viene posto a 0 all'accensione del dispositivo e si incrementa ogni 312.5μs (metà slot). Il ciclo del contatore copre la durata di un giorno: $312.5\text{ μs} \cdot 2^{28} = 23.3 \text{ h}$.

Ogni dispositivo Bluetooth ha il suo native clock. Tutte le unità attive (_slave_) nella _piconet_ devono sincronizzare il proprio native clock con quello del _master_ aggiungendo un offset.

### Indirizzo MAC e Access Code

Come tutti i protocolli IEEE 802, ogni dispositivo Bluetooth ha un indirizzo MAC (**BD_ADDR**) a **48 bit** (EUI-48), espresso in 6 coppie esadecimali (es. `00:11:22:33:FF:EE`):

- **NAP** (_Non-significant Address Part_) - 2 byte, MSB
- **UAP** (_Upper Address Part_) - 1 byte -> usato per seeding FHSS
- **LAP** (_Lower Address Part_) - 3 byte, assegnato univocamente dal produttore -> parte dell'**Access Code** in ogni frame; usato per seeding FHSS

Il **LAP del master** genera il **Channel Access Code (CAC)** che identifica la _piconet_. Esistono tre tipi di access code:

- **CAC** (_72 bit_): identifica la piconet
- **DAC** (_68 o 72 bit_): identifica uno slave, usato nel paging
- **IAC** (_68 o 72 bit_): valore fisso `0x9E8B33`, usato per l'inquiry broadcast

### Tipi di Connessione

Bluetooth Classic supporta due tipi di link logico:

#### ACL (Asynchronous Connection-Less)

Principalmente usato per traffico dati:

- Commutazione di pacchetto (best-effort)
- Velocità simmetrica: 108-433 kb/s; asimmetrica fino a **723 kb/s**
- ARQ e CRC attivi; ritrasmissione automatica
- Multipoint: un master può avere più link ACL con slave diversi

#### SCO (Synchronous Connection-Oriented)

Principalmente usato per voce/audio:

- Commutazione di circuito, slot riservati a intervalli fissi
- Velocità fissa: **64 kb/s** (PCM, 8000 simboli/s × 8 bit)
- **Niente [[Automatic Repeat Request|ARQ]] né [[Cyclic Redundancy Check|CRC]]** (la voce non tollera la ritrasmissione); [[Forward Error Correction|FEC]] opzionale
- Uno slave può avere fino a **3 connessioni SCO** con il master

### Formato dei Pacchetti BR/EDR

Struttura base di un pacchetto **Basic Rate (5 slot, 1 Mb/s, GFSK)**:

![[Pasted image 20260622234234.png]]

Struttura pacchetto **EDR**:

![[Pasted image 20260622234313.png]]

L'access code e l'header sono **sempre in GFSK a 1 Mb/s**; il payload EDR usa DQPSK o 8DPSK dopo il guard interval.

Il **Baseband Header** (18 bit -> 54 con FEC 1/3) contiene:

- **AMADDR** (3 bit): identifica lo slave destinatario
- **PDU Type** (4 bit): tipo di pacchetto (NULL, POLL, SCO, ACL) Inoltre, indica il tipo di correzione degli errori utilizzata e il numero di slot (1, 3 o 5) di cui è composto il frame.
- **FLOW, ARQN, SEQN** (1 bit ciascuno): controllo flusso e ARQ
- **HEC** (8 bit): Header Error Check

> [!important] Bassa efficienza
> **Efficienza del 13%**: 41% della capacità utilizzata per il setting, il 20% per le intestazioni e 26% redundancy coding.

### Procedura di Connessione (Classic)

La connessione avviene in due fasi:

1. **Inquiry**: il master fa un broadcast con l'**IAC** su 32 canali (non tutti 79), cambiando frequenza ogni 312,5 μs (metà slot). Trasmette su due treni di 16 canali, ognuno ripetuto 256 volte. Tempo massimo di inquiry: ~**10,24 secondi**. Gli slave rispondono con il loro DAC e il loro clock.

2. **Page**: il master invia un invito specifico allo slave selezionato. Lo slave risponde e riceve:
   - Il **CAC** (Channel Access Code, identità della piconet)
   - Il **clock del master** (per calcolare l'offset e sincronizzarsi)
   - Il proprio **AMADDR** (3 bit) per partecipare attivamente

### ARQ e Meccanismo di Affidabilità

Il meccanismo **ARQ (Automatic Repeat reQuest)** per i link ACL usa i flag:

- **ARQN = 1** (ACK): pacchetto ricevuto correttamente -> il master trasmette il successivo
- **ARQN = 0** (NAK): errore -> ritrasmissione dello stesso pacchetto
- La ritrasmissione continua fino al timeout (supervisione della connessione)

Tramite il flag ARQN viene realizzato il meccanismo di ARQ (solo per i link ACL, mentre invece i pacchetti di un link SCO non vengono mai ritrasmessi).

Quando lo slave riceve correttamente il pacchetto inviatogli dal master, pone il bit ARQN a 1 nel pacchetto che esso manda in risposta al master.

Basandosi su questa informazione, il master decide se trasmettere un nuovo pacchetto o ritrasmettere lo stesso. Il master continua a ritrasmettere lo stesso pacchetto finché non riceve un’indicazione sulla corretta ricezione del pacchetto, oppure finché non scade un timeout.

### Stati di operazione

- **Standby**: In attesa di entrare a far parte di una piconet.
- **Inquire**: Il master broadcast una richiesta di informazioni. Gli slave rispondono con il loro DAC e il loro clock, in maniera che il master calcola l’offset.
- **Page**: Il master invia un messaggio ad uno slave. Se riceve una risposta, invia il suo clock e CAC, in modo che lo slave possa ricevere l’_Active Member Address_ (**AM_ADDR**) (3 bit) e si connette alla piconet. Lo slave calcola il clock offset.
- **Connected**: Dispositivo (master or slave) attivo.
- **Hold**: solo la connessione SCO può continuare. Il canale audio rimane attivo, ma non viene trasmesso altro traffico.
- **Sniff**: ascolta ad intervalli fissi. Conserva l’**AM_ADDR**.
- **Park**: periodicamente ascolta i beacon. Riceve il _Parked Member Address_ (**PAM_ADDR**) (8 bit).

## Stack di Protocolli BT Classic

Il Bluetooth Classic ha uno stack complesso, eredità del processo SIG multi-azienda:

`[ Audio Apps | vCal/vCard | TCP/UDP Apps | Telephony | Management ]     |               |            |              |             |  [ OBEX ]    [ AT cmds ]  [ TCS-BIN ]   [ SDP ]         [ IP ]                    |                                      [ PPP/BNEP ]                    +---> [ RFCOMM (serial line interface) ] <--+                                    |                    [ L2CAP - Logical Link Control & Adaptation ]                                    |                    [ LMP - Link Manager Protocol ]                                    |                    [ HCI - Host Controller Interface ]                                    |                    [ Baseband / Radio ]`

I livelli principali: **Baseband** (PHY + MAC), **LMP** (gestione link), **L2CAP** (multiplexing e segmentazione), **RFCOMM** (emulazione porte seriali), **SDP** (Service Discovery Protocol).

---

## Bluetooth Low Energy (BLE)

### Caratteristiche Generali

BLE (introdotto con Bluetooth 4.0 dalla Nokia nel 2010) è un protocollo **completamente diverso** dal Classic, riprogettato da zero per dispositivi a batteria con trasmissioni sporadiche di piccole quantità di dati.

Caratteristiche chiave:

| Parametro     | Bluetooth Classic | BLE                         |
| ------------- | ----------------- | --------------------------- |
| Canali RF     | 79 × 1 MHz        | **40 × 2 MHz**              |
| Data rate     | 1-3 Mb/s          | 125 kb/s - 2 Mb/s           |
| Range         | 10-100 m          | 10-400 m                    |
| Consumo picco | ~30 mA            | **~15 μA - 20 mA**          |
| Latenza setup | ~100 ms           | **< 6 ms**                  |
| Topologia     | Stella (piconet)  | Stella, broadcast, **mesh** |

### Canali BLE

Dei 40 canali da 2 MHz ciascuno:

- **37 canali dati** (numerati 0-36): usati per la trasmissione dati dopo la connessione
- **3 canali di advertising/servizio** (37, 38, 39): usati prima della connessione, scelti appositamente per cadere **tra i canali Wi-Fi** (1, 6, 11) più utilizzati, minimizzando il cross-talk:
  - Ch. 37 -> 2402 MHz (tra fine guard e canale Wi-Fi 1)
  - Ch. 38 -> 2426 MHz (tra Wi-Fi 6 e 11)
  - Ch. 39 -> 2480 MHz (fine banda ISM)

### Modulazione BLE

BLE usa **sempre e solo GFSK binaria** (nessuna ambiguità come nel Classic):

- **LE 1M PHY** (1 Mb/s): deviazione frequenza ±185 kHz
- **LE 2M PHY** (2 Mb/s): deviazione ±370 kHz - introdotto in BLE 5.0
- **LE Coded PHY**: aggiunge FEC per aumentare il range a discapito del throughput:
  - **S=2** -> 500 kb/s, distanza raddoppiata rispetto LE 1M
  - **S=8** -> 125 kb/s, distanza quadruplicata (fino a ~400 m)

### Ruoli GAP dei Dispositivi

BLE introduce **quattro ruoli** distinti nel **Generic Access Profile (GAP)**:

- **Broadcaster**: trasmette solo pacchetti advertising (es. etichette elettroniche ESL, beacon iBeacon). Non accetta connessioni.
- **Observer**: ascolta solo (scanning passivo). Non avvia connessioni.
- **Peripheral**: dispositivo a basso consumo che fa advertising e accetta connessioni (es. smartwatch, auricolare, sensore).
- **Central**: dispositivo con più risorse (es. smartphone, tablet) che scansiona e avvia connessioni verso più periferiche.
- **Hybrid**: un dispositivo può svolgere più ruoli contemporaneamente.

### Advertising e Discovery

Il processo di advertising è la principale differenza rispetto al Classic - è la **periferica** (non il master) a iniziare diffondendo la propria presenza:

- Il Peripheral invia periodicamente pacchetti **ADV** sui canali 37, 38, 39 con **Access Address fisso**: `0x8E89BED6`
- **Advertising Interval**: da 20 ms a 10,24 s
- Il Central in ascolto, al ricezione di un ADV packet, può inviare una **SCAN_REQ** (active scanning) per richiedere informazioni aggiuntive; la periferica risponde con **SCAN_RSP** (nome, servizi supportati)

### Connessione BLE e Parametri CONNECT_IND

Quando il Central decide di connettersi, attende il successivo pacchetto ADV dalla periferica, poi - dopo esattamente **150 μs (IFS, Inter Frame Spacing)** - invia sullo stesso canale RF un pacchetto **CONNECT_IND** con i parametri di connessione:

**Struttura del payload CONNECT_IND (LLData):**

| Campo               | Dimensione | Descrizione                                                                           |
| ------------------- | ---------- | ------------------------------------------------------------------------------------- |
| Access Address (AA) | 4 byte     | Codice univoco a 32 bit generato random dal Central per identificare la connessione   |
| CRC Init            | 3 byte     | Valore iniziale per il CRC della connessione                                          |
| Win Size            | 1 byte     | Finestra di trasmissione                                                              |
| Win Offset          | 2 byte     | Offset della prima finestra di connessione                                            |
| **Interval**        | 2 byte     | **Connection Interval**: da 7,5 ms a 4 s (multipli di 1,25 ms)                        |
| **Latency**         | 2 byte     | **Peripheral Latency**: numero di eventi che la periferica può saltare se non ha dati |
| **Timeout**         | 2 byte     | **Supervision Timeout**: max 32 s; se superato -> Link Loss                           |
| ChM                 | 5 byte     | **Channel Map**: mappa dei 37 canali dati usabili                                     |
| Hop                 | 5 bit      | **Hop Increment** (h): usato per AFH                                                  |

> [!tip] Differenza con BT Classic
> A differenza del Classic, ogni connessione BLE ha il suo **Access Address univoco**, permettendo a una periferica di mantenere **più connessioni simultanee** con dispositivi diversi -> topologia **mesh**.

### Gestione della Connessione e Sleep Mode

Il grande vantaggio di BLE è il ciclo **trasmetti -> dormi -> svegliati**:

- Il dispositivo si sveglia ogni **Connection Interval (CI)** per scambiare dati, poi torna in **sleep mode**
- Se non ci sono dati, invia un **pacchetto vuoto** (10 byte) per confermare la connessione
- Il **IFS = 150 μs** è fisso e separato tra pacchetto TX e pacchetto RX
- La frequenza cambia ogni CI secondo l'algoritmo AFH per BLE:
  $$
  f_{k+1} = (f_k + h) \bmod 37
  $$
  dove $h$ è l'**hop increment**, valore costante negoziato durante il CONNECT_IND. La frequenza può variare da 133 volte/s (CI minimo = 7,5 ms) fino a 1 volta ogni 4 s.

### Formato Pacchetti BLE

**LE Uncoded PHY (1M o 2M):**

text

`| Preamble (1-2 byte) | Access Address (4 byte) | PDU (2-258 byte) | CRC (3 byte) |`

**LE Coded PHY (per range esteso):**

text

`| Preamble (10 byte) | AA (4b) | CI (2b) | Term1 (3b) | PDU | CRC (24b) | Term2 (3b) |`

Il preambolo più lungo serve per il sincronismo con il demodulatore FEC a bassa velocità.

### ARQ in BLE - Sequence Number Alternato

Il meccanismo ARQ BLE usa due flag nell'header del PDU:

- **SN (Sequence Number)**: alterna tra 0 e 1 ad ogni nuovo pacchetto
- **NESN (Next Expected Sequence Number)**: il ricevitore risponde con NESN = NOT(SN ricevuto) se il pacchetto è OK

Esempio: dispositivo A invia SN=0 -> B risponde NESN=1 (ACK implicito) -> A invia SN=1 -> B risponde NESN=0 -> ...

---

## Stack di Protocolli BLE

L'architettura BLE è molto più snella rispetto al Classic, progettata a strati chiari:

`┌─────────────────────────────────────────┐ │              APPLICATIONS               │ ├─────────────────────────────────────────┤  HOST │       Generic Access Profile (GAP)      │ │      Generic Attribute Profile (GATT)   │ │  Attribute Protocol (ATT) | Sec. Manager│ │   L2CAP (Logical Link Control & Adapt.) │ │     Host Controller Interface (HCI)     │ ├─────────────────────────────────────────┤  CONTROLLER │    Link Layer | Direct Test Mode        │ │          Physical Layer (PHY)           │ └─────────────────────────────────────────┘`

### Generic Access Profile (GAP)

Il **GAP** è il protocollo che governa come i dispositivi si "trovano" e si connettono:

#### Fasi di lavoro del GAP

1. **Discovery (Advertising/Scanning)**: gestisce Advertising Interval, Scan Window, Scan Interval
2. **Connection Establishment**: trasmissione CONNECT_IND con tutti i parametri temporali
3. **Security (Pairing & Bonding)**: gestisce cifratura AES-128, scambio chiavi ECDH (Elliptic Curve Diffie-Hellman), memorizzazione dei dispositivi (bonding)

### Generic Attribute Profile (GATT)

Il **GATT** definisce come i dati sono organizzati e scambiati tra dispositivi connessi, tramite il protocollo **ATT (Attribute Protocol)**:

**Gerarchia:**

`PROFILE   └── SERVICE (UUID 16-bit o 128-bit)        └── CHARACTERISTIC (UUID)              ├── Value (i dati veri)              └── Descriptor (metadati)`

- **GATT Server**: il dispositivo che espone i dati (es. sensore, auricolare) -> Peripheral
- **GATT Client**: il dispositivo che legge/scrive i dati (es. smartphone) -> Central

**Operazioni GATT**: Read, Write, Notify, Indicate, Write Without Response

**Esempi di servizi standard (UUID assegnati da Bluetooth SIG):**

| Service                       | UUID     | Caratteristica                  | Formato               |
| ----------------------------- | -------- | ------------------------------- | --------------------- |
| Environmental Sensing Service | `0x181A` | Temperature `0x2A1C`            | 16 bit, unità 0,01 °C |
| ESS                           | `0x181A` | Humidity `0x2A6F`               | 16 bit, unità 0,01%   |
| ESS                           | `0x181A` | Pressure `0x2A6D`               | 32 bit, unità 0,1 Pa  |
| Heart Rate Service            | `0x180D` | Heart Rate Measurement `0x2A37` | 1-2 byte, BPM         |
| Battery Service               | `0x180F` | Battery Level `0x2A19`          | 1 byte, % (0-100)     |

---

## Applicazioni BLE

BLE si presta a un'ampia gamma di applicazioni IoT:

- **Healthcare & Fitness**: cardiofrequenzimetri, pulsossimetri, monitor glucosio, termometri, activity tracker
- **Home Automation**: sensori temperatura/umidità, automazione I/O
- **Security & Proximity**: rilevamento presenza, beacon iBeacon, tag anti-smarrimento
- **Navigazione e posizionamento indoor**: misura distanza (ranging), presenza, **direzione** (Direction Finding, BLE 5.1+)
- **Retail**: etichette elettroniche ESL (Electronic Shelf Labels) nei supermercati

### BlueVoice - Audio su BLE

Il BLE non è nativo per lo streaming audio continuo (richiede throughput costante e alto), ma **BlueVoice** (protocollo vendor-specific di STMicroelectronics) risolve il problema:

- Trasmette l'audio compresso in **burst**, sfruttando al massimo la banda disponibile e tornando in sleep mode tra un burst e l'altro
- Pipeline: acquisizione audio -> modulazione **PDM** -> conversione PDM-to-PCM -> compressione -> trasmissione BLE
- Architettura simmetrica: il servizio audio è esposto come GATT Service con due Characteristic: **AudioData** (notifica, max 20 byte) e **SyncData** (notifica, 6 byte)
- Non richiede risposta (Notification, non Indication) -> ottimizzazione della banda per l'audio

> [!caution] Limitazioni di banda
> BLE ha un throughput massimo di ~2 Mb/s: **non è adatto allo streaming video**. Per proiettori wireless o video HD serve necessariamente il Wi-Fi.

---

## Tabella Comparativa

| Parametro   | Classic BR          | Classic EDR        | BLE (LE 1M)        | BLE (LE 2M)        | BLE Coded S8       |
| ----------- | ------------------- | ------------------ | ------------------ | ------------------ | ------------------ |
| Modulazione | GFSK                | π/4-DQPSK / 8-DPSK | GFSK               | GFSK               | GFSK + FEC         |
| Canali      | 79 × 1 MHz          | 79 × 1 MHz         | 37 × 2 MHz         | 37 × 2 MHz         | 37 × 2 MHz         |
| Bit rate    | 1 Mb/s              | 2-3 Mb/s           | 1 Mb/s             | 2 Mb/s             | 125 kb/s           |
| Slot/durata | 625 μs (1,3,5 slot) | 625 μs             | variabile          | variabile          | variabile          |
| Topologia   | Stella              | Stella             | Stella/Mesh        | Stella/Mesh        | Stella/Mesh        |
| Connessione | Master/Slave        | Master/Slave       | Central/Peripheral | Central/Peripheral | Central/Peripheral |
| ARQ         | ARQN bit            | ARQN bit           | SN/NESN alternato  | SN/NESN alternato  | SN/NESN alternato  |
| Audio       | SCO (64 kb/s)       | SCO                | BlueVoice (vendor) | BlueVoice          | -                  |

---

## Domande Frequenti

1. **Perché il Bluetooth usa il FHSS?** Per coesistere con il Wi-Fi sulla banda ISM 2,4 GHz evitando interferenze, e per sicurezza (difficoltà di intercettazione).
2. **Come viene generata la sequenza di hopping?** Da un algoritmo pseudo-random con seed: UAP+LAP del MAC del master e bits 1-26 del clock Bluetooth.
3. **Perché il Bluetooth Classic non può fare reti mesh?** Perché l'Access Code della piconet deriva dal MAC del master: è univoco per tutta la piconet. In BLE ogni connessione ha il suo Access Address a 32 bit generato casualmente, permettendo connessioni multiple indipendenti.
4. **Qual è la differenza tra SCO e ACL?** SCO = circuito (voce, intervalli fissi, no ARQ, no CRC); ACL = pacchetti (dati, best-effort, con ARQ e CRC).
5. **A cosa serve il Connection Interval in BLE?** Definisce la periodicità con cui central e periferica si svegliano per scambiarsi dati; il resto del tempo il dispositivo dorme, risparmiando energia.
