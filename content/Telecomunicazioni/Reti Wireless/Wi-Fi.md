---
tags:
  - wifi
  - pacchetti
  - data-link
---

Il Wi-Fi (IEEE 802.11) è il sistema di connettività wireless a corto raggio più diffuso al mondo, nato per sostituire le LAN cablate negli ambienti domestici e aziendali.

Le prestazioni sono nettamente superiori rispetto ad altre tecnologie wireless a bassa potenza: [[Bluetooth#Bluetooth Low Energy (BLE)|Bluetooth Low Energy]] si ferma a circa 2-3 Mbit/s, mentre Wi-Fi 7 (802.11be) raggiunge **46 Gbit/s** di velocità RAW teorica e Wi-Fi 6 (802.11ax) arriva a **9,6 Gbit/s**.

> [!caution] Nota legale
> Il termine "Wi-Fi" è gestito dalla **Wi-Fi Alliance**, che certifica i dispositivi conformi allo standard con il marchio **Wi-Fi CERTIFIED**.

Il Wi-Fi trova applicazione in:

- Streaming video 4K e sistemi di sorveglianza IP
- Domotica e smart home (hub, lampadine smart, assistenti vocali come Alexa)
- IoT industriale e reti di sensori
- Reti aziendali e campus universitari

A differenza del [[Bluetooth]], che usa il **frequency hopping** su canali variabili, il Wi-Fi opera su un canale fisso e utilizza tecniche di accesso multiplo basate su **[[Orthogonal Frequency Division Multiplexing|OFDM]]** e **CSMA/CA** per gestire la condivisione del mezzo.

## Evoluzione standard IEEE 802.11

Il Wi-Fi è uno standard della famiglia IEEE 802, esattamente come Ethernet (802.3) e [[Bluetooth]] (802.15). I primi due standard (802.11 originale del 1997 e 802.11b/a del 1999) sono oggi obsoleti. Lo studio attivo parte dal **Wi-Fi 3 (802.11g)** in poi.

### Tabella degli standard Wi-Fi

| Anno | Standard | Branding   | Tecnologia | Modulazione max | MIMO  | BW max  | Bande (GHz) | Velocità max |
| ---- | -------- | ---------- | ---------- | --------------- | ----- | ------- | ----------- | ------------ |
| 1997 | 802.11   | - -        | FH         | GFSK            | - -   | 1 MHz   | 2.4         | 2 Mbit/s     |
| 1999 | 802.11b  | Wi-Fi 1    | DSSS       | QPSK            | - -   | - -     | 2.4         | 11 Mbit/s    |
| 1999 | 802.11a  | Wi-Fi 2    | OFDM       | 64-QAM          | - -   | 20 MHz  | 5           | 54 Mbit/s    |
| 2003 | 802.11g  | Wi-Fi 3    | OFDM       | 64-QAM          | - -   | 20 MHz  | 2.4         | 54 Mbit/s    |
| 2008 | 802.11n  | Wi-Fi 4    | OFDM       | 64-QAM          | SU×4  | 40 MHz  | 2.4/5       | 600 Mbit/s   |
| 2014 | 802.11ac | Wi-Fi 5    | OFDM       | 256-QAM         | MU×8  | 160 MHz | 5           | 6,9 Gbit/s   |
| 2019 | 802.11ax | Wi-Fi 6/6E | OFDM/OFDMA | 1024-QAM        | MU×8  | 160 MHz | 2.4/5/6     | 9,6 Gbit/s   |
| 2024 | 802.11be | Wi-Fi 7    | OFDMA      | 4096-QAM        | MU×16 | 320 MHz | 2.4/5/6     | 46 Gbit/s    |

> [!info] Definizioni
>
> - **SU** = [[Onde a radiofrequenza#MIMO - Multiple Input Multiple Output|Single-User MIMO]]
> - **MU** = [[Onde a radiofrequenza#Multi-User MIMO (MU-MIMO)|Multi-User MIMO]]
>
> La velocità indicata è il picco RAW PHY (non la throughput reale dell'utente).

---

## Bande di Frequenza

### Banda 2.4 GHz

La banda ISM a 2,4 GHz è suddivisa in **14 canali** di 20 MHz ciascuno, ma i canali si sovrappongono parzialmente. Per evitare interferenze tra AP adiacenti si usano i **canali non sovrapposti**: **1, 6 e 11** (in Europa; il canale 14 è disponibile solo in Giappone). La maggior parte dei router seleziona il canale 6 come default. L'AP sceglie il canale all'avvio e tutti i client lo seguono.

> [!info] Canali 2.4GHz
> ![[Pasted image 20260625212814.png]]

- **Pro:** Maggiore copertura (portata fino a ~150 m in ambienti aperti), migliore penetrazione delle pareti.
- **Contro:** Banda disponibile limitata (max 60 MHz usando 3 canali), molto congestionata in aree urbane dense.

### Bande 5 GHz e 6 GHz

A 5 GHz sono disponibili fino a **25 canali da 20 MHz** (500 MHz totali), con possibilità di channel bonding fino a 160 MHz. Tuttavia, alcuni canali condividono frequenze con i **radar militari e meteorologici** (bande S, L, C). Per questo esiste il meccanismo:

- **DFS (Dynamic Frequency Selection):** il router ascolta il canale per ~60 secondi prima di usarlo. Se rileva un radar, cambia canale immediatamente. I canali senza radar sono usabili senza restrizioni, i canali DFS richiedono questa procedura di sensing.

> [!info] Canali 5GHz
> ![[Pasted image 20260625224008.png]]

La banda a **6 GHz** (introdotta con Wi-Fi 6E) aggiunge **59 canali da 20 MHz** (1.200 MHz di spettro totale), praticamente quasi inutilizzata da altre tecnologie, permettendo prestazioni elevate con interferenze minime.

> [!info] Canali GHz
> ![[Pasted image 20260625224630.png]]

### Channel Bonding

Il **channel bonding** consiste nell'aggregare canali adiacenti non sovrapposti per ottenere una banda complessiva maggiore. Introdotto dal Wi-Fi 4 in poi:

| Banda   | Configurazioni possibili           |
| ------- | ---------------------------------- |
| 2.4 GHz | 20 MHz, 40 MHz                     |
| 5 GHz   | 20, 40, 80, 160 MHz                |
| 6 GHz   | 20, 40, 80, 160, 320 MHz (Wi-Fi 7) |

Quando si combinano canali si eliminano le guard band intermedie, massimizzando le sottoportanti utili per i dati.

## Modulazione

L'[[Orthogonal Frequency Division Multiplexing|OFDM]] (_Orthogonal Frequency Division Multiplexing_) è la tecnica di modulazione usata da tutti gli standard Wi-Fi a partire dal Wi-Fi 3.

### Wi-Fi 3 - 802.11g

**Banda:** 20 MHz unico canale a 2,4 GHz

**Struttura OFDM:**

- 64 sottoportanti totali
- 12 guard band + 4 pilot -> **48 sottoportanti dati**
- Spaziatura: $\Delta f = 20/64 = 312.5$ kHz
- Durata simbolo dati: $1/\Delta f = 3.2$ µs
- _Guard Interval_ (**CP**): 0,8 µs -> durata totale simbolo: **4 µs**
  - CP = 25% della durata simbolo (stimato per ambienti con ritardo ~200 ns)

**Calcolo del bit rate massimo (64-QAM, CR = 3/4):**

$$
R = \frac{48 \cdot 6 \cdot \frac{3}{4}}{4\ \mu\text{s}} = 54\ \text{Mbit/s}
$$

> [!info] Tabella MCS 802.11g
> | Modulazione | Coding Rate | Bit/simbolo/subcarrier | Data Bits/simbolo | Data Rate (Mbit/s) |
> | ----------- | ----------- | ---------------------- | ----------------- | ------------------ |
> | BPSK | 1/2 | 1 | 24 | 6 |
> | BPSK | 3/4 | 1 | 36 | 9 |
> | QPSK | 1/2 | 2 | 48 | 12 |
> | QPSK | 3/4 | 2 | 72 | 18 |
> | 16-QAM | 1/2 | 4 | 96 | 24 |
> | 16-QAM | 3/4 | 4 | 144 | 36 |
> | 64-QAM | 2/3 | 6 | 192 | 48 |
> | 64-QAM | 3/4 | 6 | 216 | **54** |

### Wi-Fi 4 - 802.11n

Wi-Fi 4 introduce cinque miglioramenti rispetto a 802.11g per passare da 54 Mbit/s a **600 Mbit/s** massimi:

1. **[[Onde a radiofrequenza#MIMO - Multiple Input Multiple Output|MIMO]]** (_Multiple Input Multiple Output_): fino a 4 flussi spaziali indipendenti ($4\times4:4$)
2. **Coding rate 5/6** (migliore del $3/4$ di 802.11g: meno overhead di correzione errori)
3. **Guard Interval ridotto:** da 800 ns a **400 ns** (ambienti indoor tipici: ritardo multipath 50–75 ns)
   - Durata simbolo: da 4 µs -> **3,6 µs**
4. **Channel bonding:** due canali da 20 MHz -> canale singolo da **40 MHz** (senza guard band intermedia -> 4 subcarrier in più rispetto al doppio)
5. **Aggregazione frame MAC:** impacchettamento di più frame in un unico PPDU per ridurre l'overhead di intestazione

**Struttura OFDM 802.11n:**

- 20 MHz: 64 subcarrier, 8 guard band, 4 pilot -> **52 subcarrier dati** (+4 rispetto a 802.11g)
- 40 MHz: **108 subcarrier dati**

**Calcolo bit rate massimo (40 MHz, 64-QAM, CR 5/6, GI 400 ns):**

$$
R = \frac{108 \cdot 6 \cdot \frac{5}{6}}{3{,}6\ \mu\text{s}} = 150\ \text{Mbit/s per stream} \rightarrow \times 4\ \text{MIMO} = 600\ \text{Mbit/s}
$$

> [!info] Tabella MCS 802.11n (1 stream)
> | MCS | Modulazione | CR | 20 MHz / 800 ns GI | 20 MHz / 400 ns GI | 40 MHz / 800 ns GI | 40 MHz / 400 ns GI |
> | --- | ----------- | --- | ------------------ | ------------------ | ------------------ | ------------------ |
> | 0 | BPSK | 1/2 | 6,5 | 7,2 | 13,5 | 15 |
> | 3 | 16-QAM | 1/2 | 26 | 28,9 | 54 | 60 |
> | 7 | 64-QAM | 5/6 | 65 | **72,2** | 135 | **150** |

### Wi-Fi 5 - 802.11ac

Wi-Fi 5 opera **solo a 5 GHz** e spinge ulteriormente il bit rate con:

1. **MIMO 8×8:8** (8 stream, contro $4\times4:4$ del Wi-Fi 4)
2. **Channel bonding** fino a **160 MHz** (8 canali da 20 MHz)
3. Riduzione delle pilot subcarrier
4. **MU-MIMO downlink:** l'AP può trasmettere a più utenti simultaneamente sugli stessi stream spaziali (solo DL)

**Struttura subcarrier per larghezza di canale:**

| Banda   | Subcarrier dati | Subcarrier pilot | Guard band |
| ------- | --------------- | ---------------- | ---------- |
| 20 MHz  | 52              | 4                | 8          |
| 40 MHz  | 108             | 6                | 14         |
| 80 MHz  | 234             | 8                | 14         |
| 160 MHz | **468**         | 16               | 28         |

**Calcolo bit rate max (160 MHz, 256-QAM, CR 5/6, GI 400 ns, 1 stream):**

$$
R = \frac{468 \cdot 8 \cdot \frac{5}{6}}{3{,}6\ \mu\text{s}} = 866{,}7\ \text{Mbit/s} \xrightarrow{\times 8\ \text{MIMO}} 6{,}9\ \text{Gbit/s}
$$

### Wi-Fi 6 e 6E - 802.11ax

Wi-Fi 6 rappresenta un cambio architetturale profondo: oltre ad aumentare il picco di velocità, migliora l'**efficienza in ambienti densi** grazie all'[[Orthogonal Frequency Division Multiplexing#OFDMA - Orthogonal Frequency Division Multiple Access|OFDMA]].

**Modifiche al livello PHY rispetto a 802.11ac:**

- Simbolo dati: **12,8 µs** ($\times4$ rispetto a 3,2 µs) -> spaziatura subcarrier: $1/12.8 = 78.125$ kHz
- Guard Interval: tre opzioni: **0,8 µs / 1,6 µs / 3,2 µs**
- In 20 MHz: **256 subcarrier** totali -> **234 subcarrier dati**
- Modulazione massima: **1024-QAM** (10 bit/simbolo)
- **MU-MIMO bidirezionale** (DL e UL), fino a 8 stream simultanei
- **Wi-Fi 6E:** estende lo spettro alla banda **6 GHz** (5,925–7,125 GHz)

**Nuovi tipi di subcarrier introdotti:**

- **DC subcarrier** (direct conversion): subcarrier nulla attorno alla frequenza portante
- **Null subcarrier**, oltre alle pilot e guard band tradizionali

**Target Wake Time (TWT):** I dispositivi IoT negoziano con l'AP le finestre temporali per trasmettere, riducendo drasticamente il consumo energetico standby.

**Calcolo bit rate max (160 MHz, 1024-QAM, CR 5/6, GI 0,8 µs):**

$$
R = \frac{1960 \times 10 \times \frac{5}{6}}{12{,}8 + 0{,}8\ \mu\text{s}} = 1201\ \text{Mbit/s} \xrightarrow{\times 8\ \text{MIMO}} 9{,}6\ \text{Gbit/s}
$$

> [!info] Tabella MCS 802.11ax - selezione (20 MHz, singolo stream)
> | Modulazione | CR | GI 0,8 µs | GI 1,6 µs | GI 3,2 µs |
> | ----------- | --- | --------- | --------- | --------- |
> | BPSK | 1/2 | 8,6 | 8,1 | 7,3 |
> | QPSK | 3/4 | 25,8 | 24,4 | 21,9 |
> | 64-QAM | 3/4 | 77,4 | 73,1 | 65,8 |
> | 256-QAM | 5/6 | 114,7 | 108,3 | 97,5 |
> | 1024-QAM | 5/6 | **143,4** | 135,4 | 121,9 |

### Da OFDM a OFDMA

In [[Orthogonal Frequency Division Multiplexing|OFDM]] (Wi-Fi 3/4/5), l'**intero canale** è assegnato a **un solo utente** per volta, indipendentemente dalla larghezza di banda richiesta. [[Orthogonal Frequency Division Multiplexing#OFDMA - Orthogonal Frequency Division Multiple Access|OFDMA]] (Wi-Fi 6+) divide invece il canale in _Resource Unit_ (**RU**): sottoinsiemi di subcarrier assegnati a utenti diversi nello **stesso istante temporale**.

Questo è lo stesso principio usato nei sistemi mobili **4G/5G (LTE/NR)**.

#### Resource Unit (RU)

Le RU sono aggregati di subcarrier con spaziatura 78,125 kHz:

| Tipo RU | Subcarrier | Banda approssimativa           |
| ------- | ---------- | ------------------------------ |
| RU-26   | 26         | ~2 MHz                         |
| RU-52   | 52         | ~4 MHz                         |
| RU-106  | 106        | ~8 MHz                         |
| RU-242  | 242        | ~20 MHz (intero canale 20 MHz) |
| RU-484  | 484        | ~40 MHz                        |
| RU-996  | 996        | ~80 MHz                        |

**Numero di utenti simultanei per banda:**

| Tipo RU | 20 MHz | 40 MHz | 80 MHz | 160 MHz |
| ------- | ------ | ------ | ------ | ------- |
| RU-26   | 9      | 18     | 37     | 74      |
| RU-52   | 4      | 8      | 16     | 32      |
| RU-106  | 2      | 4      | 8      | 16      |
| RU-242  | 1      | 2      | 4      | 8       |

L'AP assegna dinamicamente le RU ai dispositivi in funzione del tipo di traffico e della QoS negoziata: a un utente che fa streaming video assegna una RU grande, a uno che messaggia su chat ne assegna una piccola.

#### BSS Color

**BSS Coloring** è un meccanismo introdotto in Wi-Fi 6 per migliorare il **riuso spaziale dello spettro** in ambienti densi (es. condomini, uffici open space):

- Ogni BSS riceve un identificativo numerico (**BSS Color**) inserito nel preambolo del PPDU
- Se il segnale ricevuto ha lo **stesso colore** -> appartiene alla propria rete -> la stazione si astiene dal trasmettere
- Se il segnale ha un **colore diverso** (rete vicina interferente) -> se il segnale è abbastanza debole, la stazione lo considera rumore e può trasmettere comunque
- Questo evita che un AP blocchi inutilmente la propria rete per trasmissioni lontane e a bassa potenza

### Wi-Fi 7 - 802.11be

Wi-Fi 7, standardizzato nel **2024**, introduce caratteristiche rivoluzionarie per ambienti ad alta densità e applicazioni a bassa latenza:

| Parametro       | 802.11n    | 802.11ac   | 802.11ax   | 802.11be      |
| --------------- | ---------- | ---------- | ---------- | ------------- |
| Bande           | 2.4/5      | 5          | 2.4/5/6    | **2.4/5/6**   |
| BW max          | 40 MHz     | 160 MHz    | 160 MHz    | **320 MHz**   |
| Modulazione max | 64-QAM     | 256-QAM    | 1024-QAM   | **4096-QAM**  |
| MIMO streams    | 4          | 8          | 8          | **16**        |
| Data rate max   | 600 Mbit/s | 6,9 Gbit/s | 9,6 Gbit/s | **46 Gbit/s** |

**Novità chiave di Wi-Fi 7:**

- **Multi-Link Operation (MLO):** Un dispositivo può collegarsi simultaneamente a più bande (2.4, 5, 6 GHz) con lo stesso AP. Il traffico può essere distribuito tra le bande o duplicato per massimizzare l'affidabilità. Prima era obbligatorio legarsi a una singola banda.
- **Multi-RU Puncturing:** Se una porzione di un canale da 320 MHz è occupata (interferenza da radar o vicino), si esclude solo quella porzione e si utilizza il resto. Non si butta via l'intero canale.
- **Multi-AP Coordination:** Il downlink può essere gestito da AP1 e l'uplink da AP2 in modo coordinato, eliminando la necessità di roaming attivo per ottimizzare le prestazioni.
- **16 x 16 MU-MIMO:** Raddoppia il numero di stream spaziali rispetto a Wi-Fi 6, portandoli a 16.
- **4096-QAM:** Ogni simbolo codifica 12 bit (contro 10 bit della 1024-[[Quadrature Amplitude Modulation|QAM]]). Richiede SNR molto elevato, ma massimizza l'efficienza spettrale.

## Data Rate

La velocità effettiva di un collegamento Wi-Fi è determinata dalla combinazione di cinque parametri (indice **MCS** - _Modulation and Coding Scheme_):

| Parametro          | Effetto sul bit rate                                                                                |
| ------------------ | --------------------------------------------------------------------------------------------------- |
| **Modulazione**    | $\log_2 M$ bit/simbolo per M-QAM; più alta -> più bit ma richiede SNR maggiore                      |
| **Coding Rate**    | Frazione di bit informativi sul totale; più alta -> meno overhead [[Forward Error Correction\|FEC]] |
| **Guard Interval** | GI più corto -> simbolo più breve -> baud rate più alto                                             |
| **Channel Width**  | Più subcarrier -> più dati per simbolo; realizzato con [[#Channel Bonding]]                         |
| **MIMO Streams**   | Moltiplicatore diretto del bit rate (es. x 4 con 4 stream)                                          |

L'indice MCS combina modulazione e coding rate in un numero intero, permettendo all'AP di segnalare rapidamente quale configurazione usare in base alle condizioni del canale.

---

## Livello MAC

Il livello **MAC** del Wi‑Fi genera i propri **frame**, in modo analogo a quanto fa Ethernet, ma con scopi diversi. In particolare, il livello MAC deve:

- gestire l’**accesso al mezzo trasmissivo**;
- definire il **formato dei frame**;
- gestire i meccanismi di **sicurezza**;
- supportare i meccanismi di _risparmio energetico_ (**power saving**).

Nei sistemi Wi‑Fi moderni, la gestione del consumo energetico è molto importante, quindi il MAC ha anche il compito di ridurre l’uso di potenza quando possibile.

### Accesso al canale

Il Wi‑Fi usa un meccanismo di accesso multiplo al canale basato su _Carrier Sense Multiple Access_, più precisamente **CSMA/CA** (_collision avoidance_), al contrario di Ethernet classica che usa **CSMA/CD** (_collision detection_). L’idea di base è questa:

1. prima di trasmettere, una stazione deve **ascoltare il mezzo**;
2. solo se il canale risulta libero può iniziare la procedura di trasmissione;
3. il livello MAC prende i pacchetti dei livelli superiori, ad esempio i pacchetti IP, e li inserisce in un **frame MAC**;
4. il frame viene poi passato al livello fisico per la trasmissione.

Il MAC non usa gli indirizzi IP per la trasmissione sul mezzo radio, ma gli **indirizzi MAC** dei dispositivi coinvolti, ad esempio l’indirizzo dell’**Access Point** a cui il frame deve essere inviato.

### Protocolli di Accesso al Mezzo (CSMA/CA)

Il Wi-Fi utilizza il protocollo **CSMA/CA** (Carrier Sense Multiple Access with Collision Avoidance) per evitare le collisioni, poiché non può rilevarle come fa l'Ethernet con il Collision Detection. L'accesso al canale è regolato da due meccanismi principali:

- **DCF** (_Distributed Coordination Function_): Metodo decentralizzato standard, basato sui tempi di attesa e regole di backoff.
- **PCF** (_Point Coordination Function_): Metodo centralizzato (opzionale) in cui l'Access Point (AP) assegna priorità a specifici flussi, creando un periodo senza contesa (Contention Free Period).

### Tempi di Attesa e Priorità (IFS)

L'accesso al mezzo e le priorità dei pacchetti sono gestiti tramite gli **IFS** (_InterFrame Space_). Più l'IFS è breve, maggiore è la priorità del pacchetto.

- **SIFS** (_Short IFS_): Tempo brevissimo, massima priorità; usato per frame di controllo essenziali come ACK (Acknowledgement), RTS (Request to Send) e CTS (Clear to Send).
- **PIFS** (_PCF IFS_): Tempo intermedio; usato per il traffico in tempo reale (video, voce) sotto la gestione del protocollo PCF.
- **DIFS** (_DCF IFS_): Tempo più lungo, priorità base; usato per il traffico dati standard (es. traffico IP) sotto la gestione del protocollo DCF.

### Regole di Accesso e Meccanismo di Backoff

Per trasmettere un pacchetto dati standard, una stazione deve seguire una precisa sequenza per evitare collisioni simultanee:

1. La stazione verifica se il canale è libero (Carrier Sense).
2. Se occupato, attende che si liberi.
3. Quando il canale si libera, attende obbligatoriamente il tempo **DIFS**.
4. Se il canale è ancora libero dopo il DIFS, la stazione avvia un timer di **Backoff** casuale scelto all'interno di una Contention Window.
5. Il timer scala solo mentre il canale è libero; se qualcun altro inizia a trasmettere, il timer si congela e riprenderà quando il mezzo tornerà libero.
6. Quando il backoff arriva a zero, la stazione trasmette i dati.
7. Se la trasmissione va a buon fine, il destinatario attende un tempo **SIFS** (molto breve) e risponde con un **ACK**.

### Il Problema del Nodo Nascosto e RTS/CTS

Il problema del "nodo nascosto" si verifica quando due stazioni non si possono rilevare tra loro perché troppo lontane, ma entrambe vedono lo stesso AP, finendo per trasmettere contemporaneamente e causando collisioni. Per risolvere questo problema, si utilizza il meccanismo di handshake RTS/CTS.

- La stazione mittente invia un breve frame **RTS** (_Request to Send_).
- L'AP risponde dopo un SIFS con un frame **CTS** (_Clear to Send_) in broadcast.
- Il CTS notifica alla stazione richiedente che può trasmettere e avvisa tutte le altre stazioni (anche quelle nascoste al mittente originale) di rimanere in silenzio.
- La stazione invia i Dati e l'AP conclude inviando un ACK.

### Network Allocation Vector (NAV)

Il **NAV** è un meccanismo di **Virtual Carrier Sense** che permette alle stazioni di risparmiare energia evitando di ascoltare fisicamente il canale in continuazione.

- I frame _RTS_ e _CTS_ contengono la durata prevista dell'intera transazione (dati + ACK).
- Le stazioni in ascolto leggono questo valore e aggiornano il proprio **NAV**, un timer interno.
- Finché il **NAV** non scade, le stazioni considerano il canale virtualmente occupato e non provano a trasmettere, disattivando temporaneamente i circuiti di ascolto.

## Tipi di frame MAC

Nel Wi‑Fi i frame MAC si dividono in **tre categorie principali**:

- Frame di _dati_ (**Data Transfer**), usati per trasportare le informazioni utente.
- Frame di _controllo_ (**Control**), usati per gestire l’accesso al canale.
- Frame di _gestione_ (**management**), usati per autenticazione, associazione, beacon, probe e altre procedure di gestione della rete.

### Frame di controllo

I frame di controllo servono perché nel Wi‑Fi non è possibile rilevare una collisione come in Ethernet mentre si sta trasmettendo. Per questo motivo:

- dopo aver inviato un frame, il mittente si aspetta un **ACK** (**Acknowledgement**);
- se l’ACK non arriva, il frame viene ritrasmesso;
- dopo un certo numero di tentativi falliti, la trasmissione viene interrotta.

I principali frame di controllo sono:

- **ACK**, per confermare che un frame è stato ricevuto correttamente;
- **RTS** (_Request to Send_), per chiedere il permesso di trasmettere;
- **CTS** (_Clear to Send_), inviato dall’Access Point per autorizzare la trasmissione.

L’uso di RTS e CTS non è sempre obbligatorio, perché introduce overhead e quindi consuma banda.

### Frame di gestione

I frame di management servono per la gestione della rete Wi‑Fi. Tra questi ci sono:

- **autenticazione**;
- **associazione** e **disassociazione**;
- gestione delle **chiavi di sicurezza**;
- **beacon**, cioè i frame periodici trasmessi dall’Access Point;
- **probe request** e **probe response**.

## Struttura generale del frame MAC

Il frame MAC del Wi‑Fi contiene diversi campi. I più importanti sono:

- **Frame Control**, che descrive il tipo di frame e varie informazioni di controllo;
- **Duration/Connection ID**, che indica per quanto tempo il mezzo resterà occupato;
- fino a **quattro campi indirizzo**;
- **Sequence Control**, per la gestione dell’ordine dei frame;
- eventuali campi aggiuntivi, come **HT Control**;
- **Frame Body**, cioè i dati veri e propri;
- **FCS** (**Frame Check Sequence**), cioè il controllo finale con **CRC a 32 bit**.

### Frame Control

Nel campo **Frame Control** troviamo informazioni come:

- la **versione del protocollo**;
- il **tipo di frame**: management, control oppure data;
- il **sottotipo**, ad esempio ACK, RTS, CTS, beacon, association request, ecc.;
- i bit **To DS** e **From DS**;
- indicazioni su crittografia, presenza di altri dati e altri flag di controllo.

### Duration/ID e NAV

Il campo **Duration/ID** è importante perché comunica per quanto tempo il mezzo sarà occupato. Le altre stazioni leggono questa informazione e aggiornano un contatore interno chiamato **NAV** (**Network Allocation Vector**).

Il **NAV** serve a far capire alle stazioni che il canale deve essere considerato occupato per quel certo intervallo di tempo, anche se in quel momento non stanno fisicamente ascoltando una trasmissione utile per loro.

### I quattro indirizzi

Una delle particolarità del frame MAC del Wi‑Fi è che può contenere fino a **quattro indirizzi**.

In Ethernet di solito bastano:

- **Destination Address (DA)**;
- **Source Address (SA)**.

Nel Wi‑Fi, invece, la situazione è più complessa perché i frame possono passare attraverso un **Access Point** e, in alcuni casi, anche tra **due Access Point**.

#### Caso diretto: stazione verso Access Point

Se un client deve trasmettere un frame verso la rete tramite un Access Point, nel frame compaiono tipicamente tre indirizzi:

- **Address 1**: il destinatario radio immediato, cioè l’Access Point (**Receiver Address / BSSID**).
- **Address 2**: la stazione che sta trasmettendo, cioè il mittente radio (**Transmitter Address / Source Address**).
- **Address 3**: la destinazione finale del frame nella rete.

In pratica:

- il frame viene mandato via radio all’Access Point;
- l’Access Point poi lo inoltra verso la destinazione finale.

#### Caso opposto: Access Point verso stazione

Se invece il frame arriva dalla rete verso il client tramite l’Access Point, i ruoli degli indirizzi cambiano:

- un indirizzo identifica il destinatario radio immediato, cioè la stazione;
- un altro identifica l’Access Point che sta trasmettendo via radio;
- un altro ancora rappresenta la sorgente finale del traffico.

Per questo motivo, nel Wi‑Fi i campi di indirizzamento sono più flessibili rispetto a Ethernet.

#### Il quarto indirizzo

Il **quarto indirizzo** serve in scenari particolari, ad esempio quando due Access Point sono collegati **via wireless** tra loro, cioè in modalità **wireless bridge** o **WDS**.

In questo caso bisogna distinguere:

- il mittente radio;
- il destinatario radio;
- la sorgente finale;
- la destinazione finale.

Per questo possono servire tutti e quattro gli indirizzi.

### Significato di To DS e From DS

I bit **To DS** e **From DS** servono a indicare la direzione del frame rispetto al **Distribution System (DS)**:

- **To DS = 1**: il frame sta andando verso il Distribution System.
- **From DS = 1**: il frame proviene dal Distribution System.

I casi tipici sono:

- **To DS = 1, From DS = 0**: stazione che invia verso l’Access Point.
- **To DS = 0, From DS = 1**: Access Point che invia verso la stazione.
- **To DS = 1, From DS = 1**: collegamento tra Access Point in modalità bridge wireless, quindi può servire anche il quarto indirizzo.
- **To DS = 0, From DS = 0**: comunicazioni che non coinvolgono il DS, ad esempio alcuni frame di management o reti ad hoc.

---

## Accesso alla rete

### Procedure di Scanning

Per potersi connettere a una rete wireless, una stazione deve prima scoprire quali Access Point sono disponibili nelle vicinanze. Questo processo si chiama scanning e può avvenire in due modalità distinte, a seconda delle impostazioni del sistema operativo e delle priorità energetiche.

#### Scanning passivo

Lo scanning passivo avviene quando il client si sintonizza sui vari canali radio e si mette semplicemente in ascolto.

L'Access Point trasmette periodicamente (di solito ogni 100 millisecondi) frame di _management_ chiamati Beacon, contenenti il SSID, le velocità supportate e i parametri di rete.

Il vantaggio principale dello scanning passivo è il notevole risparmio energetico, che lo rende ideale per i dispositivi alimentati a batteria.

Lo svantaggio dello scanning passivo è la lentezza, poiché il client deve sostare su ogni canale per un tempo sufficiente ad intercettare un eventuale Beacon.

#### Scanning attivo

Lo scanning attivo prevede invece che il client prenda l'iniziativa, inviando su tutti i canali dei frame di management chiamati Probe Request.

Gli Access Point in ascolto che soddisfano i criteri della richiesta rispondono direttamente alla stazione con un frame di Probe Response.

Questo metodo è molto più veloce e permette connessioni rapide, ma comporta un maggiore consumo di energia per la trasmissione attiva.

Lo scanning attivo espone maggiormente la privacy del dispositivo, poiché spesso i client trasmettono in chiaro i nomi delle reti a cui si sono connessi in passato per cercarle nell'etere.

### Autenticazione e Sicurezza

Dopo aver individuato la rete, la stazione deve completare il processo di autenticazione e la successiva associazione all'Access Point. Nel tempo, lo standard IEEE 802.11 ha introdotto diverse tecnologie per garantire l'accesso sicuro al mezzo condiviso.

#### Rete aperta

L'autenticazione **Open System** è il metodo basilare dove non avviene alcuna verifica crittografica delle credenziali a livello MAC.

In un sistema aperto la stazione invia la richiesta e l'Access Point l'accetta sempre, delegando la reale sicurezza ai livelli superiori della rete (come un Captive Portal o l'uso di una VPN).

#### Wired Equivalent Privacy

Il protocollo **WEP** (_Wired Equivalent Privacy_) è lo standard di sicurezza originale, oggi deprecato e totalmente insicuro a causa di gravi vulnerabilità nella gestione dei vettori di inizializzazione (IV) dell'algoritmo RC4.

Con il WEP, un attaccante in ascolto passivo può decifrare la chiave di rete semplicemente catturando un numero sufficiente di pacchetti dati perché rimane la stessa durante tutta la sessione.

#### Wi-Fi Protected Access

Il protocollo **WPA** (_Wi-Fi Protected Access_) è stato introdotto come soluzione ponte per sostituire il WEP utilizzando lo stesso hardware, implementando il **TKIP** per variare dinamicamente le chiavi di cifratura.

Il **WPA2** rappresenta l'attuale standard di base per la sicurezza wireless, introducendo l'uso obbligatorio dell'algoritmo di crittografia forte AES-CCMP.

Il **WPA3** è lo standard di ultima generazione, progettato per bloccare gli attacchi a dizionario offline grazie al protocollo di handshake **SAE** (_Simultaneous Authentication of Equals_) e per garantire la Forward Secrecy.

Le implementazioni WPA si dividono in Personal, basate su una password condivisa (PSK), ed Enterprise, che richiedono un server RADIUS per autenticare individualmente ogni utente della rete.

---

## Power Management nel Wi-Fi

Poiché l'AP è generalmente collegato alla rete elettrica mentre i dispositivi client vanno a batteria, il Wi-Fi prevede funzioni di risparmio energetico (Power Save).

- Una stazione invia un frame all'AP impostando un flag specifico nel livello MAC per avvisare che sta entrando in **Sleep Mode**.
- Da quel momento, l'AP non invia più pacchetti a quella stazione, ma li memorizza nei propri buffer interni.
- Quando la stazione si "sveglia" periodicamente, interroga l'AP per verificare se ci sono dati in sospeso e, in caso affermativo, richiede la trasmissione dei pacchetti bufferizzati.
