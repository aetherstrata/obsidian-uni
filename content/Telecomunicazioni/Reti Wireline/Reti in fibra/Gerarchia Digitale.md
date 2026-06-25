---
tags:
  - fibra
  - physical
  - multiplexing
  - reti
  - wireline
---

Il punto di partenza di entrambe le gerarchie è la **Pulse Code Modulation** ([[Pulse Code Modulation|PCM]]), tecnica di digitalizzazione del segnale analogico vocale. Il processo avviene in tre fasi:

1. **Campionamento**: il segnale analogico viene campionato a 8 kHz (${}\ge 2\times4$ kHz per il [[Telecomunicazioni/Segnali/Operazioni e Proprietà/Teorema del Campionamento|Teorema del Campionamento]]), ottenendo un campione ogni **125 μs**
2. **Quantizzazione**: ogni campione viene approssimato a uno dei $2^8 = 256$ livelli possibili, codificato su **8 bit**
3. [[Time Division Multiplexing|TDM]] (**Time Division Multiplexing**): i campioni di più utenti vengono interallacciati (round robin) nello stesso canale fisico per formare una gerarchia digitale

Il canale elementare risultante si chiama **DS0** (Digital Signal Level 0) e ha un bit rate di **64 kbit/s** (8 bit × 8000 campioni/s).

![[Pasted image 20260616210531.png]]

---

## PDH - Plesiochronous Digital Hierarchy

La PDH è la prima gerarchia digitale di trasporto sviluppata negli anni '70. Il termine _plesiocrono_ (dal greco "quasi-sincrono") indica che ogni nodo ha un proprio clock locale, con frequenza nominalmente uguale ma non perfettamente sincronizzata con gli altri. Per compensare le piccole differenze di fase e frequenza si usa il meccanismo del **bit stuffing**: vengono inseriti bit di riempimento per allineare i flussi tributari prima del multiplexing.

### Gerarchia T/E - Frame DS1 e E1

Il Frame DS1 (nord America) si ottiene multiplando **24 canali DS0** e aggiungendo 1 **F-bit** (framing bit) per il controllo degli errori:

$$
\text{Bit rate DS1} = (24 \times 8 + 1) \text{ bit} \div 125\,\mu s = 1{,}544\,\text{Mb/s}
$$

In Europa il frame **E1** trasporta 30 canali voce + 2 canali di segnalazione:

$$
\text{Bit rate E1} = 30 \times 64\,\text{kbit/s} + 128\,\text{kbit/s (overhead)} = 2{,}048\,\text{Mb/s}
$$

| Parametro               | T1/DS1 (Nord America) | E1 (Europa)         |
| ----------------------- | --------------------- | ------------------- |
| Canali voce PCM         | 24 + 1 segnalazione   | 30 + 2 segnalazione |
| Bit per campione        | 8 bit                 | 8 bit               |
| Frequenza campionamento | 8000 camp/s           | 8000 camp/s         |
| Bit rate totale         | 1,544 Mb/s            | 2,048 Mb/s          |
| Frame rate              | 8000 frame/s          | 8000 frame/s        |

### Gerarchie complete PDH

Il multiplexing sale di livello per aggregare più flussi. In Nord America si usa **bit interleaving** a partire dal DS2 (un bit di controllo ogni 48 bit di payload); in Europa si usa **byte interleaving** per l'E1 e poi bit interleaving per i livelli superiori:

| Livello | Nord America | Mb/s    | Canali | Europa | Mb/s    | Canali |
| ------- | ------------ | ------- | ------ | ------ | ------- | ------ |
| 1       | T1/DS1       | 1,544   | 24     | E1     | 2,048   | 32     |
| 2       | T2/DS2       | 6,312   | 96     | E2     | 8,448   | 128    |
| 3       | T3/DS3       | 44,736  | 672    | E3     | 34,368  | 512    |
| 4       | DS4          | 274,176 | 4032   | E4     | 139,264 | 2048   |
| 5       | DS5          | 400,352 | 5760   | E5     | 564,992 | 8192   |

> [!info] Gerarchie US
> ![[Pasted image 20260616210739.png]]

> [!info] Gerarchie EU
> ![[Pasted image 20260616210823.png]]

### Limiti della PDH

Le principali limitazioni che hanno portato allo sviluppo di SDH sono:

- **Impossibilità di estrarre canali tributari** direttamente da un flusso ad alta velocità senza demultiplare l'intera gerarchia
- **Capacità di gestione insufficiente**: la maggior parte degli elementi di gestione PDH è proprietaria e non standardizzata
- **Tre standard incompatibili** (Nord America, Europa, Giappone) senza interoperabilità diretta
- **Nessuna standardizzazione** oltre i 140 Mbit/s
- **Nessuna sincronizzazione globale**: il bit stuffing introduce instabilità e jitter nell'estrazione dei canali

---

## SDH - Synchronous Digital Hierarchy

L'SDH (e il suo equivalente americano **SONET** - _Synchronous Optical NETworking_) risolve tutti i limiti della PDH introducendo una **sincronizzazione globale**: tutti gli elementi di rete condividono lo stesso clock di riferimento. Lo standard SDH è definito dall'**ITU** con le raccomandazioni G.707, G.783, G.784, G.803; SONET è standardizzato dall'ANSI con T1.105.

In Italia, TIM (Telecom) introdusse la rete SDH nel **1996**.

### Architettura e componenti di rete

Una rete SONET/SDH è composta da elementi specifici che operano a livelli diversi:

- **Terminal (T)**: terminale che origina o termina un percorso end-to-end
- **STS MUX / DEMUX**: multiplexer/demultiplexer del segnale di trasporto sincrono
- **Regenerator (R)**: rigeneratore del segnale ottico lungo la tratta
- **ADM (Add/Drop Multiplexer)**: nodo che estrae e inserisce flussi tributari senza demultiplare l'intero segnale

![[Pasted image 20260616210948.png]]

### Stratificazione SDH/SONET

La rete è organizzata in **quattro strati gerarchici**, ognuno responsabile di funzioni precise:

| Strato                | Funzione principale                                                                           |
| --------------------- | --------------------------------------------------------------------------------------------- |
| **Path (Cammino)**    | Gestione end-to-end della connessione; inserisce l'overhead di percorso (POH) nel payload     |
| **Line (Linea)**      | Sincronizzazione e multiplazione di più path-layer; protezione e recupero guasti tra due nodi |
| **Section (Sezione)** | Framing, scrambling, controllo errori tra rigeneratori adiacenti; overhead RSOH e MSOH        |
| **Physical (Fisico)** | Conversione elettro-ottica e trasmissione di impulsi luminosi                                 |

Questa stratificazione corrisponde all'aggiunta incrementale di **overhead** durante la discesa attraverso i layer. Il path overhead è visibile solo agli elementi terminali del percorso, mentre il section overhead viene processato da ogni rigeneratore.

![[Pasted image 20260616211046.png]]

### Frame STS-1 (SONET) e STM-1 (SDH)

L'unità base di trasmissione SONET è il frame **STS-1** (Synchronous Transport Signal level 1), trasmesso ogni **125 μs** a 8000 frame/s:

- Struttura: **matrice 9 righe x 90 colonne = 810 byte**
- Prime **3 colonne** (27 byte): overhead di sezione (_SOH_ - 9byte) e linea (_LOH_ - 18byte)
- **Colonna 4** (9 byte): path overhead (_POH_)
- Restanti **756 byte**: payload effettivo -> efficienza = **93,33%**

$$
\text{Bit rate STS-1} = \frac{810 \times 8}{125 \times 10^{-6}} = 51{,}84\,\text{Mb/s}
$$

![[Pasted image 20260616211659.png]]

Il frame viene trasmesso **riga per riga**, da sinistra a destra.

Il frame **STM-1** (SDH) è la versione europea equivalente a STS-3 di SONET e ha struttura di **9 righe × 270 colonne**, con le prime 9 colonne dedicate a Section Overhead (_SOH_ nelle prime 3 righe, _LOH_ nelle ultime 5 righe) e le restanti 261 colonne al **Virtual Container (VC)**.

### Gerarchia SONET/SDH e bit rate

| OC (Ottico) | SONET   | SDH       | Mb/s      |
| ----------- | ------- | --------- | --------- |
| OC-1        | STS-1   | -         | 51,84     |
| OC-3        | STS-3   | **STM-1** | 155,52    |
| OC-12       | STS-12  | STM-4     | 622,08    |
| OC-48       | STS-48  | STM-16    | 2.488,32  |
| OC-192      | STS-192 | STM-64    | 9.953,28  |
| OC-768      | STS-768 | STM-256   | 39.813,12 |

### Pointer Justification in SDH

Un aspetto fondamentale di SDH è la gestione della **sincronizzazione residua** con flussi tributari plesiocroni tramite i **pointer** (puntatori H1, H2, H3 nel frame STS/STM):

- Il pointer indica la posizione di inizio del **Synchronous Payload Envelope (SPE)** all'interno del frame
- Se il tributario è **più lento** del frame SDH -> **positive stuffing**: si inserisce un byte aggiuntivo neutro vicino all'H3 e il puntatore viene incrementato di 1
- Se il tributario è **più veloce** -> **negative stuffing**: il byte H3 viene usato per dati reali e il puntatore viene decrementato di 1
- Questo meccanismo elimina la necessità di buffer di compensazione usati nella PDH

### Overhead

L'overhead SDH si divide in:

- **RSOH (Regenerator Section Overhead)**: righe 1-3 del STM-1; framing (A1/A2), identificativo di sezione (J0), parità BIP-8 (B1), canale dati (D1-D3), orderwire (E1)
- **MSOH (Multiplex Section Overhead)**: righe 5-9; parità BIP-24 (B2), protezione APS (K1/K2), canale dati (D4-D12), sincronizzazione (S1), orderwire (E2)
- **POH (Path Overhead)**: colonna 1 del VC; trace (J1), parità (B3), indicatore di payload (C2), stato remoto (G1)

## Vantaggi SDH rispetto a PDH

| Aspetto            | PDH                             | SDH                               |
| ------------------ | ------------------------------- | --------------------------------- |
| Sincronizzazione   | Plesiocrona (clock locali)      | Globale (clock unico di rete)     |
| Accesso tributario | Demultiplex completo necessario | Estrazione diretta con pointer    |
| Gestione rete      | Proprietaria, limitata          | Standardizzata, overhead dedicato |
| Protezione guasti  | Manuale                         | Automatica (APS < 50 ms)          |
| Interoperabilità   | 3 standard incompatibili        | Standard mondiale unico           |
| Bit rate massimo   | ~140 Mbit/s (E4)                | Oltre 100 Gbit/s (STM-1024)       |
| Mezzo fisico       | Rame e fibra                    | Principalmente fibra ottica       |

---

## Mapping PDH -> SDH

Per trasportare flussi PDH (es. E1 a 2 Mbit/s) dentro SDH si usa una procedura di **containerizzazione**:

1. Il segnale E1 viene inserito nel **Container C-12**
2. Aggiungendo il path overhead si ottiene il **Virtual Container VC-12**
3. Aggiungendo un pointer si ottiene un **Tributary Unit TU-12**
4. Più TU-12 formano un **Tributary Unit Group TUG-2**, poi **TUG-3**
5. Il TUG-3 viene inserito nel **VC-4** che riempie il payload del STM-1

Un STM-1 può quindi trasportare **63 flussi E1** (2 Mbit/s), rispetto ai 64 della PDH E4, a causa dell'overhead aggiuntivo necessario per la gestione e monitoraggio.
