---
tags:
  - multiplexing
  - segnale
  - physical
  - digitale
aliases:
  - FDM
---

Nel _Frequency Division Multiplexing_ (**FDM**) tutti gli utenti trasmettono nello stesso momento, ma il canale fisico viene suddiviso in più sottobande di frequenza. Ogni utente occupa la propria sottobanda in modo esclusivo e continuo: è il principio alla base della TV analogica e della radio [[Frequency Modulation|FM]], dove ogni stazione trasmette sempre, ma su una frequenza diversa.

## Funzionamento

### Suddivisione dello Spettro

La banda totale disponibile $B$ viene divisa in $N$ canali, ciascuno di larghezza $B_c$: $$ B*c = \frac{B}{N} $$ Ogni sorgente informativa (utente, stazione) viene traslata in frequenza tramite un **modulatore** che sposta il segnale in banda base sulla propria sottobanda assegnata. Il segnale composito trasmesso è la somma di tutti i canali modulati: $$ s(t) = \sum*{i=1}^{N} m_i(t) \cdot \cos(2\pi f_i t) $$ dove $m_i(t)$ è il segnale in banda base dell'utente $i$ e $f_i$ è la frequenza portante del canale $i$. In ricezione, un banco di **filtri passa-banda** seleziona ciascuna sottobanda separatamente, dopodiché il segnale viene demodulato per recuperare il segnale originale in banda base.

### Guard Band

I filtri elettronici reali non tagliano le frequenze in modo netto: la risposta in frequenza decade gradualmente nella cosiddetta **banda di transizione**.

Per evitare che il segnale di un canale trabocchi nel canale adiacente (_Inter-Channel Interference_, **ICI**), tra una sottobanda e l'altra vengono inserite delle **bande di guardia** (_guard band_) vuote.

Questo ha un costo diretto in efficienza spettrale: se ogni canale ha larghezza $B_c$ e la guard band ha larghezza $B_g$, la banda effettivamente occupata per $N$ canali è: $$ B\_{tot} = N \cdot B_c + (N-1) \cdot B_g $$ La frazione di spettro sprecata in guard band può arrivare al **25-30%** della banda totale nei sistemi analogici.

> [!example] Esempio - Radio FM
> In Europa, le stazioni radio FM occupano la banda 87.5-108 MHz. Ogni stazione ha un canale da 200 kHz, di cui ~150 kHz sono effettivamente utili e i restanti 50 kHz fungono da guard band rispetto alle stazioni adiacenti.

## Implementazione

Nei sistemi **analogici** (radio FM, TV analogica, telefonia analogica FDM su doppino), la modulazione e il filtraggio avvengono direttamente nel dominio RF tramite circuiti LC, oscillatori e miscelatori. Nei sistemi **digitali** più datati (es. modem xDSL a portante multipla pre-OFDM), le operazioni possono essere realizzate nel dominio digitale tramite banchi di filtri, ma la condizione di ortogonalità non è garantita: le sottoportanti devono rimanere sufficientemente separate da evitare sovrapposizione degli spettri.

> [!tip] Vantaggi e svantaggi
> **Vantaggi**: semplicità concettuale e implementativa, nessuna sincronizzazione temporale richiesta tra gli utenti, robustezza in scenari broadcast (punto-multipunto).
> **Svantaggi**: le guard band riducono l'efficienza spettrale; la larghezza di banda fissa per canale è rigida e non si adatta al traffico variabile; i filtri analogici precisi sono costosi. L'[[Orthogonal Frequency Division Multiplexing|OFDM]] ha superato questi limiti introducendo l'ortogonalità matematica tra le sottoportanti.

---

## FDMA - Frequency Division Multiple Access

L'**FDMA** (_Frequency Division Multiple Access_) è l'applicazione del principio FDM come tecnica di **accesso multiplo** nelle reti di comunicazione: mentre l'FDM descrive come dividere fisicamente uno spettro in sottobande, l'FDMA definisce come tale divisione viene usata per permettere a più utenti distinti di condividere lo stesso mezzo trasmissivo.

La relazione tra FDM e FDMA è analoga a quella tra [[Code Division Multiplexing|CDM]] e [[Code Division Multiplexing#Code Division Multiple Access (CDMA)|CDMA]], o tra [[Orthogonal Frequency Division Multiplexing|OFDM]] e [[Orthogonal Frequency Division Multiplexing#OFDMA - Orthogonal Frequency Division Multiple Access|OFDMA]]: il primo è il meccanismo fisico, il secondo è il protocollo di accesso che lo sfrutta.

### Assegnazione dei Canali

In FDMA ogni utente riceve un canale di frequenza dedicato per tutta la durata della connessione (_channel assignment_). Esistono due modalità principali:

- **FDMA/SCPC** (_Single Channel Per Carrier_): un singolo canale vocale o dati per portante; usato nella telefonia satellitare e nei link punto-punto
- **FDMA a canali preassegnati** (_Fixed Assignment_): le frequenze sono suddivise a priori tra gli utenti; tipico dei sistemi radio analogici di prima generazione (**1G**, es. TACS, AMPS)
- **FDMA a canali su richiesta** (_Demand Assigned FDMA_, **DAMA**): i canali vengono assegnati dinamicamente a un utente solo quando ha traffico da trasmettere; migliora l'efficienza in scenari con traffico intermittente (es. reti satellitari VSAT)

### Struttura del Canale

In un sistema FDMA puro, la banda totale $B$ è divisa in $N$ canali simmetrici. Nei sistemi mobili di prima generazione si adottava la separazione **uplink/downlink** tramite **FDD** (_Frequency Division Duplex_): due bande separate, una per la trasmissione e una per la ricezione:

| Parametro        | AMPS (1G USA) | TACS (1G Europa) |
| ---------------- | ------------- | ---------------- |
| Banda uplink     | 824-849 MHz   | 890-915 MHz      |
| Banda downlink   | 869-894 MHz   | 935-960 MHz      |
| Larghezza canale | 30 kHz        | 25 kHz           |
| N. canali        | 832           | 1000             |

### Problemi e Limitazioni

L'FDMA soffre degli stessi limiti strutturali dell'FDM, amplificati dallo scenario multi-utente:

- **Rigidità dell'allocazione**: un utente che non trasmette occupa comunque la propria frequenza, sprecando risorse spettrali
- **Effetto del prodotto di intermodulazione** (_intermodulation_): in presenza di molti segnali su portanti vicine, le non linearità degli amplificatori generano frequenze spurie che interferiscono con i canali adiacenti; richiede amplificatori ad alta linearità e back-off di potenza
- **Scalabilità limitata**: il numero massimo di utenti è deterministico e rigido, fissato dal numero di canali disponibili; non c'è il degradamento graduale tipico del [[Code Division Multiplexing#Code Division Multiple Access (CDMA)|CDMA]]
- **Nessun guadagno dalla diversità**: ogni utente trasmette sempre sulla stessa frequenza, senza poter sfruttare porzioni di spettro momentaneamente meno disturbate

> [!tip] Confronto con le altre tecniche di accesso multiplo
> | Aspetto | FDMA | TDMA | CDMA |
> | --- | --- | --- | --- |
> | Dominio di separazione | Frequenza | Tempo | Codice |
> | Sinc. richiesta | No | Sì (slot temporali) | Sì (chip sync) |
> | Flessibilità | Bassa | Media | Alta (soft capacity) |
> | Efficienza spettrale | Bassa (guard band) | Media | Alta |
> | Utilizzo storico | 1G (AMPS, TACS) | 2G (GSM) | 3G (UMTS/WCDMA) |
> | Handover | Hard | Hard | Soft (MRC) |

### Utilizzo Moderno

L'FDMA puro è stato largamente soppiantato nelle reti cellulari da [[Code Division Multiplexing#Code Division Multiple Access (CDMA)|CDMA]] (3G) e [[Orthogonal Frequency Division Multiplexing#OFDMA - Orthogonal Frequency Division Multiple Access|OFDMA]] (4G/5G). Tuttavia rimane la tecnica dominante in alcuni contesti specifici:

- **Reti satellitari** (es. VSAT, Iridium): i link punto-punto su transponder beneficiano della semplicità FDMA/DAMA
- **Sistemi PMR** (_Professional Mobile Radio_, es. TETRA, P25): reti radio professionali per polizia, vigili del fuoco
- **Sottosistema di accesso in xDSL**: la separazione POTS/ADSL sul doppino telefonico è di fatto FDMA tra voce (0-4 kHz) e dati (25 kHz-1.1 MHz)
- **Upstream DOCSIS** (reti via cavo): il canale upstream è suddiviso in mini-slot frequenziali assegnati ai modem via FDMA/TDMA ibrido
