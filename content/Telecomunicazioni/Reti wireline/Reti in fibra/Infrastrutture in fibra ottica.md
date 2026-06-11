---
tags:
  - wireline
  - infrastruttura
  - fibra
---

## Ambiti di utilizzo

### Data Center

Nei data center la fibra interconnette server, storage e switch con latenza ultra-bassa:

- **Top of Rack (ToR)**: connessioni da 10G, 25G o 50G per server, con [[Fibra ottica#Fibre Multimodo (MMF)|fibra MMF]] e VCSEL
- **Spine-Leaf (backbone interno)**: collegamenti tra switch a 100G, 400G e fino a 800G
- **Data Center Interconnect (DCI)**: [[Fibra ottica#Fibre Monomodo (SMF)|fibra SMF]] con DWDM a 100G/400G tra DC geograficamente distanti
- **Cablaggio ad alta densità**: connettori MPO/MTP con 12, 24 o 72 fibre, organizzati in _Zone Distribution Area_ (**ZDA**)

Lo standard **Fibre Channel (FC)** a 16G, 32G e 64G è lo standard per le SAN (Storage Area Network).

### Backhaul e Fronthaul Mobile (5G)

L'architettura delle reti mobili 5G è suddivisa in tre segmenti di trasporto in fibra:

- **Fronthaul**: tratto dalla BBU/DU (a terra) alla AAU/RRU (in cima alla torre) che trasporta segnali radio digitalizzati con protocolli **CPRI/eCPRI** su fibra SMF a 25G o 100G; richiede latenza < 1 ms
- **Middlehaul**: nelle architetture C-RAN, collega le Distributed Unit (DU) alle Centralized Unit (CU)
- **Backhaul**: rete IP/MPLS su fibra DWDM che porta il traffico aggregato verso il core 5G, con QoS differenziata

### Reti Metropolitane (MAN)

Le **Metropolitan Area Networks** interconnettono edifici, PoP e nodi di aggregazione entro un'area urbana. Le topologie tipiche sono ad **anello ridondante (ring)**, magliato (mesh) o punto-punto, con fibra posata in cavidotti interrati. Gli elementi chiave sono switch Ethernet 100G/400G e amplificatori EDFA. Il monitoraggio avviene tramite **OTDR** per la localizzazione dei guasti e piattaforme OSS/NMS.

### Reti Dorsali e Sottomarine

Le reti backbone trasportano volumi enormi di traffico tra nodi primari; i **cavi sottomarini** collegano i continenti. Le caratteristiche principali sono:

- Fibre monomodali SMF con amplificatori EDFA ogni 50–100 km
- DWDM con centinaia di lunghezze d'onda
- Protezione multi-strato contro pressione, corrosione e abrasione, fino a 8.000+ m di profondità
- Oltre **1,3 milioni di km** di cavi sottomarini attivi nel mondo
- Vita media: 25 anni
- Stazioni di atterraggio (_landing stations_) costiere

Il primo cavo sottomarino transatlantico in fibra (TAT-8) fu posato nel **1988**, segnando la definitiva transizione dal rame alla fibra per le connessioni intercontinentali.

### Reti di Accesso PON

Le reti **PON** (_Passive Optical Network_) portano la fibra fino all'utente finale (FTTH) o all'armadio di quartiere (FTTC). Il termine "passivo" indica che tra la centrale (OLT) e l'utente (ONU/ONT) non ci sono elementi attivi alimentati: il segnale viene splittato tramite **splitter ottici passivi**. Gli standard evolutivi sono:

| Standard                   | Velocità DL      | Velocità UL      | Note                                              |
| -------------------------- | ---------------- | ---------------- | ------------------------------------------------- |
| **GPON** (ITU-T G.984)     | 2,5 Gbit/s       | 1,25 Gbit/s      | Fino a 128 ONU per OLT                            |
| **XG-PON** (ITU-T G.987)   | 10 Gbit/s        | 2,5 Gbit/s       | Asimmetrica                                       |
| **XGS-PON** (ITU-T G.9807) | 10 Gbit/s        | 10 Gbit/s        | Simmetrica, compatibile GPON                      |
| **NG-PON2**                | fino a 40 Gbit/s | fino a 40 Gbit/s | TWDM su 4 $\lambda$, usato anche per 5G fronthaul |

Nella configurazione FTTH, l'OLT usa **modulazione diretta** su [[Fibra ottica#Fibre Monomodo (SMF)|fibra SMF]]: downlink (centrale -> utente) in terza finestra (1550 nm), uplink (utente -> centrale) in seconda finestra (1310 nm). L'FTTC arriva con fibra all'armadio stradale e poi prosegue su doppino in rame, con prestazioni massime di ~200 Mbit/s.

## Evoluzione Storica

La progressione tecnologica delle reti in fibra segue tappe ben definite:

1. **Anni '70**: Charles Kao dimostra che la fibra attenua meno del rame (Premio Nobel 2009); prime sorgenti $\text{GaAs}$ a 850 nm
2. **1988**: TAT-8, primo cavo sottomarino transatlantico in fibra monomodale
3. **1990**: EDFA e DWDM rivoluzionano la capacità: più canali sulla stessa fibra, amplificazione ottica senza rigenerazione elettrica
4. **2000**: Bolla dotcom: eccesso di infrastrutture, dark fiber inutilizzate per anni
5. **2010**: Sistemi coerenti con DSP; sfruttamento di ampiezza, fase e polarizzazione (5 dimensioni del segnale)
6. **2015**: Open Fiber e piano Banda Ultra Larga per copertura FTTH delle aree bianche in Italia

## Frontiere Tecnologiche

Le tendenze di ricerca attuali puntano a superare i limiti della fibra convenzionale:

- **Fibre multicore (MCF) e few-mode (FMF)**: moltiplicano la capacità tramite _Space Division Multiplexing (SDM)_: più core indipendenti in una singola fibra senza nuova infrastruttura
- **Sistemi coerenti nuova generazione**: modulazioni avanzate (es. QAM-64, QAM-256) con DSP ad alta velocità; obiettivo > 100 Tbit/s per fibra, avvicinandosi al **limite di Shannon**
- **Reti quantistiche (QKD)**: la fibra è il canale preferenziale per la distribuzione di chiavi crittografiche quantistiche; Internet quantistico con repeater a entanglement per trasmissione di qubit su scala intercontinentale
- **Integrazione 5G/6G**: ogni antenna 5G richiede fronthaul in fibra ad altissima capacità con latenza sub-millisecondale per IoT massivo e veicoli autonomi
