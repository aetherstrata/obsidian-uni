---
tags:
  - wireless
  - reti
  - iot
  - protocollo
---

LoRa/LoRaWAN è una tecnologia **LPWAN** (_Low Power Wide Area Network_) progettata specificamente per l'_Internet of Things_ (**IoT**): connettere sensori a batteria distribuiti su aree geografiche molto estese, con requisiti di brevissimi messaggi inviati sporadicamente e autonomia della batteria di **oltre 10 anni**.

> [!tip] Vantaggi
> Le tecnologie Wi-Fi e [[Bluetooth]] coprono al massimo qualche centinaio di metri. LoRa invece può coprire **fino a 15 km in ambienti rurali** e **3-5 km in ambienti urbani** con consumo energetico minimo. È ideale per misuratori d'acqua, gas, elettricità, sensori anti-incendio nei boschi, tracker GPS per animali e smart city.

**Confronto tra tecnologie IoT LPWAN:**

| Parametro      | LoRaWAN          | Sigfox           | NB-IoT           |
| -------------- | ---------------- | ---------------- | ---------------- |
| Range tipico   | 2-15 km          | 3-50 km          | ~10 km           |
| Data rate max  | 50 kb/s          | 100 b/s          | 250 kb/s         |
| Consumi        | µA in sleep      | µA in sleep      | mA in sleep      |
| Infrastruttura | Privata/pubblica | Operatore Sigfox | Operatore TLC    |
| Frequenza EU   | 868 MHz          | 868 MHz          | Bande licenziate |

> [!info] Distinzione con LoRa
>
> **LoRa** e **LoRaWAN** indicano due cose diverse, ed è un punto critico per comprendere questa tecnologia:
>
> - **LoRa** (Long Range): tecnologia **proprietaria di Semtech**, protetta da brevetto. È il **livello fisico (PHY)**, ovvero la tecnica di modulazione radio basata sul **Chirp Spread Spectrum (CSS)**. Solo il collegamento radio tra nodo e gateway è brevettato da Semtech.
> - **LoRaWAN**: il protocollo **MAC (livello 2)**, aperto e gestito dalla **LoRa Alliance**. Definisce l'architettura di rete, la sicurezza, le classi di dispositivi e la gestione delle frequenze. Dal gateway verso il server di rete, la comunicazione avviene in **TCP/IP** standard su Internet (Ethernet, 4G, Wi-Fi, fibra).
>
> ![[Pasted image 20260624204305.png]]

## Frequenze di utilizzo

LoRaWAN usa le bande **ISM** (_Industrial, Scientific, Medical_), libere da licenza:

| Regione     | Frequenza         | Limitazione                              |
| ----------- | ----------------- | ---------------------------------------- |
| **Europa**  | **868 / 433 MHz** | **Duty Cycle $\le$ 1%** (ETSI EN300.220) |
| Stati Uniti | 915 MHz           | Limite di potenza                        |
| Asia        | 923 MHz           | Duty Cycle $\le$ 10%                     |

Le frequenze basse (sub-GHz) garantiscono una **maggiore portata** rispetto ai 2,4 GHz di Wi-Fi e Bluetooth, grazie alla minore attenuazione in aria e alla migliore penetrazione degli ostacoli.

### Duty Cycle

**Duty Cycle in Europa** (ETSI EN300.220-2): il tempo in cui un dispositivo può trasmettere è limitato per evitare interferenze sulla banda condivisa. La regolamentazione europea definisce sotto-bande specifiche:

- **868,0-868,6 MHz**: duty cycle $\le$ **1%**
- **869,4-869,65 MHz**: duty cycle $\le$ **10%**
- **863-865 MHz**: duty cycle $\le$ **0,1%**

Questo significa che un dispositivo sulla banda a 1% può trasmettere **al massimo 36 secondi per ora**. Questo limita ulteriormente la frequenza di invio dei messaggi e obbliga a progettare il sistema con payload piccoli e trasmissioni rare.

#### Canali di default EU868

Questi canali sono obbligatori per ogni dispositivo LoRaWAN:

- 868,10 MHz, 868,30 MHz, 868,50 MHz - tutti con bandwidth 125 kHz, DR0-DR5

## Architettura LoRaWAN

L'architettura LoRaWAN è **gerarchica a stella** ed è composta da quattro elementi:

### End Devices

Sensori IoT a bassissima potenza. Contengono: sensori fisici (temperatura, umidità, GPS, accelerometro), un **transponder LoRa** (chip Semtech) e opzionalmente un microcontrollore con memoria. Trasmettono in **broadcast** - non si connettono a un singolo gateway come nella rete cellulare, ma i loro pacchetti possono essere ricevuti da **più gateway contemporaneamente**.

### Gateway

Ricevono i segnali RF dai nodi e li convertono in **pacchetti IP** da inoltrare al Network Server tramite Ethernet, 4G o Wi-Fi. I gateway:

- Operano **solo a livello fisico (PHY)**
- Verificano l'integrità dei dati tramite **CRC** - se il CRC non è corretto, il pacchetto viene scartato
- Aggiungono ai pacchetti i metadati: **RSSI** (potenza del segnale ricevuto) e **timestamp**
- Ascoltano su **più canali contemporaneamente** (antenna diversity), aumentando l'affidabilità
- Sono sempre alimentati dalla rete elettrica

> [!imortant] **Differenza chiave rispetto alla rete cellulare**
> In un sistema cellulare ogni terminale è connesso a un unico gateway (BTS). In LoRaWAN, uno stesso nodo può essere ricevuto da più gateway simultaneamente, e sarà il **Network Server** a fare deduplicazione.

### Network Server

Cuore della rete LoRaWAN, è tipicamente ospitato su **cloud** (es. _The Things Network_ - **TTN**, **LORIOT**). Gestisce:

- **Deduplicazione** dei messaggi ricevuti da più gateway
- **Controllo accessi** e instradamento verso l'Application Server appropriato
- **Adaptive Data Rate (ADR)**: regola dinamicamente SF, bandwidth e potenza di trasmissione di ogni dispositivo
- **Downlink**: invia comandi verso i nodi attraverso il gateway più adatto

### Join Server

Componente specializzata del Network Server che gestisce la **procedura di attivazione** dei dispositivi (OTAA). Deriva le chiavi di sessione e informa il Network Server dell'applicazione associata.

### Application Server

Elabora i dati applicativi (decifra il payload con AppSKey), li presenta all'utente finale e genera eventuali comandi downlink.

## Modulazione Chirp Spread Spectrum

### Principio fisico

Il CSS è la tecnica di modulazione brevettata da Semtech che costituisce il livello fisico LoRa. Un **chirp** è un segnale la cui frequenza varia linearmente nel tempo:

$$
u(t) = U_0 \cos\!\left(2\pi f_0 t + \pi \frac{B}{T} t^2\right), \quad 0 \le t \le T
$$

dove $B$ è la larghezza di banda e $T$ la durata del simbolo. La derivata della fase è la frequenza istantanea $f(t) = f_0 + \frac{B}{T} t$, che cresce linearmente.

- **Up-chirp**: frequenza che **aumenta** da fminf*{min}fmin​ a fmaxf*{max}fmax​ -> usato per l'**uplink** (nodo -> gateway)
- **Down-chirp**: frequenza che **diminuisce** da $f_{max}$​ a $f_{min}$​ -> usato per il **downlink** (gateway -> nodo) e per la sincronizzazione nel preambolo

Questo approccio è identico a quello usato nei **radar e sonar** da decenni: l'integrazione in frequenza del segnale ricevuto permette di estrarlo anche **20 dB al di sotto del rumore** (SNR negativo), cosa impossibile con una portante a frequenza fissa.

### Codifica dei simboli

La **frequenza iniziale** del chirp (cioè il punto di partenza della rampa) codifica il simbolo. Con un alfabeto di MMM simboli, lo **Spreading Factor** è:

$$
SF = \log_2(M)
$$

La banda è fissa $B = 125$ kHz (o 500 kHz). La durata del simbolo è:

$$
T_s = \frac{2^{SF}}{B} = \frac{M}{B}
$$

LoRaWAN usa **SF da 7 a 12** (6 valori), corrispondenti a $M$ da 128 a 4096 simboli:

| SF  | M    | Ts (B=125kHz) | Bit rate (tipico) |
| --- | ---- | ------------- | ----------------- |
| 7   | 128  | 1,024 ms      | 5470 b/s          |
| 8   | 256  | 2,048 ms      | 3125 b/s          |
| 9   | 512  | 4,096 ms      | 1760 b/s          |
| 10  | 1024 | 8,192 ms      | 980 b/s           |
| 11  | 2048 | 16,384 ms     | 440 b/s           |
| 12  | 4096 | 32,768 ms     | 250 b/s           |

Ogni incremento di SF **raddoppia** la durata del simbolo e **dimezza circa il bit rate**.

### Calcolo del Bit Rate

Il bit rate fisico (senza FEC) è:

$$
R_b = \frac{SF \cdot B}{2^{SF}} = \frac{SF}{T_s}
$$

Il bit rate effettivo, tenendo conto del **Coding Rate (CR)** per la correzione degli errori (FEC):

$$
R_{b,eff} = \frac{B \cdot SF}{2^{SF}} \cdot \frac{4}{4 + CR}
$$

Il **Coding Rate** può assumere i valori: **4/5** (default), 4/6, 4/7, 4/8 (il denominatore CR indica i bit aggiuntivi di ridondanza inseriti ogni 4 bit di dati).

**Tabella Data Rate standard EU868:**

| DR  | Config.     | Bit rate fisico | Max Payload MAC | Max Payload frame |
| --- | ----------- | --------------- | --------------- | ----------------- |
| DR0 | SF12 125kHz | 250 b/s         | 59 B            | 51 B              |
| DR1 | SF11 125kHz | 440 b/s         | 59 B            | 51 B              |
| DR2 | SF10 125kHz | 980 b/s         | 59 B            | 51 B              |
| DR3 | SF9 125kHz  | 1760 b/s        | 123 B           | 115 B             |
| DR4 | SF8 125kHz  | 3125 b/s        | 230 B           | 222 B             |
| DR5 | SF7 125kHz  | 5470 b/s        | 230 B           | 222 B             |
| DR6 | SF7 250kHz  | 11000 b/s       | 230 B           | 222 B             |
| DR7 | FSK 50kb/s  | 50000 b/s       | 230 B           | 222 B             |

### Trade-off SF

SF alto -> simbolo più lungo -> ricevitore ha più tempo per integrare il segnale -> **maggiore portata** (fino a ~15 km) e resistenza al rumore (SNR fino a -20 dB).

Ma SF alto -> **più tempo di trasmissione -> più energia consumata** e duty cycle occupato più a lungo.

L'**Adaptive Data Rate (ADR)** seleziona automaticamente il **minimo SF possibile** compatibile con la qualità del link: dispositivi vicini al gateway usano SF7 (batteria dura di più, bit rate più alto); dispositivi lontani usano SF12 (portata massima).

### Ortogonalità tra Spreading Factor

Due segnali chirp con **SF diverso** trasmessi **sullo stesso canale nello stesso istante** sono **matematicamente ortogonali** tra loro (come i codici CDMA) e non si interferiscono. Un singolo gateway può quindi decodificare simultaneamente pacchetti con SF diversi.

La capacità in parallelo per SF è proporzionale a $2^{SF_{max}-SF}$:

- $1 \times \text{SF12}$,
- $2 \times \text{SF11}$,
- $4 \times \text{SF10}$,
- $8 \times \text{SF9}$,
- $16 \times \text{SF8}$,
- $32 \times \text{SF7}$

### Robustezza al Multipath e Doppler

Il CSS è robusto al **multipath fading** perché lo spettro del segnale è ampio (l'energia è distribuita su 125 kHz). L'**effetto Doppler** introduce uno shift in frequenza che, trasposto in banda base, diventa uno shift trascurabile del chirp: il sistema rimane funzionante anche con sensori in movimento (es. tracker GPS su animali).

---

## Formato del Pacchetto LoRa

Il pacchetto LoRa è progettato per la semplicità:

`| Preambolo | Header (opzionale) | Payload | CRC (opzionale) |`

- **Preambolo**: sequenza di **up-chirp** seguiti da alcuni **down-chirp**, usata per la sincronizzazione del clock e l'aggancio del ricevitore
- **Header (opzionale)**: indica lunghezza del payload, coding rate, presenza del CRC. Se omesso, questi parametri devono essere configurati manualmente su entrambi i lati
- **Payload**: i dati utili, dimensione variabile (vedi tabella DR)
- **CRC** (_opzionale_): controllo di integrità; non sempre presente nei pacchetti uplink, quasi mai nel downlink

---

## Classi di Dispositivi LoRaWAN

Le tre classi definiscono il **profilo di ricezione** di un dispositivo, bilanciando consumo energetico e latenza del downlink:

### Classe A - Massima efficienza

Questa è la classe con la _massima efficienza_, è obbligatoria per tutti i dispositivi.

Il dispositivo dorme quasi sempre. Si sveglia **quando lo decide lui** (trasmissione uplink in istanti casuali), poi apre **due finestre di ricezione downlink** a tempi fissi dopo la trasmissione:

- **RX1**: stessa frequenza dell'uplink, dopo ~1 s
- **RX2**: frequenza e DR fissi (869,525 MHz, DR0 in EU868), dopo ~2 s

Se il gateway vuole inviare un downlink, deve aspettare che il nodo trasmetta e sfruttare una di queste finestre. Il gateway conosce l'offset temporale e si sincronizza.

**Applicazioni**: sensori ambientali, contatori, tracker, qualsiasi applicazione dove il downlink non è time-critical.

### Classe B - Ricezione pianificata

Funziona come la Classe A, ma aggiunge **finestre di ricezione periodiche e programmate** sincronizzate tramite **beacon GPS** inviati da tutti i gateway. I gateway trasmettono beacon ogni $2^n$ secondi ($n = 0,1,\ldots,7$, quindi ogni 1, 2, 4, 8, 16, 32, 64 o 128 s), mantenendo sincronizzati i dispositivi.

**Applicazioni**: valvole intelligenti, attuatori dove il downlink deve avvenire entro un intervallo prevedibile.

### Classe C - Ricezione continua

Il dispositivo è **quasi sempre in ricezione**, eccetto quando trasmette. Richiede alimentazione da rete. Ha la **latenza downlink minima** ma il **consumo più elevato**.

**Applicazioni**: lampioni intelligenti, contatori elettrici industriali, qualsiasi dispositivo alimentato da rete.

**Schema temporale comparativo:**

```
Classe A:  [Sleep]─[TX]─[RX1]─[RX2]─[Sleep]─[Sleep]─[Sleep]
Classe B:  [Sleep]─[TX]─[RX1]─[Sleep]─[Beacon RX]─[Slot RX]─[Sleep]
Classe C:  [RX]──────────[TX]─[RX1/RX2]─[RX]────────────────[RX]
```

---

## Sicurezza LoRaWAN

### Identificatori del dispositivo

Ogni dispositivo LoRaWAN possiede tre identificatori univoci:

| ID                   | Dimensione      | Descrizione                                                                           |
| -------------------- | --------------- | ------------------------------------------------------------------------------------- |
| **DevEUI**           | 8 byte (EUI-64) | Identificatore globale univoco del dispositivo (simile al MAC address IEEE)           |
| **AppEUI / JoinEUI** | 8 byte (EUI-64) | Identifica il Join Server/Application Server                                          |
| **AppKey**           | 16 byte         | Chiave radice AES-128, **segreta**, nota solo al dispositivo e all'Application Server |

### Doppia cifratura AES-128

La sicurezza LoRaWAN si basa su **due chiavi di sessione distinte**, derivate dall'AppKey durante la procedura di join:

- **NwkSKey** (Network Session Key, 128 bit): usata tra nodo e **Network Server**. Calcola il **MIC (Message Integrity Code)** a 4 byte su ogni frame (algoritmo AES-CMAC) -> garantisce integrità e autenticità.
- **AppSKey** (Application Session Key, 128 bit): usata tra nodo e **Application Server**. Cifra il **payload** con AES-128 in modalità CTR -> garantisce confidenzialità end-to-end. Nemmeno il gateway e il Network Server possono leggere il payload.

Questo schema è chiamato **doppia crittografia**: anche se qualcuno intercettasse i pacchetti radio, non potrebbe né leggere il payload (senza AppSKey) né modificarlo passando il controllo di integrità (senza NwkSKey).

### Procedure di Attivazione

**OTAA (Over-The-Air Activation)** - raccomandata per la maggior parte dei deployment:

1. Il dispositivo invia un **Join Request** al server (contiene DevEUI, AppEUI, DevNonce casuale)
2. Il Join Server verifica l'AppKey e risponde con un **Join Accept** cifrato
3. Entrambe le parti derivano le chiavi di sessione NwkSKey e AppSKey dall'AppKey + nonce
4. Al prossimo join le chiavi vengono **rigenerate** -> sicurezza forward secrecy

**ABP (Activation By Personalization)** - più semplice ma meno sicura:

- NwkSKey, AppSKey e DevAddr sono pre-configurate nel firmware del dispositivo
- Le chiavi sono **statiche** e non cambiano tra sessioni
- I frame counter si azzerano ad ogni reset -> rischio di replay attacks
- Usata principalmente per sviluppo e test

### Protezione anti-replay

Ogni messaggio include un **Frame Counter (FCnt)** che si incrementa ad ogni trasmissione. Il Network Server rifiuta messaggi con FCnt inferiore o uguale all'ultimo ricevuto, impedendo attacchi di **replay** (ritrasmissione fraudolenta di messaggi intercettati).

---

## Tabella Comparativa

| Parametro          | LoRaWAN       | Bluetooth Classic | BLE             | Wi-Fi               |
| ------------------ | ------------- | ----------------- | --------------- | ------------------- |
| Standard           | LoRa Alliance | IEEE 802.15.1     | IEEE 802.15.1   | IEEE 802.11         |
| Modulazione        | CSS (chirp)   | GFSK / DPSK       | GFSK            | OFDM / QAM          |
| Frequenza EU       | 868 / 433 MHz | 2,4 GHz           | 2,4 GHz         | 2,4 / 5 / 6 GHz     |
| Range tipico       | 2-15 km       | 10-100 m          | 10-400 m        | <100 m              |
| Data rate          | 0,25-50 kb/s  | 1-3 Mb/s          | 125 kb/s-2 Mb/s | fino a Gb/s         |
| Consumo sleep      | ~µA           | -                 | ~µA             | ~mA                 |
| Topologia          | Stella        | Stella (piconet)  | Stella/Mesh     | Infrastruttura/Mesh |
| Sicurezza          | AES-128 ×2    | E0 stream cipher  | AES-128 ECDH    | WPA3/AES            |
| Autonomia batteria | 10+ anni      | ore/giorni        | mesi/anni       | ore                 |

---

## Domande Frequenti

1. **Qual è la differenza tra LoRa e LoRaWAN?** LoRa è il livello fisico (modulazione CSS, brevettato Semtech); LoRaWAN è il protocollo MAC (aperto, LoRa Alliance). Solo il collegamento nodo-gateway usa LoRa; dal gateway in poi si usa TCP/IP.
2. **Perché CSS permette di ricevere segnali sotto il rumore?** Perché la frequenza del chirp spazia su tutta la larghezza di banda. Il ricevitore integra l'energia su tutti i $2^{SF}$ campioni del simbolo: più grande è SF, più lunga è l'integrazione, più è facile estrarre il segnale dal rumore (SNR fino a -20 dB).
3. **Come si codifica l'informazione in LoRa?** Con la **frequenza iniziale** del chirp. Un simbolo su alfabeto di $M = 2^{SF}$ simboli ha un diverso punto di partenza della rampa in frequenza. Tutti i simboli hanno la stessa pendenza $B/T_s$​: questo li rende facilmente distinguibili.
4. **Perché all'aumentare di SF il bit rate diminuisce?** Perché $T_s = 2^{SF}/B$ cresce esponenzialmente con SF, mentre il numero di bit per simbolo cresce solo linearmente ($\log_2 M = SF$). Il risultato netto è $R_b \propto SF/2^{SF}$, che decresce.
5. **Perché i gateway LoRa possono ricevere da più nodi contemporaneamente?** Perché segnali con SF diversi sono **ortogonali**. Un gateway multi-canale può demodulare in parallelo 6 simboli con SF diversi sullo stesso canale, più pacchetti su canali diversi.
6. **Cosa fa il Network Server in LoRaWAN?** Deduplicazione (se più gateway ricevono lo stesso pacchetto), ADR, instradamento verso l'Application Server, gestione dei downlink, verifica dell'integrità MIC con NwkSKey.
7. **Qual è la differenza tra OTAA e ABP?** OTAA rigenera le chiavi di sessione ad ogni join usando l'AppKey come radice (più sicura). ABP ha chiavi statiche precaricate nel firmware (più semplice, usata per debug).
