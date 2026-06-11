---
tags:
  - reti
  - physical
  - fibra
  - cavo
---
## Introduzione

Le **Optical Wireless Communications (OWC)** trasmettono dati tramite fasci di luce visibile, infrarossa o ultravioletta, senza occupare lo spettro a radiofrequenza (RF) regolamentato. A differenza dei sistemi RF, la luce non attraversa i muri, garantendo sicurezza intrinseca contro l'intercettazione e assenza di interferenze elettromagnetiche, caratteristica fondamentale per ospedali, aeromobili e impianti industriali. Lo spettro ottico mette a disposizione una **banda non regolamentata dell'ordine di centinaia di THz** (circa 200 THz solo nella finestra 700-1500 nm), senza costi di licenza.

Un ulteriore vantaggio rispetto alle RF è l'assenza di **multipath fading**: i fasci ottici sono altamente direzionali e coerenti, quindi le riflessioni parassite hanno impatto trascurabile a differenza dei segnali radio che si riflettono sulle pareti creando copie ritardate del segnale.
## Classificazione per Lunghezza d'Onda

Le OWC si suddividono in tre macrocategorie in base allo spettro utilizzato:

| Tecnologia                    | Range spettrale     | Banda teorica disponibile |
| ----------------------------- | ------------------- | ------------------------- |
| **VLC** - Visible Light Comm. | 390 - 750 nm        | ~320 THz                  |
| **FSO** - Free Space Optics   | 750 - 1600 nm       | ~20 THz                   |
| **UVC** - Ultraviolet Comm.   | 200 - 280 nm (UV-C) | ~30 PHz (teorico)         |
![[Pasted image 20260611215433.png]]

La banda UV-A e UV-B (280-400 nm) non si usa per le comunicazioni perché la luce solare copre quelle stesse lunghezze d'onda, generando rumore eccessivo sul ricevitore. L'infrarosso nelle finestre a 850 nm, 1310 nm e 1550 nm è particolarmente conveniente perché **riutilizza le stesse sorgenti laser già sviluppate per la fibra ottica**.
## Tecnologie OWC


![[Pasted image 20260611215833.png]]

### Visible Light Communication (VLC)

La VLC sfrutta l'**infrastruttura di illuminazione a LED** esistente (390-750 nm): modulando la corrente di pilotaggio, il LED si accende e si spegne a velocità dell'ordine del nanosecondo (~1 GHz), invisibili all'occhio umano, trasmettendo dati fino a **10 Gb/s**. I LED bianchi commerciali sono in realtà combinazioni RGB di tre LED a semiconduttore (GaN, GaP, GaAs), ognuno modulabile separatamente per aumentare la capacità con CSK (Color Shift Keying).
### Light Fidelity (LiFi)

Il **LiFi** è il sottoinsieme bidirezionale della VLC, introdotto da **Harald Haas nel 2011** e standardizzato con **IEEE 802.11bb (approvato giugno 2023)**. Opera nella banda 800-1000 nm (IR vicino) con velocità minima garantita di 10 Mb/s e massima teorica di **9.6 Gb/s**, compatibile con il layer MAC del Wi-Fi. Il **downlink** usa i LED di illuminazione a soffitto; l'**uplink** usa LED a infrarossi montati sul dispositivo dell'utente. Il segnale LiFi non attraversa le pareti, rendendo ogni stanza una cella separata ad altissima sicurezza fisica.
### Free Space Optics (FSO)

L'**FSO** utilizza **laser collimati** (tipicamente a 850 nm, 1064 nm o 1550 nm) in configurazione punto-punto outdoor, con divergenza del fascio di 0.1-2 mrad. A differenza della VLC che usa LED, i laser FSO consentono data rate di **1-100 Gb/s** su distanze fino a **10 km** in condizioni di cielo sereno. Il canale FSO è soggetto a fenomeni atmosferici caratterizzati dall'attenuazione totale:
$$
A(\lambda) = \alpha_{fog}(\lambda) + \alpha_{snow}(\lambda) + \alpha_{rain}(\lambda) + \alpha_{scattering}(\lambda) \quad [\text{dB/km}]
$$

L'attenuazione va da **0.1 dB/km** (cielo sereno) a **oltre 300 dB/km** (nebbia fitta), rendendo il collegamento di fatto inutilizzabile. La lunghezza d'onda preferita è 1550 nm per la minore attenuazione in nebbia, la compatibilità con gli amplificatori EDFA in banda C e la sicurezza per gli occhi (classe 1 eye-safe).
### Infrared Communication (IRC) e IrDA

L'**IrDA** (*Infrared Data Association*) è uno standard per comunicazioni IR a distanze inferiori a 1 m, principalmente NLoS. I profili di velocità vanno da **Serial IR (SIR) a 115.2 kb/s** fino a **Ultra Fast IR (UFIR) a 96 Mb/s**, con modulazione **PPM** (*Pulse Position Modulation*). Era usato in PDA, telefoni di prima generazione e stampanti; oggi è in declino a favore dei sistemi RF.
### Ultraviolet Communication (UVC)

Le comunicazioni **UV-C (200-280 nm)** sono caratterizzate da propagazione **NLoS** (Non-Line of Sight) grazie alla forte diffusione (scattering) nell'atmosfera a quelle lunghezze d'onda, rendendo possibile la comunicazione senza allineamento diretto. Trovano applicazione in **sistemi militari** e link outdoor a breve distanza dove la riservatezza è prioritaria.
### Optical Camera Communication (OCC)

Nelle **OCC** il ricevitore è una fotocamera (sensore CMOS, come quella di uno smartphone), che può essere divisa in settori per implementare **MIMO ottico** (*Multiple Input Multiple Output*): ogni settore del sensore funziona come un fotodiodo indipendente, consentendo la separazione spaziale di segnali provenienti da più LED o display. Le applicazioni includono navigazione indoor, realtà aumentata, V2V e V2I.
## Schema del Sistema OWC

Il trasmettitore converte il segnale elettrico in ottico (conversione E/O) tramite laser o LED, con segnale **sempre non negativo** (intensità ottica $\ge 0$) - a differenza dei sistemi coerenti in fibra che modulano la fase. Il ricevitore converte il segnale ottico in elettrico (O/E) tramite fotodiodo ([[Collegamenti in fibra ottica#Fotodiodo PIN|PIN]] o [[Collegamenti in fibra ottica#Fotodiodo a Valanga (APD)|APD]]) o sensore di immagine, seguito da processamento in banda base. I laser hanno **Field of View (FoV) ridotto** e richiedono allineamento LoS preciso; i LED hanno **FoV ampio** e tollerano configurazioni NLoS.

## Confronto con RF

| Parametro                | RF (Wi-Fi)                  | OWC (LiFi/FSO)       |
| ------------------------ | --------------------------- | -------------------- |
| Bit rate                 | Gb/s                        | > Gb/s               |
| Dimensione fascio        | > 20 m                      | ~ 2 m                |
| Attraversa le pareti     | Sì                          | No                   |
| Sensibilità alla pioggia | Elevata                     | Bassa                |
| Sensibilità alla nebbia  | Bassa                       | Molto elevata        |
| Spettro                  | Regolamentato (a pagamento) | Non regolamentato    |
| Multipath fading         | Sì                          | No (LoS direzionale) |
| Mobilità                 | Alta                        | Limitata             |

La pioggia attua principalmente sulle RF (le gocce, grandi rispetto a $\lambda_{RF}$, causano scattering), mentre la nebbia è devastante per l'FSO (le particelle di dimensione comparabile a $\lambda_{ottico}$ causano forte scattering di Mie).
## Sicurezza dei Laser (Eye Safety)

Le normative **IEC 60825-1** e **IEC 62471** classificano i laser da Classe 1 (sicuri) a Classe 4 (pericolosi, rischio incendio). Per $\lambda \lt 1400 \text{nm}$ la luce viene focalizzata dalla lente dell'occhio sulla retina, richiedendo limiti di potenza severi; per $\lambda \gt 1400 \text{nm}$ la cornea assorbe completamente la radiazione prima che raggiunga la retina, ma può danneggiarsi essa stessa. Per questo la finestra a **1550 nm (Classe 1 eye-safe)** è preferita nei sistemi FSO ad alta potenza.

| Classe       | Descrizione                                                                                                                                |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **Class 1**  | Safe                                                                                                                                       |
| **Class 1M** | Safe, provided optical instruments are not used                                                                                            |
| **Class 2**  | Visible lasers. Safe for accidental exposure $(\lt 0.25s)$                                                                                 |
| **Class 2M** | Visible lasers. Safe for accidental exposure $(\lt 0.25s)$, provided optical instruments, such as binoculars and telescopes, are not used. |
| **Class 3R** | Not safe. Low risk.                                                                                                                        |
| **Class 3B** | Hazardous. Viewing of diffuse reflection, from dull surfaces, is safe.                                                                     |
| **Class 4**  | Hazardous. Viewing of diffuse reflection is also hazardous. Fire risk.                                                                     |

## Applicazioni

### Ospedali e Ambienti RF-Sensibili

Le OWC (VLC/LiFi) consentono comunicazioni ad alta velocità (> 100 Mb/s) in ambienti dove le RF interferiscono con apparecchiature mediche o di sicurezza. Il collegamento ottico rimane confinato alla stanza, eliminando il rischio di interferenza cross-sala.
### Aeronautica

I LED di illuminazione di un aereo possono modulare il segnale trasmettendo dati diversi a ogni posto, ottenendo **accesso spaziale** senza condivisione di frequenze o slot temporali tra passeggeri. L'OWC è anche impiegato per link **aircraft-to-aircraft**, **aircraft-to-ground**, **aircraft-to-satellite** e controllo di droni.

### Reti Veicolari VANET/VLC-V2V

Le **VANET ottiche** usano VLC per comunicazioni **V2V (Vehicle-to-Vehicle)** e **V2I (Vehicle-to-Infrastructure)** a distanze di circa 100 m. Le applicazioni includono:

- **Luci di stop intelligenti**: trasmissione dell'evento frenata al veicolo seguente per frenata automatica
- **Platooning**: coordinazione ravvicinata (pochi centimetri) tra veicoli autonomi tramite dati di accelerazione e sterzo
- **Sistemi informativi sul traffico** e **pagamento elettronico del pedaggio**

### LANET - Visible Light Ad hoc Network

Le **LANET** sono reti ad hoc senza infrastruttura (senza access point) in cui i nodi comunicano via VLC in modalità single-hop o multi-hop. Il range operativo è limitato a **pochi metri** per la natura LoS della luce; il rumore principale è la luce solare diretta e le sorgenti di illuminazione non appartenenti alla rete.

### OWC Sottomarina (UOWC)

I sistemi **acustici sottomarini** tradizionali soffrono di banda ridotta (1 kHz-100 kHz), alta latenza e data rate nell'ordine dei kbps; i sistemi RF sono limitati a meno di 10 m per l'alta conduttività dell'acqua di mare. Le **UOWC (Underwater Optical Wireless Communications)** sfruttano la **finestra di bassa attenuazione nel blu-verde (450-550 nm)** dell'acqua marina, raggiungendo data rate nell'ordine dei **Gbps** su distanze di 10-150 m (potenzialmente fino a 500 m) con bassa latenza. Il segnale è attenuato per assorbimento e scattering da particelle in sospensione (acqua torbida: ~11 dB/m vs ~0.39 dB/m in oceano aperto); le sorgenti preferite sono **laser a diodo verde e APD** come fotorivelatori.

| Parametro | Acustico  | RF       | Ottico (UOWC) |
| --------- | --------- | -------- | ------------- |
| Data rate | kbps      | Mbps     | Gbps          |
| Distanza  | > 100 km  | ≤ 10 m   | 10-150 m      |
| Latenza   | Alta      | Moderata | Bassa         |
| Banda     | 1-100 kHz | MHz      | ~150 MHz      |

### Backhaul 5G e Last Mile

L'FSO è usato come **backhaul ottico** per stazioni base 5G quando la posa di fibra non è praticabile, o come collegamento di emergenza in caso di disastri naturali (terremoti, alluvioni) che danneggino la fibra interrata.
## Schemi di Modulazione VLC

| Schema    | Data rate     | Caratteristiche                     | Uso                  |
| --------- | ------------- | ----------------------------------- | -------------------- |
| **OOK**   | 1-2 Gb/s      | Bassa complessità                   | VLC generico         |
| **OFDM**  | > 10 Gb/s     | Alta efficienza spettrale           | VLC ad alta capacità |
| **PPM**   | moderato      | Efficiente energeticamente          | IrDA                 |
| **PAM-4** | moderato-alto | DSP semplice, 4 livelli di ampiezza | LiFi IEEE 802.11bb   |

## Standard di Riferimento

- **IEEE 802.11bb (2023)**: standard LiFi; PHY a 800-1000 nm, downlink fino a 9.6 Gb/s, compatibile con MAC 802.11 Wi-Fi; uplink su LED IR
- **IEEE 802.15.7 (2018)**: standard VLC; definisce PHY I/II/III con bit rate da 11.67 kb/s a 96 Mb/s, supporta dimming e resa cromatica
- **IrDA**: standard IR a distanze < 1 m; SIR 115.2 kb/s -> UFIR 96 Mb/s; modulazione PPM
- **ITU-T G.FSO / ETSI GS FSO**: requisiti per apparecchiature FSO, modelli di link budget atmosferico; preferenza per 1550 nm (eye-safe, compatibile EDFA)