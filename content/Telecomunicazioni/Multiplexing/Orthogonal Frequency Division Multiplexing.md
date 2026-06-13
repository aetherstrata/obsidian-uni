---
tags:
  - multiplexing
  - segnale
  - digitale
  - physical
---

L'_Orthogonal Frequency Division Multiplexing_ (**OFDM**) ha rivoluzionato il panorama delle telecomunicazioni (è la base di [[Sistemi mobile#4G - LTE (Long Term Evolution)|4G]], [[5G]], Wi-Fi moderno e [[Discrete Multi Tone|ADSL]]) per rimediare all'inefficienza dello spettro del vecchio [[Frequency Division Multiplexing|FDM]].

## Funzionamento

### Ortogonalità

Anziché dividere un flusso dati molto veloce su una singola portante molto larga, che subirebbe fortissime distorsioni e riflessioni (_multipath fading_), l'OFDM lo divide su centinaia di piccole sottoportanti vicinissime tra loro a bassissima velocità.

![[Pasted image 20260606172701.png]]

La condizione matematica di ortogonalità impone che la spaziatura in frequenza tra due sottoportanti adiacenti sia esattamente l'inverso del tempo di simbolo utile $T_s$:

$$
\Delta f = \frac{1}{T_s}
$$

Questo garantisce che, integrando il prodotto di due sottoportanti su $T$​, il risultato sia sempre zero:

$$
\int_{0}^{T_s} e^{j2\pi f_i t}\cdot e^{-j2\pi f_k t}\,dt = \int_{0}^{T_s} e^{j2\pi (f_i-f_k) t}\,dt =0\quad \forall\ i\ne j
$$

### Sovrapposizione

A differenza dell'[[Frequency Division Multiplexing|FDM]] tradizionale che deve tenere separati i canali usando delle guard band, l'OFDM permette che gli spettri delle singole sottoportanti **si sovrappongano**. Questo è possibile perché sono matematicamente ortogonali tra loro: i loro picchi coincidono con lo zero esatto delle altre. Il guadagno in efficienza spettrale rispetto a FDM è circa il **50%**.

### Prefisso Ciclico (CP)

Per evitare l'**ISI** (_Inter-Symbol Interference_) causata dagli echi multipath, tra un simbolo e l'altro viene inserito un _guard interval_ copiando in esso l'ultima parte del simbolo stesso. Questa copia è chiamata _Cyclic Prefix_ (**CP**).

![[1780846886-1.png]]

Il CP deve essere più lungo del massimo ritardo di propagazione del canale ($\tau_{max}$): finché $T_{CP}\ge\tau_{max}$, tutti gli echi del simbolo precedente si esauriscono prima dell'inizio del simbolo corrente, eliminando l'ISI.

![[Pasted image 20260607191815.png]]

L'effetto collaterale è che la convoluzione lineare del canale diventa una [[Convoluzione]] **circolare**, che è diagonalizzabile con la [[Trasformata Discreta di Fourier|DFT]]: ogni sottoportante sperimenta semplicemente una moltiplicazione scalare $H[k]$ (attenuazione e sfasamento), senza interferenza tra sottoportanti.

Il costo del **CP** è una riduzione dell'efficienza: se $T_{CP} = 5$ μs e $T_s=66.67$ μs, l'overhead è circa il 7%.

### Resistenza al Multipath Fading selettivo

Il multipath fading _selettivo in frequenza_ attenua solo alcune porzioni dello spettro. In un sistema a portante singola, un'attenuazione su tutta la banda è catastrofica. In OFDM, l'attenuazione colpisce solo le sottoportanti in quella zona: i bit persi su quelle sottoportanti vengono recuperati tramite _codici di correzione d'errore_ ([[Forward Error Correction|FEC]]) e **interleaving** in frequenza, che distribuisce i bit dello stesso pacchetto su sottoportanti distanti tra loro.

## Implementazione

I circuiti analogici necessari per filtrare centinaia di frequenze ravvicinate sarebbero impossibili da costruire, quindi le operazioni richieste dall'OFDM vengono eseguite da microchip DSP attraverso la trasformata di Fourier Veloce Inversa (_IFFT_) in trasmissione e la _FFT_ in ricezione.

La _modulazione_ è digitale ([[Phase Shift Keying|PSK]] o [[Quadrature Amplitude Modulation|QAM]]) ed è usata per mappare i simboli alle $N$ sottoportanti. La complessità temporale dell'_IFFT_ è $O(N\log N)$, rendendola praticabile anche su centinaia di sottoportanti.

> [!info] Schema di un trasmettitore OFDM
> ![[Pasted image 20260606173352.png]]

> [!info] Schema di un ricevitore OFDM
> ![[Pasted image 20260606173450.png]]

> [!tip] Vantaggi e svantaggi  
> **Vantaggi**: alta efficienza spettrale, robustezza al multipath, equalizzazione semplice in frequenza, implementazione con IFFT/FFT efficiente.
>
> **Svantaggi**: elevato **PAPR (Peak-to-Average Power Ratio)** - la somma di molte sottoportanti può creare picchi molto alti, richiedendo amplificatori lineari costosi e riducendo l'efficienza energetica. Sensibile all'offset di frequenza tra trasmettitore e ricevitore (CFO), che rompe l'ortogonalità.

---

## OFDMA - Orthogonal Frequency Division Multiple Access

L'**OFDMA** estende l'OFDM da tecnica di multiplexing a tecnica di **accesso multiplo**: invece di assegnare tutte le sottoportanti a un singolo utente, il piano tempo-frequenza viene suddiviso in blocchi di risorse assegnati dinamicamente a utenti diversi.

### Resource Block (RB)

L'unità minima di allocazione è il **Resource Block**, che occupa una porzione sia nel dominio del tempo che della frequenza:

- **Asse frequenza**: 12 sottoportanti × 15 kHz = **180 kHz**
- **Asse tempo**: 1 slot = **0.5 ms** = 7 simboli OFDM (Normal CP)

Ogni Resource Block contiene quindi $12 \times 7 = 84$ _Resource Element_ (**RE**), dove ciascun RE trasporta un simbolo [[Quadrature Amplitude Modulation|QAM]].

| Banda totale | N. Resource Block | N. sottoportanti | Dim. IFFT |
| ------------ | ----------------- | ---------------- | --------- |
| 1.4 MHz      | 6                 | 72               | 128       |
| 5 MHz        | 25                | 300              | 512       |
| 10 MHz       | 50                | 600              | 1024      |
| 20 MHz       | 100               | 1200             | 2048      |

### Scheduling Dinamico

Lo scheduler dell'eNodeB ([[Sistemi mobile#4G - LTE (Long Term Evolution)|LTE]]) o gNodeB ([[5G]]) decide **ogni millisecondo** a quale UE assegnare ciascun RB. Questa decisione si basa su:

- **CQI** (_Channel Quality Indicator_): l'UE misura la qualità del canale su ciascun gruppo di sottoportanti e lo segnala alla base station
- **Priorità QoS**: bearer GBR (voce, video) hanno precedenza sui bearer non-GBR (dati best-effort)
- **Fairness**: algoritmi come il **Proportional Fair** bilanciano il throughput massimo con l'equità tra utenti

Il risultato è la **Link Adaptation**: se l'UE riceve male su alcune frequenze (es. a causa di multipath selettivo), la base station evita semplicemente di assegnargli RB su quelle frequenze, usando porzioni di spettro più favorevoli.

> [!tip] Confronto con OFDM
> | Aspetto | OFDM | OFDMA |
> | -------------------------- | -------------------------------- | ------------------------------------------------- |
> | Utenti serviti | Uno alla volta | Più utenti simultaneamente |
> | Assegnazione sottoportanti | Tutte a un utente per slot | Sottoinsieme di RB per utente |
> | Scheduling | Temporale (chi trasmette quando) | Spazio-temporale (chi, quando, dove in frequenza) |
> | Efficienza risorse | Minore | Maggiore |
> | Flessibilità | Bassa | Alta (adatta a traffico asimmetrico) |
> | Utilizzo | Wi-Fi (accesso singolo), ADSL | LTE DL, 5G NR, Wi-Fi 6/6E/7 |

### SC-FDMA

L'OFDMA ha un problema critico nell'uplink: l'elevato **PAPR** scarica rapidamente la batteria del dispositivo mobile. La soluzione adottata in [[Sistemi mobile#4G - LTE (Long Term Evolution)|LTE]] per l'uplink è l'**SC-FDMA (Single Carrier FDMA)**, che aggiunge un blocco **DFT pre-coding** prima del normale flusso OFDMA:

$$
\underbrace{\text{Bit} \to \text{QAM} \to \overbrace{\text{DFT}}^{\text{blocco SC}} \to \text{Subcarrier Mapping} \to \text{IDFT} \to \text{CP}}_{\text{SC-FDMA}}
$$

La [[Trasformata Discreta di Fourier|DFT]] preliminare distribuisce l'energia di ciascun simbolo su tutte le sottoportanti assegnate, trasformando di fatto il segnale in uno a singola portante. Il risultato è un PAPR significativamente più basso rispetto a OFDMA puro, a parità di efficienza spettrale.

> [!tip] Regola pratica
>
> - In **Downlink** (base station → UE) si usa **OFDMA**: la base station ha alimentazione di rete, il PAPR non è un problema
> - In **Uplink** (UE → base station) si usa **SC-FDMA**: il dispositivo mobile deve risparmiare batteria
