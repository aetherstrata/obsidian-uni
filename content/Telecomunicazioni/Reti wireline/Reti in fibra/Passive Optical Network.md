---
tags:
  - fibra
  - reti
  - internet
aliases:
  - PON
  - GPON
---

La rete di accesso è il segmento che collega l'utente finale (**NIU** - _Network Interface Unit_) al **POP** (_Point of Presence_), ovvero la centrale telefonica dell'operatore. Rispetto alle reti dorsali, presenta caratteristiche opposte:

| Caratteristica          | Rete Dorsale (OTN)              | Rete di Accesso          |
| ----------------------- | ------------------------------- | ------------------------ |
| **N° collegamenti**     | Pochi, ad alta capacità (400G+) | Moltissimi, capillari    |
| **Velocità**            | Fissa, altissima                | Flessibile per l'utente  |
| **Distanze**            | Centinaia-migliaia di km        | Max ~20 km               |
| **Riconfigurazioni**    | Rare                            | Frequenti (nuovi utenti) |
| **Criterio principale** | Capacità e affidabilità         | Riduzione dei costi      |

La rete di accesso non utilizza amplificatori ottici [[Collegamenti in fibra ottica#EDFA (Erbium Doped Fiber Amplifier)|EDFA]]: la rete deve essere **completamente passiva** tra centrale e utente per mantenere i costi bassi e non richiedere alimentazione elettrica intermedia. Anche i futuri standard a 50G non prevedono amplificazione in linea.

## Architettura FTTx

**FTTx** (Fiber To The x) indica dove termina la fibra ottica nel percorso verso l'utente e dove inizia il tratto in rame (se presente):

- **FTTEx** (_Fiber To The Exchange_): tutta rame dal cabinet all'utente; la fibra arriva solo fino alla centrale; si usa con ADSL2/VDSL2
- **FTTCab** (_Fiber To The Cabinet_): la fibra arriva fino all'armadio di strada (cabinet), poi rame per il tratto finale (~300-500 m); tecnologia xDSL sul tratto in rame
- **FTTB** (_Fiber To The Building_): la fibra arriva fino al palazzo; tratto in rame max ~200 m dentro l'edificio con tecnologia **G.fast** (fino a 1 Gbit/s); il dispositivo terminale si chiama **ONU** (Optical Network Unit)
- **FTTH** (_Fiber To The Home_): fibra fino in casa dell'utente; nessun tratto in rame; il dispositivo terminale si chiama **ONT** (Optical Network Terminal); si usano i protocolli PON

![[Pasted image 20260621153047.png]]

In Italia l'infrastruttura è gestita principalmente da **Open Fiber** e **FiberCop (ex TIM)**, con accesso wholesale agli operatori retail.

### Denominazione della terminazione

Se **UNI** si trova presso l'utente e il collegamento con l'**OLT** è in fibra ottica, allora si parla di **FTTH**. In questo caso UNI prende il nome di _Optical Network Termination_ (**ONT**).

Se il **UNI** è un centro di smistamento per più utenti, la connessione è di tipo **FTTC** oppure **FTTB**. In questo caso UNI viene definito _Optical Network Unit_ (**ONU**).

L'OLT, presso la centrale, effettua una multiplazione statistica per la condivisione della banda.

![[Pasted image 20260621160749.png]]

---

## Architettura PON

Una **PON** (Passive Optical Network) è una rete punto-multipunto in cui **nessun elemento attivo** (amplificatori, switch elettrici) è presente tra la centrale e l'utente: solo fibra ottica passiva e splitter ottici passivi.

### Elementi fondamentali

- **OLT** (_Optical Line Terminal_): posizionato in centrale, contiene laser **DFB** (Distributed Feedback) ad alta stabilità per il downstream e fotodiodo in ricezione; è il punto focale della PON, gestisce accesso al mezzo e scheduling
- **Splitter ottico passivo**: al **nodo remoto** (Remote Node - RN); divide il segnale ottico da 1 fibra in ingresso a N fibre in uscita (tipicamente 1:32, 1:64, 1:128) senza conversioni elettroniche; può essere realizzato con divisori in planar waveguide (PLC) a Y-branch
- **ONU/ONT**: a casa dell'utente; contiene [[Collegamenti in fibra ottica#Laser Fabry-Pérot|laser FP]] per upstream e fotodiodo PIN + amplificatore a transimpedenza per ricezione downstream

![[Pasted image 20260621154441.png]]

### Topologia fisica

La rete si divide in due segmenti:

- **ODN primaria** (feeder): fibra dalla centrale OLT -> splitter al nodo remoto
- **ODN secondaria** (distribution): fibra dallo splitter -> singolo utente ONU/ONT

![[Pasted image 20260621154616.png]]

La stessa fibra fisica è usata sia per downstream sia per upstream tramite **multiplexing in lunghezza d'onda** (WDM): due λ diverse per le due direzioni per evitare il cross-talk.

### PON vs P2P

Una rete _P2P_ richiederebbe **2N** transceiver in centrale (un laser + fotodiodo per ogni utente), un costo inaccettabile per centinaia di migliaia di utenti. La PON riduce a **N+1** i transceiver attivi: N transceiver a casa degli utenti, 1 solo per OLT in centrale.

> [!info] Architettura PON
> ![[Pasted image 20260621161022.png]]

> [!info] Architettura P2P
> ![[Pasted image 20260621161211.png]]

---

## Accesso al Mezzo

### Downstream

L'OLT trasmette in broadcast: tutti gli utenti connessi allo stesso splitter ricevono **tutti** i frame trasmessi dalla centrale. Ogni ONU/ONT **filtra** in ricezione i soli frame con il proprio indirizzo, ignorando gli altri. Il multiplexing è di tipo **statistico** nel dominio del tempo (**[[Time Division Multiplexing|TDM]]**): se un utente non ha dati da ricevere, la sua banda viene usata dagli altri. Per garantire la **privacy** del downstream broadcast si usa la crittografia **AES** (Advanced Encryption Standard).

![[Pasted image 20260621162451.png]]

### Upstream

In upstream il problema è opposto: se tutti gli utenti trasmettono liberamente si generano **collisioni** nello splitter passivo (che non distingue i segnali). La soluzione è il **TDMA controllato** (_Time Division Multiple Access_): solo l'OLT decide chi può trasmettere e quando, attraverso l'algoritmo **DBA** (_Dynamic Bandwidth Allocation_).

![[Pasted image 20260621164021.png]]

Il ciclo DBA funziona così:

1. Ogni ONU comunica all'OLT lo stato del suo buffer di trasmissione (DBA report)
2. L'OLT esegue l'algoritmo DBA e calcola la **BW Map** (mappa di allocazione della banda)
3. La BW Map viene inviata a tutte le ONU nel frame downstream; specifica per ogni ONU l'**ALLOC_ID** con il time slot di inizio e fine in cui può trasmettere
4. Le ONU trasmettono in upstream **esclusivamente nel proprio slot temporale** assegnato

In GPON le unità di allocazione della banda si chiamano **T-CONT** (Transmission Container), ciascuno con una propria classe di priorità. Il DBA permette di ottimizzare l'utilizzo della banda su scala di millisecondi/microsecondi adattandosi al traffico a burst.

> [!info] T-CONT e Tipi di Banda
>
> Il **T-CONT** (Transmission Container) è l'unità logica di allocazione della banda upstream in GPON. Ogni ONU può avere più T-CONT, ciascuno con diversi requisiti di QoS, identificato dall'**Alloc-ID** nella BWmap.
>
> I 5 tipi di banda allocabili a un T-CONT sono:
>
> | Tipo       | Nome                        | Comportamento OLT                                                                         | Uso tipico                      | Latenza        |
> | ---------- | --------------------------- | ----------------------------------------------------------------------------------------- | ------------------------------- | -------------- |
> | **Type 1** | Fixed                       | Alloca periodicamente indipendentemente dal buffer; invia celle idle se il buffer è vuoto | TDM, VoIP isocrono              | Deterministica |
> | **Type 2** | Assured                     | Alloca solo se il buffer dell'ONU ha dati; la banda non usata va agli altri               | VoIP adattivo, video conferenza | Bassa          |
> | **Type 3** | Non-assured (Assured + Max) | Banda guaranteed + banda aggiuntiva fino al massimo se disponibile; allocazione SR-driven | Video streaming, IPTV           | Media          |
> | **Type 4** | Best-effort                 | Ultima priorità; riceve banda residua dopo tutti gli altri                                | Dati Internet best-effort       | Non garantita  |
> | **Type 5** | Mixed (tutti i precedenti)  | Combinazione di fixed + assured + non-assured + best-effort in un solo T-CONT             | Servizi multipli su un T-CONT   | Varia          |

### Problemi fisici dell'upstream

Due problemi tecnici critici dell'upstream in PON:

1. **Ranging**: Gli ONU si trovano a distanze diverse dall'OLT (da pochi metri a ~20 km) creando il **near-far problem**. Per sincronizzare correttamente i time slot, l'OLT misura il **Round Trip Time (RTT)** per ogni ONU e assegna un offset di clock correttivo. Il ranging viene eseguito sia all'installazione che continuamente per mantenere la sincronizzazione.

2. **Ricevitore burst-mode**: Gli ONU più lontani trasmettono segnali più attenuati rispetto a quelli vicini. Il ricevitore OLT deve gestire frame con livelli di potenza molto diversi in sequenza rapida. Si usano ricevitori **burst-mode** che adattano automaticamente la soglia di decisione (AGC rapida) frame per frame.

## Lunghezze d'Onda

In una PON TDM classica si usano **due finestre ottiche** per separare i due sensi di trasmissione sulla stessa fibra:

| Direzione                       | λ standard  | Finestra | Attenuazione fibra          |
| ------------------------------- | ----------- | -------- | --------------------------- |
| **Downstream** (OLT -> ONU)     | **1490 nm** | Terza    | ~0,24 dB/km                 |
| **Upstream** (ONU -> OLT)       | **1310 nm** | Seconda  | ~0,34 dB/km                 |
| **Video broadcast** (opzionale) | **1550 nm** | Terza    | riservato per TV RF overlay |

La finestra a 1310 nm in upstream ha attenuazione leggermente maggiore rispetto a 1490 nm in downstream: ciò significa che il power budget upstream è più vincolante. Si stanno installando fibre **Zero Water Peak (ZWP, ITU G.652.D)** prive di ioni OH- per eliminare il picco di attenuazione intorno a 1383 nm, aprendo l'intera seconda finestra tra 1260 e 1625 nm per future espansioni.

> [!tip] Perché non si Usa il WDM nelle PON Standard?
>
> Un concetto fondamentale da affrontare è il perché non assegnare a ogni utente una lunghezza d'onda diversa (WDM-PON puro), invece di condividere il tempo (TDM). La risposta è economica e logistica:
>
> - Se ogni utente avesse una $\lambda$ dedicata, l'ONU da installare a casa dovrebbe essere **specifico per quella λ**, richiedendo un catalogo di dispositivi diversi
> - Se un ONU si guasta, l'operatore dovrebbe sostituirlo con uno che operi **esattamente sulla stessa λ**, complicando la gestione del magazzino
> - Il TDM invece usa ONU **identici e intercambiabili**: tutti operano alla stessa lunghezza d'onda, semplificando enormemente la logistica e riducendo i costi
>
> Il **WDM viene invece usato** solo per separare downstream e upstream sulla stessa fibra (due $\lambda$ fisse) e, in NG-PON2, per aumentare la capacità aggregata dell'OLT con un numero limitato di $\lambda$ (4 o 8) gestite da transceiver sintonizzabili.

---

## Stack Protocollare GPON

**GPON** (Gigabit Passive Optical Network) è definito da **ITU-T G.984** e rappresenta lo standard ampiamente installato in Europa. Le sue caratteristiche base:

- **Downstream**: 2,488 Gbit/s a 1490 nm
- **Upstream**: 1,244 Gbit/s a 1310 nm
- **Split ratio**: fino a 1:128
- **Distanza max**: 20 km
- **Frame rate**: 125 µs (8.000 frame/s, allineato al clock SDH)

### Layer GTC (GPON Transmission Convergence)

Il **GTC** (_GPON Transmission Convergence_, ITU-T G.984.3) è parte fondamentale del protocollo GPON: si occupa dell'incapsulamento dei dati, del controllo dell'accesso al mezzo, della sincronizzazione e della gestione della rete.

È composto da due sublayer quasi indipendenti che comunicano tramite interfacce ben definite:

> [!info] Struttura Transmission Coverage
> ![[Pasted image 20260621173954.png]]
>
> - **PLOAM** - Physical Layer Operation Administration Maintenance
> - **OMCI** - ONT Management and Control Interface
> - **GEM** - GPON Encapsulation Method
> - **DBA** - Dynamic Band Allocation

> [!info] Struttura layer GTC
> ![[Pasted image 20260621180150.png|center|450]]

#### GTC Framing Sublayer

- Sincronizzazione dei frame tramite pattern fisso di 4 byte nell'header
- Gestione del ranging e dell'orologio di ogni ONU
- Canale **PLOAM** (Physical Layer OAM): invia e riceve informazioni di gestione, stato della connessione, allarmi
- Controllo di parità **BIP** (_Bit Interleaved Parity_) sull'header per misurare il BER
- **BW Map** downstream: tabella che assegna a ogni T-CONT il time slot upstream (`ALLOC_ID`, `T_Start`, `T_Stop`)

#### GEM Encapsulation Sublayer (GPON Encapsulation Method)

- Adatta qualsiasi tipo di frame cliente (Ethernet, IP, TDM) ai GEM frame
- Ogni ONU ha N **porte GEM** logiche (canali unidirezionali virtuali OLT <--> ONU)
- Le porte GEM corrispondono alle diverse classi di servizio (dati, VoIP, video)
- Frame di lunghezza variabile (simile a GFP nelle [[Optical Transport Network|OTN]])

![[Pasted image 20260621182927.png]]

### Frame GTC Downstream

Ogni frame GTC in downstream ha una durata di esattamente **125 µs** (8.000 frame/s), allineato al clock [[Gerarchia Digitale#SDH - Synchronous Digital Hierarchy|SDH]]. Un frame GPON a 2,488 Gbit/s trasporta quindi:

$$
N_{byte} ​= 82.488\cdot10^9\cdot125\cdot10^{-6}​=38.880\text{ byte per frame}
$$

> [!tip] Sequenza di eventi
>
> 1.  L'**OLT** invia i frame Ethernet al modulo di elaborazione del servizio **GPON**
> 2.  Il modulo di elaborazione del servizio **GPON** incapsula i dati Ethernet in pacchetti di dati per trasmetterli downstream.
> 3.  I frame vengono trasmessi a tutte le unità **ONT**/**ONU** collegate alla **GPON**.
> 4.  Le unità **ONT**/**ONU** filtrano i dati ricevuti in base all'ID della porta **GEM** e conserva solo i dati dedicati alla **ONT**/**ONU**.
> 5.  L'**ONT** elabora i dati e invia i frame Ethernet agli utenti finali tramite le porte di servizio.

> [!info] Struttura frame GTC Downstream
> ![[Pasted image 20260621183244.png]]

L'header **PCBd** (Physical Control Block downstream) contiene:

- **PSync** (_Physical Syncronization_) ha un valore fisso di `0xB6AB31E0`. Ogni ONU cerca continuamente questa sequenza nel flusso downstream per identificare l'inizio di ogni frame e mantenere la sincronizzazione.
- **Ident** indica il numero di _superframe_. Un **superframe** è un gruppo di 256 frame GTC consecutivi, e il contatore Ident serve a numerare i superframe per permettere alle ONU di allineare operazioni che si estendono su più frame.
- **PLOAMd**: canale che trasporta un singolo messaggio indirizzato a una specifica ONU o a tutte le ONU in broadcast.
- **BIP** (_Bit Interleaved Parity_) contiene un checksum calcolato su tutti i byte del frame precedente.
- **Plend** (_Payload length downstream_) indica la dimensione della sezione ATM e della sezione GEM all'interno del payload. È trasmesso due volte per robustezza.
- **BWmap**: tabella che specifica per ogni T-CONT attivo nella rete quando e per quanti byte è autorizzato a trasmettere nel prossimo frame upstream.

Il payload è composto da **GEM frame** destinati ai vari ONU, ciascuno con il proprio Port-ID.

> [!caution] Sicurezza del flusso downstream
>
> Il canale downstream è intrinsecamente **broadcast**: tutti gli ONU connessi allo stesso splitter ricevono tutti i GEM frame. Il meccanismo di sicurezza è il seguente:
>
> 1.  L'OLT cifra il **payload GEM** (non l'header) con **AES-128** in modalità CTR (Counter Mode)
> 2.  La chiave AES è **per-ONU**: ogni ONU ha la propria chiave e può decifrare solo i propri frame
> 3.  Il **Port-ID** nell'header GEM (in chiaro) consente a ogni ONU di identificare quali frame sono destinati a sé e scartare il resto **prima** della decifrazione
> 4.  La rotazione delle chiavi avviene tramite messaggi PLOAM `Encrypt_Key_Switching`, sincronizzati tramite il **Superframe Counter** nel campo Ident

### Frame GTC Upstream

Il frame upstream ha la stessa durata di 125 µs e alla stessa velocità (1,244 Gbit/s per GPON), ma è strutturato in modo completamente diverso dal downstream. Non esiste un singolo frame upstream perché è creato partendo dalla **concatenazione temporale di burst** provenienti da ONU diversi, ciascuno trasmesso nel proprio slot assegnato dalla **BWmap**.

#### Struttura del Burst Upstream

Ogni burst trasmesso da un ONU in upstream si apre con un **PLO** (Physical Layer Overhead) obbligatorio, seguito dai dati:
text

`|←-- PLO (Overhead) --------------->|←-- Payload ------->| | Guard | Preamble | Delimit | PLOAMu | PLSu | DBRu | GEM frames | | time  |          |         |        |      |      |            |`

- **Guard Time**: intervallo di silenzio prima di ogni burst; garantisce separazione temporale tra ONU diversi ed evita collisioni dovute alle imprecisioni di clock residue dopo il ranging. Tipicamente 32-64 bit.

- **Preamble**: sequenza di addestramento (training) per il ricevitore burst-mode dell'OLT; permette al ricevitore di acquisire la fase del clock e impostare la soglia di decisione dell'AGC (Automatic Gain Control) sul livello di potenza specifico di quell'ONU. Essendo ogni ONU ad una distanza diversa, la potenza ricevuta può variare di molti dB tra burst consecutivi.

- **Delimiter**: sequenza che segnala la fine del preamble e l'inizio dei dati effettivi; consente al ricevitore burst-mode di passare dalla fase di acquisizione alla fase di ricezione dati.

#### Campi Overhead Upstream

**PLOAMu - Physical Layer OAM upstream (13 byte)**  
Messaggi di gestione dall'ONU verso l'OLT:

- `Serial_Number_ONU`: l'ONU risponde alla scoperta con il proprio numero seriale WWPN-equivalente
- `Password`: autenticazione durante l'attivazione
- `REI` (Remote Error Indication): segnala al OLT quanti errori BIP ha rilevato sul downstream
- `Dying_Gasp`: l'ONU segnala che sta per perdere alimentazione
- Acknowledgment dei messaggi PLOAM ricevuti

**PLSu - Power Levelling Sequence upstream (120 byte)**  
Sequenza di calibrazione della potenza laser dell'ONU; permette all'OLT di misurare il livello di potenza ricevuto e inviare all'ONU un comando di correzione (Power_Level_Range), compensando le differenti attenuazioni del link.

**DBRu - Dynamic Bandwidth Report upstream (1-5 byte)**  
Report di stato del buffer dell'ONU, inviato all'OLT per comunicare quanti dati sono in attesa di trasmissione nei T-CONT dell'ONU. Due modalità:

- **SR (Status Reporting)**: l'ONU invia il DBRu con la dimensione del buffer -> il DBA dell'OLT alloca banda proporzionale alla domanda reale
- **NSR (Non Status Reporting)**: l'OLT stima il bisogno di banda senza ricevere report espliciti, utile per ridurre overhead

#### GEM Frame

Il payload dati vero e proprio del burst upstream è composto da **frame GEM** (_GPON Encapsulation Method_), identici per struttura a quelli del downstream:

text

`|← GEM Header (5 byte) →|←-- GEM Payload (variabile, max 4095 byte) --→| | PLI (12b) | Port-ID (12b) | PTI (3b) | HEC (13b) | Payload…           |`

- **PLI** (Payload Length Indicator, 12 bit): lunghezza in byte del payload GEM
- **Port-ID** (12 bit): identificatore logico del canale GEM; mappa un flusso dati specifico (un VLAN, un servizio voce, ecc.) - fino a 4096 porte GEM per ONU
- **PTI** (Payload Type Indicator, 3 bit): tipo di payload (00x = dati utente, 10x = OAM, 1x1 = fine di un frame Ethernet segmentato su più GEM frame)
- **HEC** (Header Error Correction, 13 bit): CRC sull'header per rilevare e correggere errori nell'header GEM; consente la risincronizzazione sul confine del frame GEM

Il GEM supporta la **segmentazione** di frame Ethernet lunghi su più GEM frame e il **concatenamento** di più frame Ethernet corti in un singolo GEM frame per efficienza.

### Dynamic Bandwidth Allocation

Il **DBA** (_Dynamic Bandwidth Allocation_) è l'algoritmo eseguito dall'OLT per costruire la BWmap di ogni frame. Il ciclo completo in ogni periodo di 125 µs è:

1. **Raccolta dei DBRu**: durante il frame N-1, l'OLT raccoglie i report di stato buffer di tutti gli ONU
2. **Calcolo DBA**: l'algoritmo DBA elabora i report e calcola i grant per il frame N+1:
   - Assegna prima la banda **fixed** (type 1) a tutti i T-CONT che la richiedono
   - Poi assegna la banda **assured** (type 2/3) agli ONU che hanno dati nel buffer
   - Poi distribuisce la banda **non-assured** in proporzione alle richieste
   - Infine assegna il residuo ai T-CONT **best-effort** (type 4)
3. **Costruzione della BWmap**: l'OLT crea la lista ordinata di entry (Alloc-ID, StartTime, StopTime) assicurandosi che non ci siano sovrapposizioni temporali
4. **Inserimento nel PCBd**: la BWmap viene inserita nel frame downstream del frame N
5. **Esecuzione**: gli ONU leggono la BWmap e trasmettono nel frame upstream N+1 esattamente nei loro slot

Il DBA è centralizzato: **solo l'OLT** decide chi trasmette e quando; gli ONU non possono mai trasmettere al di fuori del loro slot assegnato.

## Processo di Attivazione ONU

Prima che un ONU possa trasmettere dati, deve eseguire un processo di attivazione multi-step:

| Fase          | Stato | Descrizione                                                                                                                                                                                                                                                                                               |
| ------------- | ----- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Power On**  | O1    | L'ONU si accende e inizia a cercare il segnale a 1490nm                                                                                                                                                                                                                                                   |
| **Sync**      | O2    | L'ONU riceve il segnale e si sincronizza sul frame GTC tramite il<br>PSync. Legge i messaggi **PLOAM**                                                                                                                                                                                                    |
| **Discovery** | O3    | OLT apre una quiet window, un intervallo di tempo in cui nessuna<br>ONU è autorizzata a trasmettere traffico normale. In questa<br>finestra l'OLT invia un messaggio PLOAM di richiesta Serial Number<br>broadcast. Le ONU non ancora registrate rispondono trasmettendo<br>il proprio **Serial Number**. |
| **Ranging**   | O4    | L'OLT misura il ritardo di propagazione e assegna l’**Equalization<br>Delay** e **ONU-ID**                                                                                                                                                                                                                |
| **Operative** | O5    | L'ONU è attiva. Inizia lo scambio di dati e la configurazione **OMCI**.<br>L'OLT procede con il provisioning dei T-CONT e delle GEM port<br>tramite il canale **OMCI**.                                                                                                                                   |

### Attivazione obbligatoria in XGS-PON

In **GPON** l'autenticazione della ONU è opzionale e basata su un meccanismo semplice: durante l'attivazione la ONU può trasmettere una password in chiaro tramite un messaggio **PLOAM**. L'OLT confronta la password ricevuta con quella configurata nel proprio database di provisioning. Molti operatori non attivano questo meccanismo, affidandosi al solo **Serial Number** come identificatore.

In **XGS-PON** l'autenticazione è obbligatoria: la ONU dimostra all'OLT di conoscere la Registration ID senza trasmetterla in chiaro, e il meccanismo previene sia l'accesso di ONU non autorizzate sia gli attacchi di replay. Una ONU che fallisce l'autenticazione viene bloccata con un timer di backoff esponenziale che previene attacchi di forza bruta.

## Power Budget e Range Budget

Il **power budget** definisce la massima attenuazione tollerabile nel link ottico. Il calcolo si fa in dB:

$$
P\cdot{budget} = P\cdot{TX} - S\cdot{RX}
$$

dove $P*{TX}$​ è la potenza trasmessa (dBm) e $S*{RX}$​ è la sensitività del ricevitore (dBm, cioè la potenza minima per $\text{BER}\le10^{-9}$).

Le perdite da considerare sono:

- **Attenuazione fibra**: ~0,24 dB/km a 1490 nm (terza finestra) e ~0,34 dB/km a 1310 nm (seconda finestra)
- **Perdite dei connettori**: ~0,3 dB per connettore
- **Perdite degli splice** (giunti di fusione): ~0,1 dB per giunto
- **Perdite dello splitter**: uno splitter introduce **-3 dB** di splitting loss per ogni fattore di divisione:

| Splitting Factor | Attenuazione |
| ---------------- | ------------ |
| 1:2              | **-3 dB**    |
| 1:4              | **-6 dB**    |
| 1:8              | **-9 dB**    |
| 1:16             | **-12 dB**   |
| 1:32             | **-15 dB**   |
| 1:64             | **-18 dB**   |
| 1:128            | **-21 dB**   |

Il **range budget** determina la distanza massima raggiungibile: dal power budget si sottraggono le perdite fisse (splitter, connettori) e si divide il residuo per l'attenuazione della fibra. Le classi di ODN (ODN Class B+, C, C+) definiscono i livelli di potenza standard.

---

## Evoluzione degli Standard PON

### Famiglia ITU-T (usata in Europa e Asia)

| Standard    | ITU-T  | Downstream        | Upstream    | Split | Note                               |
| ----------- | ------ | ----------------- | ----------- | ----- | ---------------------------------- |
| **GPON**    | G.984  | 2,5 Gbit/s        | 1,25 Gbit/s | 1:128 | Standard attuale in Europa         |
| **XG-PON1** | G.987  | 10 Gbit/s         | 2,5 Gbit/s  | 1:128 | Asimmetrico, transizione           |
| **XGS-PON** | G.9807 | 10 Gbit/s         | 10 Gbit/s   | 1:128 | **Simmetrico**, in deployment 2024 |
| **NG-PON2** | G.989  | 40 Gbit/s (4×10G) | 10 Gbit/s   | 1:256 | TWDM-PON, 4 λ WDM + TDM            |
| **50G-PON** | G.9804 | 50 Gbit/s         | 50 Gbit/s   | 1:64  | Standard 2021, deployment ~2025    |

**XGS-PON** (XG-PON Symmetric) è il successore diretto del GPON: opera a **10 Gbit/s simmetrici** su ogni $\lambda$. A differenza di XG-PON1 (asimmetrico 10/2.5G), la simmetria è motivata dall'evoluzione dei pattern di traffico degli utenti (upload di video, cloud, backup).

### NG-PON2 - TWDM-PON

**NG-PON2** (G.989) combina **TDM e WDM** (TWDM - Time and Wavelength Division Multiplexing): utilizza **4 lunghezze d'onda** in downstream (1596-1603 nm) e 4 in upstream (1524-1544 nm), ciascuna a 10 Gbit/s, per una capacità totale di **40 Gbit/s downstream e 10-40 Gbit/s upstream**. Ogni ONU è dotato di un **transceiver sintonizzabile** (tunable transceiver) in grado di selezionare la propria λ di lavoro, permettendo load balancing tra le lunghezze d'onda. Lo split ratio sale a 1:256 e la distanza massima a 40 km.

### 50G-PON - La Prossima Generazione

**50G-PON** (G.9804, standardizzato da ITU-T nel 2021) porta la velocità a **50 Gbit/s simmetrici per λ**. L'IEEE ha parallelamente standardizzato il **50G-EPON** (IEEE 802.3ca) con velocità di 25 e 50 Gbit/s. Le principali sfide tecniche sono la gestione del burst-mode a 50G e il power budget per lo splitter a 1:64.

## TWDM-PON

**TWDM** (_Time and Wavelength Division Multiplexing_) è la tecnica di accesso al mezzo adottata da **NG-PON2** (ITU-T G.989), che combina la multiplazione temporale (TDM/TDMA), già vista in GPON, con la multiplazione in lunghezza d'onda (WDM), ottenendo un aumento di capacità aggregata di 4 o 8 volte rispetto a XGS-PON.

### Principio di Funzionamento

L'idea centrale del TWDM è semplice: invece di usare **una sola coppia di lunghezze d'onda** (downstream + upstream), si usano **N coppie di λ** simultaneamente sulla stessa fibra. Su ogni coppia di $\lambda$ si applica il normale TDM/TDMA tra gli ONU assegnati a quella coppia, esattamente come in GPON o XGS-PON.

Il risultato è che la capacità totale della PON scala linearmente con il numero di coppie:

$$
C_{tot} = N_\lambda \times C_{\lambda}
$$

Per NG-PON2 con 4 lunghezze d'onda e 10 Gbit/s per λ:

$$
C_{tot} = 4 \times 10\,\text{Gbit/s} = 40\,\text{Gbit/s downstream},\quad 4 \times 2{,}5 \text{ o } 10\,\text{Gbit/s upstream}
$$

### Bande di Frequenza NG-PON2

NG-PON2 utilizza la banda **L+** per il downstream e la banda **U** per l'upstream, scelte per garantire la **coesistenza** con le PON legacy (GPON a 1490/1310 nm, video overlay a 1550 nm) sulla stessa fibra senza filtri aggiuntivi:

| Direzione      | Banda | Range λ      | N. canali | Spaziatura         |
| -------------- | ----- | ------------ | --------- | ------------------ |
| **Downstream** | L+    | 1596–1602 nm | 4 (o 8)   | ~0,8 nm (~100 GHz) |
| **Upstream**   | U     | 1524–1544 nm | 4 (o 8)   | ~3,2 nm (~400 GHz) |

La spaziatura in upstream è più ampia per tollerare la deriva termica dei [[Collegamenti in fibra ottica#Laser Fabry-Pérot|laser FP]] nei trasmettitori ONU, meno stabili dei [[Collegamenti in fibra ottica#Laser a feedback distribuito|laser DFB]] dell'OLT.

### OLT in TWDM

L'OLT TWDM contiene **N trasceiver indipendenti**, uno per ogni coppia di $\lambda$. Ciascun trasceiver gestisce la propria sotto-PON (un sottoinsieme degli ONU totali) con il proprio scheduler DBA, la propria BWmap e il proprio ciclo GTC a 125 µs. I N trasceiver sono accoppiati otticamente su un singolo connettore verso la fibra tramite un **MUX/DEMUX WDM** (Array Waveguide Grating, AWG) integrato nell'OLT.

### ONU Sintonizzabile

Il componente critico e distintivo di TWDM è il **transceiver sintonizzabile** nell'ONU. A differenza di GPON/XGS-PON dove ogni ONU ha un laser fisso, in TWDM-PON ogni ONU deve essere in grado di operare su qualsiasi delle N lunghezze d'onda disponibili.

#### Componenti dell'ONU TWDM

- **Laser sintonizzabile in trasmissione** (upstream): tipicamente un laser DS-DBR (Digitally Switched Distributed Bragg Reflector) o EAM-DFB sintonizzabile; cambia λ in pochi ms su comando dell'OLT
- **Ricevitore a banda larga** (downstream): un fotodiodo con largo range spettrale (1596–1603 nm) abbinato a un filtro ottico sintonizzabile o a un array di fotodiodi

#### Vantaggi dell'ONU sintonizzabile

- **Load balancing**: l'OLT può spostare ONU da una $\lambda$ congestionata a una λ con meno traffico
- **Protezione**: in caso di guasto a un trasceiver OLT, gli ONU sulla $\lambda$ guasta vengono migrati su un'altra $\lambda$ operativa → **protezione λ**
- **Efficienza**: gli ONU rimangono fisicamente identici e intercambiabili (vantaggio chiave rispetto al WDM puro, già discusso per GPON)

---

## Riepilogo Evoluzioni

| Parametro           | GPON        | XGS-PON    | NG-PON2       | 50G-PON    |
| ------------------- | ----------- | ---------- | ------------- | ---------- |
| **Standard**        | ITU G.984   | ITU G.9807 | ITU G.989     | ITU G.9804 |
| **Anno**            | 2003        | 2016       | 2015          | 2021       |
| **DS (per λ)**      | 2,5 Gbit/s  | 10 Gbit/s  | 10 Gbit/s     | 50 Gbit/s  |
| **US (per λ)**      | 1,25 Gbit/s | 10 Gbit/s  | 2,5-10 Gbit/s | 50 Gbit/s  |
| **Simmetrico**      | No          | Sì         | Dipende       | Sì         |
| **Tecnica accesso** | TDM-PON     | TDM-PON    | TWDM-PON      | TDM-PON    |
| **N° λ**            | 1           | 1          | 4-8           | 1          |
| **Split max**       | 1:128       | 1:128      | 1:256         | 1:64       |
| **Dist. max**       | 20 km       | 20 km      | 40 km         | 20 km      |
| **λ DS**            | 1490 nm     | 1577 nm    | 1596-1603 nm  | 1340 nm    |
| **λ US**            | 1310 nm     | 1270 nm    | 1524-1544 nm  | 1260 nm    |
