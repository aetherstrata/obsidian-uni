---
tags:
  - modulazione
  - wireline
  - digitale
  - segnale
  - adsl
---
La **DMT** (*Discrete Multi-Tone Modulation*) è la tecnica di modulazione multiportante adottata dagli standard ITU-T per i sistemi *xDSL* (come **ADSL**, **ADSL2+**, **VDSL2**, **G.fast**). La modulazione DMT è l'equivalente dell'[[Orthogonal Frequency Division Multiplexing|OFDM]] applicata in **banda base**: a differenza dell'OFDM classico, che trasla il segnale su una portante ad alta frequenza (es. 2.4 GHz per il Wi-Fi), la DMT opera direttamente sulle frequenze del [[Cavi elettrici#Doppino telefonico|doppino telefonico]], senza modulazione in banda passante. Questo la rende adatta alla trasmissione su cavo in rame, dove non serve alcuna antenna e la trasmissione è guidata.

|Caratteristica|OFDM|DMT|
|---|---|---|
|Dominio di trasmissione|Banda passante|Banda base|
|Applicazioni|Wi-Fi, 4G/5G, fibra ottica|ADSL, VDSL2, G.fast|
|Modulazione portante|Sì (es. 2,4 GHz)|No|
|Bit Loading adattivo|Sì (AMC)|Sì (standard nel DSL)|
|Trasformata usata|IFFT/FFT|IFFT/FFT|
|Prefisso ciclico|Sì|Sì|
## Funzionamento
Il flusso di bit in ingresso viene diviso in $N$ canali paralleli. Ogni canale (sottoportante) occupa una larghezza di banda:
$$
\Delta f=\frac{1}{T}=4.3125\ \text{kHz}
$$
dove $T$ è la durata del simbolo DMT. Questa spaziatura garantisce l'**ortogonalità** tra le sottoportanti, eliminando l'interferenza intercanale (ICI).

Ogni sottoportante usa indipendentemente la modulazione [[Quadrature Amplitude Modulation|QAM]], con un numero di bit per simbolo adattabile in base alle condizioni del canale.
## Schema del sistema
### Trasmettitore
$$
\boxed{\vphantom{\int}\ \text{S/P}\ } \longrightarrow \boxed{\vphantom{\int}\ \text{Mappa QAM}\ } \longrightarrow \boxed{\vphantom{\int}\ \text{IFFT}\ } \longrightarrow \boxed{\vphantom{\int}\ \text{+ CP}\ } \longrightarrow \boxed{\vphantom{\int}\ \text{DAC}\ } \longrightarrow \boxed{\vphantom{\int}\ \text{TX}\ } 
$$
### Ricevitore
$$
\boxed{\vphantom{\int}\ \text{RX}\ } \longrightarrow \boxed{\vphantom{\int}\ \text{- CP}\ } \longrightarrow \boxed{\vphantom{\int}\ \text{FFT}\ } \longrightarrow \boxed{\vphantom{\int}\ \text{FEQ}\ } \longrightarrow \boxed{\vphantom{\int}\ \text{Mappa QAM}\ } \longrightarrow \boxed{\vphantom{\int}\ \text{P/S}\ } 
$$

Il  blocco **S/P** (*Bit Loading*) assegna un numero variabile di bit a ogni sottoportante in base all'[[Trasmissione Digitale#SNR|SNR]] locale.

La **Mappa QAM** converte i bit in simboli complessi della costellazione per ogni sottoportante, e viceversa durante la ricezione.

I blocchi **+ CP** e **- CP** si occupano di inserire e rimuovere la copia della coda del simbolo all'inizio, eliminando l'interferenza intersimbolica e permettendo l'uso diretto della *FFT* in ricezione.

Il **FEQ** (*Frequency Domain Equalizer*) attua un'equalizzazione nel dominio della frequenza: opera su ogni sottoportante indipendentemente, compensando la risposta in frequenza non piatta del canale.

## Bit Loading
Il **bit loading** è il meccanismo che rende la DMT adattiva alle condizioni del canale. Il numero di bit assegnato alla k-esima sottoportante è dato dalla formula di Shannon modificata:
$$
b_k=\log_{⁡2}\left(1+\frac{\text{SNR}_k}{\Gamma}\right)
$$
dove:

- $\text{SNR}_k$​ = rapporto segnale-rumore misurato sulla k-esima sottoportante
- $\Gamma$ = **gap di codifica** (coding gap), pari a circa **9 dB** per un BER target di $10^{-7}$

Il gap $\Gamma$ rappresenta la distanza dalla capacità di Shannon teorica in condizioni pratiche di codifica. Sottoportanti con [[Trasmissione Digitale#SNR|SNR]] elevato ricevono molti bit (es. 12-15 bit/simbolo), mentre quelle in zone rumorose o a frequenze elevate, dove l'attenuazione del  [[Cavi elettrici#Doppino telefonico|doppino]] è alta, ricevono pochi bit o vengono disabilitate. In pratica, il modem misura il SNR su ogni bin prima dell'inizio della trasmissione dati (fase di _initialization/training_) e aggiorna periodicamente la mappa di bit loading.

![[Pasted image 20260607191405.png]]

### Allocazione spettrale
In **ADSL** il [[Cavi elettrici#Doppino telefonico|doppino telefonico]] è condiviso tra tre servizi:

| Banda (kHz) | Servizio                                          |
| ----------- | ------------------------------------------------- |
| 0 - 4       | POTS (voce analogica, Plain Old Telephone System) |
| 25 - 138    | Upstream (upload dati)                            |
| 138 - 1104  | Downstream (download dati)                        |

- I primi 25 kHz sono riservati al **POTS** per garantire la retrocompatibilità con la voce analogica, da cui deriva la necessità del [[filtro]] splitter sulle prese telefoniche.
- Il sistema è **asimmetrico** (da cui la *A* di ADSL): il downstream ha banda molto maggiore dell'upstream perché l'utente tipicamente scarica più dati di quanti ne carichi.
- La spaziatura fissa di $\Delta f=4,3125\ \text{kHz}$ implica che la banda totale di $1104\ \text{kHz}$ si suddivide in 256 sottoportanti downstream.

>[!info] Allocazione in Bin
>| Bin | Servizio                                          |
>| ----------- | ------------------------------------------------- |
>| 0        | DC |
>|1 - 5    | Upstream (upload dati)                            |
>| 138 - 1104  | Downstream (download dati)                        |

### Varianti degli Standard xDSL

| Standard             | Sottoportanti N | Banda B  | Downstream max | Note                        |
| -------------------- | --------------- | -------- | -------------- | --------------------------- |
| **ADSL** (G.992.1)   | 256             | 1,1 MHz  | ~8 Mbit/s      | 32 upstream, 224 downstream |
| **ADSL2+** (G.992.5) | 512             | 2,2 MHz  | ~24 Mbit/s     | Doppia banda vs ADSL        |
| **VDSL2 17a**        | 4096            | 17,6 MHz | ~100 Mbit/s    | FDD, vectoring opzionale    |
| **VDSL2 35b**        | ~8000           | 35 MHz   | ~400 Mbit/s    | Distanza <1 km              |
| **G.fast 106**       | 2048            | 106 MHz  | ~1 Gbit/s      | TDD, distanza <250 m        |
| **G.fast 212**       | 4096            | 212 MHz  | ~2 Gbit/s      | Distanza ancora minore      |

Con G.fast la spaziatura tra sottoportanti aumenta a **51,75 kHz**, contro i 4,3125 kHz di ADSL, e il bit loading massimo scende a **12 bit/sottoportante** (opzionale 14 bit) per gestire le maggiori perdite a frequenze elevate. Inoltre G.fast adotta il **TDD (Time Division Duplexing)** invece del FDD classico, e usa obbligatoriamente il **vectoring** per ridurre la diafonia (FEXT) tra più doppini nello stesso cavo.

>[!info] Vectoring
>Il **vectoring** (standardizzato in ITU-T G.993.5 per VDSL2 e obbligatorio in G.fast) è una tecnica di cancellazione della diafonia che opera in modo analogo alla cancellazione del rumore nelle cuffie: misura i segnali di crosstalk tra più doppini e li sottrae precisamente dal segnale ricevuto, permettendo di mantenere SNR elevato anche a frequenze alte e su distanze maggiori.

## Canale
Il canale DMT è il [[Cavi elettrici#Doppino telefonico|doppino telefonico]] *twisted pair*, caratterizzato da:

- **Attenuazione crescente con la frequenza**: più alta è la frequenza, più il segnale si attenua, da cui il profilo a campana del bit loading.
- **Rumore [[Rumore#AWGN|AWGN]]** e **diafonia** (crosstalk, FEXT/NEXT) dovuta all'accoppiamento tra doppini paralleli nello stesso cavo.
- **Risposta in frequenza non piatta**: giustifica la necessità del FEQ in ricezione.
- **Banda fisica**: circa 1,1 MHz per l'ADSL, estesa artificialmente fino a 212 MHz per G.fast, ma con forte attenuazione alle frequenze più alte, il che impone distanze brevi.

La distanza coperta è inversamente proporzionale alla banda utilizzabile:

- fino a 250m si usa G.fast a 1 Gbit/s,
- fino a 1 km si usa VDSL2 (~100 Mbit/s), 
- fino a 4 km si scende ad ADSL2 (~24 Mbit/s).