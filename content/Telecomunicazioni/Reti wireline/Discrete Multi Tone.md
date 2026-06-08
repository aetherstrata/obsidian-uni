---
tags:
  - modulazione
  - wireline
  - digitale
  - segnale
  - adsl
---
La **DMT** (*Discrete Multi-Tone Modulation*) è la tecnica di modulazione multiportante adottata dagli standard ITU-T per i sistemi *xDSL* (come **ADSL**, **ADSL2+**, **VDSL2**, **G.fast**). La modulazione DMT è l'equivalente dell'[[Orthogonal Frequency Division Multiplexing|OFDM]] applicata in **banda base**: a differenza dell'OFDM classico, che trasla il segnale su una portante ad alta frequenza (es. 2.4 GHz per il Wi-Fi), la DMT opera direttamente sulle frequenze del [[Doppino telefonico#Doppino telefonico|doppino telefonico]], senza modulazione in banda passante. Questo la rende adatta alla trasmissione su cavo in rame, dove non serve alcuna antenna e la trasmissione è guidata.

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

Il gap $\Gamma$ rappresenta la distanza dalla capacità di Shannon teorica in condizioni pratiche di codifica. Sottoportanti con [[Trasmissione Digitale#SNR|SNR]] elevato ricevono molti bit (es. 12-15 bit/simbolo), mentre quelle in zone rumorose o a frequenze elevate, dove l'attenuazione del  [[Doppino telefonico#Doppino telefonico|doppino]] è alta, ricevono pochi bit o vengono disabilitate. In pratica, il modem misura il SNR su ogni bin prima dell'inizio della trasmissione dati (fase di _initialization/training_) e aggiorna periodicamente la mappa di bit loading.

![[Pasted image 20260607191405.png]]

### Allocazione spettrale
In **ADSL** il [[Doppino telefonico#Doppino telefonico|doppino telefonico]] è condiviso tra tre servizi:

| Banda (kHz) | Servizio                                          |
| ----------- | ------------------------------------------------- |
| 0 - 4       | POTS (voce analogica, Plain Old Telephone System) |
| 25 - 138    | Upstream (upload dati)                            |
| 138 - 1104  | Downstream (download dati)                        |

- I primi 25 kHz sono riservati al **POTS** per garantire la retro compatibilità con la voce analogica, da cui deriva la necessità del [[filtro]] splitter sulle prese telefoniche.
- Il sistema è **asimmetrico** (da cui la *A* di ADSL): il downstream ha banda molto maggiore dell'upstream perché l'utente tipicamente scarica più dati di quanti ne carichi.
- La spaziatura fissa di $\Delta f=4,3125\ \text{kHz}$ implica che la banda totale di $1104\ \text{kHz}$ si suddivide in 256 sottoportanti downstream.
$$
T_s=\frac{1}{4312.5}\approx0.232\ \text{ms} + T_{CP} = 0.25\ \text{ms}\ \Longrightarrow\ 4\ \text{kbaud/s}
$$

>[!info] Allocazione in Bin
>| Bin | Uso                                          |
>| ----------- | ------------------------------------------------- |
>| 0        | DC |
>|1 - 5    | Guard band (fino a 25.875 kHz)  - usato per traffico POTS                    |
>| 6 - 31  | Upstream (25 - 138 kHz)                     |
>| 16  | Pilota Upstream (69 kHz) |
>| 32 - 255 | Downstream (138 - 1.104 kHz)  |
>| 32  | Guard band (138 kHz) |
>| 64 | Pilota Downstream (276 kHz) |

### Varianti degli Standard xDSL

| Standard              | Banda       | Subcarrier | DL max     | UL max     |
| --------------------- | ----------- | ---------- | ---------- | ---------- |
| **ADSL** G.992.1      | 1,1 MHz     | 256        | 8 Mbit/s   | 1 Mbit/s   |
| **ADSL2** G.992.3     | 1,1 MHz     | 256        | 12 Mbit/s  | 1 Mbit/s   |
| **ADSL2+** G.992.5    | 2,2 MHz     | 512        | 24 Mbit/s  | 3,5 Mbit/s |
| **VDSL2 17a** G.993.2 | 17,664 MHz  | 4096       | 150 Mbit/s | 50 Mbit/s  |
| **VDSL2 35b** G.993.2 | 35,328 MHz  | 8192       | 300 Mbit/s | 100 Mbit/s |
| **G.fast** G.9701     | 106–212 MHz | 2048–4096  | 1 Gbit/s   | 500 Mbit/s |
#### ADSL / ADSL2+
Utilizza **256 sottoportanti** e [[Quadrature Amplitude Modulation|QAM]] adattiva, usando la banda fino a **1,1 MHz**. L'ADSL2+ raddoppia le sottoportanti a 512 estendendo la banda a 2,2 MHz, raggiungendo **24 Mbit/s in downstream e 3,5 Mbit/s in upstream** entro 4 km.
#### VDSL2
Utilizza **4096 sottoportanti**, banda fino a **17,6 MHz** (profilo 17a) o **35 MHz** (profilo 35b). Raggiunge **100 Mbit/s in download e 50 Mbit/s in upload** entro 1 km. Richiede che la fibra ottica arrivi al cabinet (FTTC) e che il tratto in rame sia corto.
#### G.fast
Adotta **DFT-Spread OFDM** (una variante dell'[[Orthogonal Frequency Division Multiplexing|OFDM]] che riduce il PAPR, usata anche nel [[5G]]) con banda fino a **212 MHz** e vectoring obbligatorio. Raggiunge **1 Gbit/s in download e 500 Mbit/s in upload** entro **250 m**. A queste frequenze l'attenuazione del rame è così elevata che il tratto in rame deve essere minimo (tipicamente dall'armadio stradale direttamente all'edificio).

Con G.fast la spaziatura tra sottoportanti aumenta a **51,75 kHz**, contro i 4,3125 kHz di ADSL, e il bit loading massimo scende a **12 bit/sottoportante** (opzionale 14 bit) per gestire le maggiori perdite a frequenze elevate. Inoltre G.fast adotta il **TDD (Time Division Duplexing)** invece del FDD classico, e usa obbligatoriamente il **vectoring** per ridurre la diafonia (FEXT) tra più doppini nello stesso cavo.

>[!info] Vectoring
>Il **vectoring** (standardizzato in ITU-T G.993.5 per VDSL2 e obbligatorio in G.fast) è una tecnica di cancellazione della diafonia che opera in modo analogo alla cancellazione del rumore nelle cuffie: misura i segnali di crosstalk tra più doppini e li sottrae precisamente dal segnale ricevuto, permettendo di mantenere SNR elevato anche a frequenze alte e su distanze maggiori.

## Canale
Il canale DMT è il [[Doppino telefonico#Doppino telefonico|doppino telefonico]] *twisted pair*, caratterizzato da:

- **Attenuazione crescente con la frequenza**: più alta è la frequenza, più il segnale si attenua, da cui il profilo a campana del bit loading.
- **Rumore [[Rumore#AWGN|AWGN]]** e **diafonia** (crosstalk, FEXT/NEXT) dovuta all'accoppiamento tra doppini paralleli nello stesso cavo.
- **Risposta in frequenza non piatta**: giustifica la necessità del FEQ in ricezione.
- **Banda fisica**: circa 1,1 MHz per l'ADSL, estesa artificialmente fino a 212 MHz per G.fast, ma con forte attenuazione alle frequenze più alte, il che impone distanze brevi.

La distanza coperta è inversamente proporzionale alla banda utilizzabile:

- fino a 250m si usa G.fast a 1 Gbit/s,
- fino a 1 km si usa VDSL2 (~100 Mbit/s), 
- fino a 4 km si scende ad ADSL2 (~24 Mbit/s).