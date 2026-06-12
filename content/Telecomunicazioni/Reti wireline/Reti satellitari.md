---
tags:
  - rf
  - wireless
  - reti
---

Le **reti satellitari** utilizzano satelliti artificiali in orbita per trasmettere dati, voce e video tra stazioni terrestri, coprendo aree geografiche anche molto remote dove nessuna infrastruttura terrestre è economicamente praticabile. Il segnale RF viene trasmesso da una stazione di terra (**uplink**), ricevuto dal satellite e ritrasmesso verso una o più stazioni di terra (**downlink**). Esistono due tipologie di satellite:

- **Satellite trasparente (bent-pipe)**: funge da semplice ripetitore; riceve il segnale uplink, lo amplifica, lo trasla in frequenza e lo ritrasmette in downlink senza elaborazione digitale
- **Satellite rigenerativo**: elabora digitalmente il segnale a bordo (demodulazione, decoding, re-encoding), agendo come vero nodo di rete; riduce la propagazione del rumore e consente routing inter-satellitare

## Architettura del Sistema

Un sistema satellitare è composto da tre segmenti distinti:

**Segmento Spaziale** (il satellite):

- Payload: transponder, antenne di TX/RX
- Bus: pannelli solari, batterie, computer di bordo, sistemi di propulsione e controllo d'assetto

**Segmento di Terra**:

- Stazione di controllo (TT&C - Telemetry, Tracking & Command)
- Hub gateway: collega la rete satellitare alla rete terrestre (fibra)
- Stazioni VSAT (Very Small Aperture Terminal): terminali compatti per utenti aziendali
- NOC (Network Operation Center): monitoraggio e gestione dell'intera rete

**Segmento Utente**:

- Terminali con antenna dish (parabola), modem satellitare, router/CPE
- Dispositivi mobili, sensori IoT, terminali marittimi/aeronautici

## Tipi di Orbita

Le orbite satellitari si classificano in tre fasce principali, con caratteristiche ingegneristiche e applicative molto diverse:

| Parametro                           | LEO                             | MEO             | GEO                             |
| ----------------------------------- | ------------------------------- | --------------- | ------------------------------- |
| **Altitudine**                      | 200-2.000 km                    | 2.000-35.786 km | 35.786 km                       |
| **Periodo orbitale**                | ~90-120 min                     | 2-12 ore        | 24 ore (sincrono)               |
| **Latenza (one-way)**               | 20-40 ms                        | 67-125 ms       | ~240 ms (round-trip ~480 ms)    |
| **Copertura per satellite**         | Ridotta, richiede costellazioni | Intermedia      | ~1/3 della superficie terrestre |
| **Path loss**                       | Basso                           | Medio           | Molto elevato                   |
| **Satelliti per copertura globale** | Centinaia-migliaia              | 12-24           | 3-4                             |
| **Applicazioni tipiche**            | Broadband (Starlink), IoT       | GNSS, O3b/SES   | TV, meteo, VSAT aziendale       |

### LEO - Low Earth Orbit (200-2.000 km)

I satelliti LEO completano un'orbita in circa 90 minuti, quindi **non sono geostazionari**: si muovono rispetto all'osservatore terrestre, richiedendo costellazioni di centinaia o migliaia di satelliti per garantire copertura continua. La bassa altitudine si traduce in **latenza paragonabile alle reti terrestri** (~20-40 ms), rendendoli idonei per applicazioni real-time. Lo svantaggio principale è il costo di lancio e gestione di costellazioni numerose.

### MEO - Medium Earth Orbit (2.000-35.786 km)

I satelliti MEO offrono un compromesso tra copertura e latenza (~67-150 ms), con periodi orbitali di 2-12 ore. Sono l'orbita elettiva per i sistemi **GNSS**: GPS (USA, 20.200 km), Galileo (EU), GLONASS (RU), BeiDou (CN). Il sistema O3b di SES usa MEO a ~8.000 km per broadband enterprise a bassa latenza.

### GEO - Geostationary Earth Orbit (35.786 km)

A 35.786 km il satellite ha un periodo orbitale esattamente pari alla rotazione terrestre (24 ore), risultando **fisso rispetto a un osservatore a terra**. Tre soli satelliti GEO coprono quasi tutta la superficie terrestre. La latenza round-trip di ~480-600 ms li esclude da applicazioni real-time (VoIP, gaming), ma restano ideali per broadcasting TV, meteo (Meteosat) e VSAT aziendali.

## Bande di Frequenza

Al crescere della frequenza aumentano throughput e larghezza di banda disponibile, ma aumentano anche l'attenuazione atmosferica e la sensibilità alla pioggia (_rain fade_):

| Banda       | Frequenza | Uso principale                         | Note                                        |
| ----------- | --------- | -------------------------------------- | ------------------------------------------- |
| **L**       | 1-2 GHz   | GPS/GNSS, Iridium, mobile              | Massima resistenza atmosferica              |
| **S**       | 2-4 GHz   | TT&C, radar meteo, mobile              | Stabile in condizioni avverse               |
| **C**       | 4-8 GHz   | TV broadcast, lunga distanza           | Molto resistente a pioggia e neve           |
| **X**       | 8-12 GHz  | Radar, militare, missioni scientifiche | Uso quasi esclusivo difesa/scienza          |
| **Ku**      | 12-18 GHz | DTH TV, VSAT, broadband                | Alta larghezza di banda, moderata rain fade |
| **Ka**      | 26-40 GHz | Broadband ad alta capacità, 5G         | Alta capacità, sensibile a rain fade        |
| **V/Q**     | 40-75 GHz | Next-gen LEO                           | In fase di deployment                       |
| **Banda E** | 71-86 GHz | ISL Starlink Gen2                      | Quadruplica la capacità per satellite       |

Il trade-off fondamentale delle bande satellitari è sintetizzabile come: **più alta la frequenza -> più banda disponibile, antenna più piccola, ma più attenuazione**.

### Starlink

Starlink (SpaceX) è attualmente la più grande mega-costellazione LEO operativa con oltre 7.000 satelliti in orbita (su 42.000 pianificati), disposti tra 340 e 570 km di altitudine. Il sistema di frequenze è strutturato su più livelli:

- **Terminale utente -> satellite (uplink)**: banda Ku 14.0-14.5 GHz
- **Satellite -> terminale utente (downlink)**: bande X/Ku 10.7-12.7 GHz
- **Gateway -> satellite (uplink backhaul)**: banda Ka 27.5-30.0 GHz
- **Satellite -> gateway (downlink backhaul)**: banda Ka 17.8-18.6 GHz
- **Inter-Satellite Links (ISL)**: laser a infrarosso vicino (~1550 nm)

Gli **ISL ottici** sono una caratteristica distintiva di Starlink: trasmettono dati direttamente tra satelliti senza passare per stazioni a terra, riducendo la latenza e garantendo connettività in mezzo agli oceani dove non esistono gateway. La velocità dei link laser (~100 Gbps) supera di gran lunga quella dei link RF inter-satellite.

> [!info] Mega-Costellazioni LEO a Confronto
> | Sistema | Operatore | Satelliti pianificati | Orbita | Bit rate | Latenza |
> | ---------------------- | ---------------- | --------------------- | -------------- | -------------- | -------- |
> | **Starlink** | SpaceX (USA) | 42.000 (7k in orbita) | 340-570 km | 50-220 Mb/s | 20-40 ms |
> | **OneWeb** | Eutelsat/UK Gov. | 648 in orbita | 1.200 km | 50-200 Mb/s | 40-70 ms |
> | **Amazon Kuiper** | Amazon (USA) | 3.236 (150 in orbita) | 590-630 km | 100-1.000 Mb/s | 25-50 ms |
> | **Telesat Lightspeed** | Telesat (CA) | 198 pianificati | 1.000-1.350 km | 100+ Mb/s | 30-50 ms |

## Tecniche Trasmissive e Standard

### Modulazioni e FEC

I sistemi satellitari usano modulazioni robuste adatte al canale con elevato path loss:

- **QPSK**: robusta, efficiente in presenza di rumore elevato, usata in condizioni sfavorevoli
- **8PSK**: maggiore efficienza spettrale rispetto a QPSK
- **APSK (Amplitude and Phase Shift Keying)**: ottimizzata per transponder con amplificatori non lineari (HPA), standard in DVB-S2X

Per la **correzione degli errori (FEC)** si usano:

- **Turbo codes**: prestazioni near-Shannon, usati in sistemi militari e 3GPP
- **LDPC (Low-Density Parity-Check)**: standard in DVB-S2, prestazioni eccellenti a bassa complessità
- **Reed-Solomon**: codici a blocco classici, spesso concatenati con LDPC

### Standard DVB

- **DVB-S2 (Digital Video Broadcasting - Satellite 2nd gen)**: standard downlink per broadband e TV satellitare, definisce QPSK/8PSK/APSK + LDPC+BCH FEC + ACM (Adaptive Coding and Modulation)
- **DVB-S2X**: estensione di DVB-S2 con modulazioni fino a 32APSK e 64APSK, throughput fino al 51% superiore a DVB-S2
- **DVB-RCS2 (Return Channel Satellite 2nd gen)**: standard per il canale di ritorno (uplink utente), supporta OFDM e ACM, ottimizzato per banda Ka

### Tecniche di Accesso Multiplo

Più utenti condividono la stessa risorsa satellitare tramite:

- **FDMA**: ogni utente usa una sottobanda di frequenza fissa
- **TDMA**: ogni utente trasmette in slot temporali assegnati
- **CDMA**: spreading con codici ortogonali
- **OFDM/OFDMA**: sottoportanti ortogonali, usato nei sistemi di nuova generazione

## Link Budget Satellitare

Il link budget per un sistema satellitare si calcola con l'equazione di Friis estesa, ma le distanze in gioco (35.786 km per GEO) generano **path loss enormi**:

$$
\text{FSPL (dB)}=20\log10​(f)+20\log10​(d)-147.55
$$

Per un GEO a 35.786 km nella banda Ku (12 GHz): FSPL $\approx$ **205 dB**. Per compensare queste perdite si usano:

- **Antenne paraboliche ad alto guadagno** a terra (tipicamente 30-50 dBi per VSAT)
- **HPA (High Power Amplifier)** a bordo del satellite (10 W - 100 W per transponder)
- **FEC aggressivo** (turbo codes, LDPC)
- **ACM (Adaptive Coding and Modulation)**: adatta in tempo reale la modulazione e il FEC alle condizioni del canale (rain fade)

## Applicazioni

- **Televisione (DTH - Direct-to-Home)**: broadcasting diretto via GEO a milioni di utenti simultanei (Astra, Eutelsat, SES)
- **Internet broadband**: accesso in aree remote prive di fibra (Starlink, Viasat, HughesNet)
- **GNSS - Navigazione**: GPS (USA), Galileo (EU), GLONASS (RU), BeiDou (CN) su orbita MEO
- **Comunicazioni marittime e aeronautiche**: VSAT su navi e aerei; connettività d'emergenza in mare
- **MILSATCOM**: comunicazioni militari sicure, satelliti X-band dedicati (NATO, USA)
- **Osservazione della Terra**: meteo (Meteosat, GOES), monitoraggio ambientale, risposta a catastrofi
- **Backhaul 5G**: collegamento di stazioni base in aree prive di fibra; integrazione con NTN

## NTN - Non-Terrestrial Networks (5G/6G)

A partire dal **3GPP Release 17** (2022), le specifiche 5G NR includono ufficialmente il supporto per le **Non-Terrestrial Networks (NTN)**, integrando satelliti LEO/GEO come nodi di accesso della rete 5G. Tre modalità di integrazione sono standardizzate:

1. **NR-based satellite access**: il satellite si connette direttamente al 5G Core come un gNB non terrestre
2. **LTE-based satellite access**: accesso NB-IoT/eMTC via satellite connesso all'EPC 4G
3. **Satellite backhaul**: il satellite fornisce il trasporto tra 5G Core e gNB terrestri in aree remote

Nel futuro **6G (IMT-2030)**, la visione ETSI/3GPP prevede una convergenza totale tra reti terrestri e non-terrestri (TN+NTN), con handover trasparente tra celle terrestri 6G e satelliti LEO. L'intelligenza artificiale sarà usata per ottimizzare dinamicamente l'allocazione delle risorse tra i due sistemi.

## Confronto RF vs Ottico per Link Inter-Satellite

I futuri ISL (Inter-Satellite Links) si baseranno sempre più su tecnologia ottica:

| Parametro             | RF                     | Ottico (laser)             |
| --------------------- | ---------------------- | -------------------------- |
| **Banda**             | Licenziata             | Non licenziata             |
| **Velocità**          | ~100 Mb/s              | ~100 Gb/s                  |
| **Interferenze**      | Suscettibile           | Immune                     |
| **Sicurezza**         | Limitata               | Elevata                    |
| **Divergenza fascio** | Alta (ampio footprint) | Bassissima (link puntuale) |
| **Potenza (LEO-LEO)** | ~33.1 W                | ~77.8 W                    |

La divergenza angolare del fascio ottico segue la formula $\theta_{div}=2.44\cdot\lambda /D_R$​: per una piccola apertura ottica il fascio è molto più collimato di qualsiasi sistema RF, rendendo l'intercettazione praticamente impossibile. Starlink usa già ISL laser a 1550 nm sui propri satelliti di nuova generazione.

## Riepilogo

### Vantaggi

- Copertura globale incluse aree remote, oceani, zone polari
- Indipendenza dall'infrastruttura terrestre -> resilienza in caso di disastri
- Scalabilità rapida (nuovi satelliti vs. posa di fibra)

### Criticità

- **Latenza**: soprattutto per GEO (480 ms round-trip), problema per VoIP e gaming
- **Debris spaziale** (sindrome di Kessler): proliferazione di detriti orbitali in LEO
- **Rain fade**: attenuazione da pioggia nelle bande Ka/V, critica per link GEO
- **Jamming e spoofing**: vulnerabilità dei segnali GNSS e broadband satellitare
- **Costi di lancio**: pur ridotti da Falcon 9/Falcon Heavy, rimangono elevati

### Tendenze future

- **ISL ottici** per capacità >100 Gbps tra satelliti
- **Satelliti rigenerativi** con elaborazione AI a bordo
- **Quantum Key Distribution (QKD)** via satellite per comunicazioni ultra-sicure
- **6G NTN**: integrazione trasparente con reti terrestri per connettività ubiqua
