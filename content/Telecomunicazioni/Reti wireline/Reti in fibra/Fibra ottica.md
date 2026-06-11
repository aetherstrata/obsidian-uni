---
tags:
  - wireline
  - cavo
  - canale
  - fibra
---
La **fibra ottica** è un mezzo trasmissivo guidato in cui l'informazione viaggia sotto forma di **impulsi luminosi** (luce infrarossa, non visibile) anziché segnali elettrici. Le sue proprietà la rendono superiore a qualsiasi mezzo metallico per applicazioni a lunga distanza e ad alta capacità:
- **Larghezza di banda nell'ordine dei Tbit/s** (vs ~200 MHz del [[Doppino telefonico|rame]] G.fast)
- **Attenuazione di soli 0,2 dB/km** a 1550 nm (vs > 10 dB/100 m del [[Doppino telefonico|rame]])
- **Totale immunità alle interferenze elettromagnetiche**: nessun campo EM esterno penetra nella fibra
- **Sicurezza**: il segnale non si irradia all'esterno, difficile da intercettare
- **Latenza**: la luce si propaga a circa 2/3 della velocità nel vuoto (~200.000 km/s)
## Struttura Fisica

Una fibra ottica è un filo di vetro (_biossido di silicio_, $SiO_2$) composto da tre strati concentrici:

![[Pasted image 20260609215938.png]]

### Nucleo (Core)
Il nucleo è formato da un cilindro centrale in vetro drogato con ossidi (es. germanio) per alzare leggermente l'indice di rifrazione. 

Il vetro ha un indice di rifrazione di circa $n_g\simeq1.5$ e ha un diametro che varia in base alle esigenze di trasmissione: **9** μm (monomodo) o **50-62,5** μm (multimodo).

### Mantello (Cladding)
Il mantello è uno strato esterno in vetro con indice di rifrazione $n_1<n_g$ , la differenza è piccola (es. da 1,50 a 1,48). È la discontinuità tra questi due indici che crea la riflessione totale.

### Rivestimento (Coating)
Il rivestimento è una guaina protettiva in materiale polimerico (acrilato o poliammide) che protegge meccanicamente la fibra. Il diametro totale della fibra è **125** μm, più sottile di un capello (**~80** μm).

## Principio di Propagazione

La luce rimane confinata nel core grazie alla **riflessione totale interna**, descrivibile con la **legge di Snell** all'interfaccia core-cladding
$$
n_g\sin⁡\theta_i=n_1\sin\theta_t
$$
![[Pasted image 20260609220120.png]]

Quando l'angolo di incidenza $\theta_i$​ supera il valore critico $\theta_{cr}$, l'angolo trasmesso $\theta_t$ tenderebbe a superare 90°, che è fisicamente impossibile, quindi **tutta la potenza luminosa viene riflessa** all'interno del core, senza perdite nel mantello. L'angolo critico si ricava ponendo $\theta_t=90\degree$:
$$
\sin\theta_{cr}=\frac{n_1}{n_g}​​
$$

![[Pasted image 20260609220220.png]]

Se $\theta_i \lt \theta_{cr}$​, parte della luce si perde nel cladding e il segnale si attenua rapidamente. La condizione $\theta_t\gt\theta_{cr}$​ deve essere rispettata per tutta la lunghezza della fibra, anche in curva , ecco perché esiste un raggio di curvatura minimo ammissibile.

![[Pasted image 20260609220324.png]]
### Apertura Numerica (NA)

L'**apertura numerica** misura la capacità della fibra di accettare luce proveniente dall'esterno, ovvero l'ampiezza del cono di accettazione entro cui la luce viene trasmessa:
$$
\text{NA}=\sin\theta_{max}=\sqrt{n_g^2−n_1^2}​​
$$
Una NA più alta significa maggiore facilità di accoppiamento con la sorgente luminosa, ma anche più modi di propagazione (fibra multimodo) e maggiore dispersione intermodale.

### Modi di Propagazione

Le onde che si riflettono all'interno del core interferiscono tra loro. Solo quelle che danno luogo a un'**onda stazionaria** (interferenza costruttiva) si propagano stabilmente: questi pattern sono detti **modi di propagazione**.

I modi si classificano come **LP (Linearly Polarized)** nel formalismo scalare, oppure come modi vettoriali (TE, TM, HE, EH). Il modo fondamentale è $LP_{01} / HE_{11}$. La fibra è **monomodo** se il parametro $V$ (frequenza normalizzata) soddisfa:
$$
V=\frac{2\pi}{\lambda}\cdot a\cdot\text{NA}<2.405
$$
dove $a$ è il raggio del core e \lambda\lambda la lunghezza d'onda. Per $V<2.405$ si propagano solo il modo fondamentale e il suo polarizzazione degenere, non ci sono altri modi guidati.

## Fibre Multimodo (MMF)

![[Pasted image 20260609220609.png]]

- Core ampio (**50** o **62,5** μm) e una NA più elevata rendono la fibra più semplice da installare e connettere
- Più modi di propagazione con percorsi diversi -> **[[#Dispersione Intermodale (MMF)]]**
- Usate con sorgenti VCSEL a 850 nm a basso costo
- Distanze massime: **da 33 m a 550 m** a seconda dello standard
- **Applicazioni**: LAN, data center (dove le distanze sono brevi e il costo dei transceiver è critico)

| Standard | Core                       | Anno | Sorgente     | 10 Gbit/s | 100 Gbit/s |
| -------- | -------------------------- | ---- | ------------ | --------- | ---------- |
| **OM1**  | 62,5/125 μm | 1989 | LED          | 33 m      | ,          |
| **OM2**  | 50/125 μm   | 1998 | LED          | 82 m      | ,          |
| **OM3**  | 50/125 μm   | 2002 | VCSEL 850 nm | 300 m     | 100 m      |
| **OM4**  | 50/125 μm   | 2009 | VCSEL 850 nm | 550 m     | 150 m      |
| **OM5**  | 50/125 μm   | 2014 | VCSEL, SWDM  | 440 m     | 150 m      |
## Fibre Monomodo (SMF)

![[Pasted image 20260609220639.png]]

- Core sottile (**9** μm) e una NA ridotta limitano la propagazione a un solo modo, nessuna dispersione intermodale
- Richiede laser a banda stretta e connettori con allineamento preciso
- Distanze: **migliaia di km** (cavi sottomarini con amplificatori EDFA ogni ~100 km)
- **Applicazioni**: reti di accesso FTTH/GPON, reti metropolitane, backbone, cavi sottomarini
- Costo dei transceiver circa **5 volte superiore** ai sistemi MMF

## Dispersione: Cause e Effetti

La **dispersione** causa l'**allargamento degli impulsi** nel dominio del tempo: se l'allargamento supera il periodo di simbolo $T_s$​, i bit adiacenti si sovrappongono causando **[[Condizione di Nyquist sulle interferenze|ISI]]** (*Inter-Symbol Interference*) e conseguente aumento del [[Quantizzazione#Bit Error Rate|BER]].

![[Pasted image 20260609224617.png]]
### Dispersione Intermodale (MMF)

In una fibra multimodo, modi diversi percorrono cammini di lunghezza diversa -> arrivano in tempi diversi. L'allargamento temporale è:
$$
\Delta\tau_m=\frac{\text{NA}^2\cdot L}{2 n_g c}​
$$

![[Pasted image 20260609224242.png]]

L'allargamento cresce **linearmente con la lunghezza** $L$ della fibra, il che è il motivo per cui le MMF sono limitate alle brevi distanze dei data center.

![[Pasted image 20260609224946.png]]
### Dispersione Cromatica (SMF)
In una fibra monomodo non c'è dispersione intermodale, ma il laser non emette una singola frequenza perfetta: ha una certa larghezza spettrale $\Delta\lambda$ Ogni componente spettrale si propaga alla propria velocità di gruppo -> allargamento dell'impulso:
$$
\Delta\tau_s=D\cdot L\cdot\Delta\lambda\quad [ps]
$$
dove $D$ è il **parametro di dispersione cromatica** della fibra $[\text{ps}/(\text{nm}\cdot\text{km})]$. Per una fibra SMF standard (G.652), $D\approx16\ \text{ps}/(\text{nm}\cdot\text{km))}$ a 1550 nm. Su una fibra da 70 km con $\Delta\lambda=1\ \text{nm}$ si accumulano circa 1120 ps di dispersione.

![[Pasted image 20260609224438.png]]

### Compensazione della Dispersione Cromatica
#### DCF (Dispersion Compensating Fiber)
Alcuni tratti di fibra utilizzano materiali speciali con $D$ **negativo** (tipicamente $-80\   \text{ps}/\text{nm}\cdot\text{km}$) inserita nel link per annullare la dispersione accumulata. La condizione di compensazione è:
$$
D_{TF}\cdot L_{TF}+D_{DCF}\cdot L_{DCF}=0
$$
#### DSP (Digital Signal Processing)
La compensazione avviene nel dominio elettrico, dopo una conversione ottico-elettrica; standard nei sistemi coerenti moderni (100G, 400G DP-QAM)

## Dispersione da Polarizzazione (PDM)

La luce può propagarsi su **due polarizzazioni ortogonali** X e Y. A causa della birifrangenza della fibra (asimmetrie geometriche o stress meccanico), le due polarizzazioni viaggiano a velocità leggermente diverse, causando un ritardo relativo che allarga ulteriormente l'impulso.

![[Pasted image 20260609225026.png]]

Il **PDM** (*Polarization Division Multiplexing*) sfrutta proprio queste due polarizzazioni per trasmettere **due canali indipendenti** sulla stessa lunghezza d'onda, raddoppiando la capacità. Al trasmettitore un **PBC** (*Polarization Beam Combiner*) combina i due flussi, mentre al ricevitore un **PBS** (*Polarization Beam Splitter*) con DSP li separa. Questa tecnica è alla base degli standard **DP-QAM** usati nei backbone a 100G/400G.

## Attenuazione

La potenza ottica decade esponenzialmente con la distanza:
$$
P(z)=P(0) e^{-\alpha z}
$$
Il coefficiente di attenuazione $\alpha_{\text{dB}}$ dipende fortemente dalla lunghezza d'onda. Le cause principali sono lo **scattering di Rayleigh** (dominante alle lunghezze d'onda corte) e l'**assorbimento da ioni** $\text{OH}^-$ (picco di assorbimento a ~1383 nm). 

![[Pasted image 20260609225650.png]]

Si definiscono tre **finestre operative** standard:

|Finestra|Lunghezza d'onda|Attenuazione tipica|Note|
|---|---|---|---|
|**Prima**|850 nm|~2,5 dB/km|Usata con fibre MMF e VCSEL a basso costo|
|**Seconda**|1300 nm|~0,4 dB/km|Zero-dispersion standard, ISDN ottico|
|**Terza**|1550 nm|~0,2 dB/km|Minima attenuazione, DWDM, backbone|

Le fibre **AllWave®** (e similari) eliminano il picco $\text{OH}^-$ a 1383 nm, rendendo utilizzabile l'intera banda da 1270 a 1625 nm per il CWDM.

## Wavelength Division Multiplexing (WDM)

Il **WDM** (*Wavelength Division Multiplexing*) permette di trasmettere più segnali a **lunghezze d'onda diverse** sulla stessa fibra, sfruttando la vastissima banda ottica disponibile. I MUX/DEMUX ottici combinano e separano i canali senza conversione ottico-elettrica.

|Caratteristica|**CWDM** (Coarse)|**DWDM** (Dense)|
|---|---|---|
|Standard|ITU-T G.694.2|ITU-T G.694.1|
|Spaziatura canali|20 nm|0,8 nm (100 GHz) o 0,4 nm (50 GHz)|
|Finestra operativa|1270-1610 nm|Banda C: 1530-1565 nm|
|Numero canali|4, 8 o 16|40, 80 o 96|
|Bit rate per canale|1-10 Gbit/s|10-400 Gbit/s (DP-QAM)|
|Laser|Non termostatato|Termostatato (±0,1 nm)|
|Costo|Basso|Alto|
|Distanza|40-80 km (non amplificato)|> 1000 km (con EDFA)|
|Applicazioni|Reti di accesso, data center|Backbone, reti metro, sottomarino|

I vantaggi principali del DWDM includono: aumento della capacità senza stendere nuove fibre, **wavelength routing** (instradamento basato sulla lunghezza d'onda), e indipendenza dal protocollo o dal bit rate per canale.

## Amplificatori EDFA

La limitazione della distanza per attenuazione è superata dagli **EDFA** (*Erbium-Doped Fiber Amplifiers*), che amplificano il segnale ottico **senza conversione ottico-elettrica**.

### Principio di funzionamento

Il core di un tratto di fibra (~10-30 m) viene drogato con ioni di **Erbio** ($\text{Er}^{3+}$). Un laser di pompaggio (_pump laser_) a **980 nm o 1480 nm** eccita gli ioni $\text{Er}^{3+}$ al livello energetico superiore. Quando un fotone del segnale (a ~1550 nm) attraversa la fibra drogata, innesca l'**emissione stimolata** degli ioni eccitati: ogni fotone del segnale genera un fotone identico, amplificando il segnale con guadagno fino a 30-40 dB. 

- **Banda operativa**: Banda C (1530-1565 nm) e Banda L (1570-1610 nm) , esattamente la terza finestra ottica a minima attenuazione
- **Vantaggio chiave**: amplifica **tutti i canali DWDM contemporaneamente** con un unico dispositivo
- **Svantaggi**: aggiunge rumore ASE (Amplified Spontaneous Emission) e non compensa la dispersione cromatica
## Link Power Budget

Per progettare un collegamento ottico si calcola il **link power budget**: la differenza tra la potenza emessa dal trasmettitore e la sensibilità del ricevitore deve essere sufficiente a compensare tutte le perdite del link, con un margine di sicurezza:
$$
\text{Margine}=P_{TX}-\text{Perdite totali}-P_{RX,min}⁡>0 \text{dB}
$$
**Perdite da considerare**: attenuazione fibra ($\text{dB}/\text{km} \times \text{km}$), perdite nei connettori, nei giunti, negli splitter ottici, eventuale dispersione non compensata.

Questo è un esempio di come vengono usati i $\text{dBm}$ per misurare la potenza così da poter velocemente fare i calcoli con le attenuazioni in $\text{dB}$:

|Voce|Valore|
|---|---|
|Potenza trasmessa|0,0 dBm (1 mW)|
|Attenuazione fibra (~20 dB)|−20,0 dB|
|Potenza ricevuta|−20,0 dBm|
|Sensibilità ricevitore|−30,0 dBm|
|**Margine del link**|**+10,0 dB**|
## Connettori

I connettori permettono connessioni **smontabili** tra fibre o tra fibra e transceiver. Introducono un'**insertion loss** tipica di 0,3-0,5 dB e un **return loss** (riflessione all'interfaccia) che deve essere minimizzato , un'interfaccia fibra-aria-fibra non lavorata introduce ~14 dB di attenuazione.

| Connettore  | Ferrula            | Applicazione                                            |
| ----------- | ------------------ | ------------------------------------------------------- |
| **LC**      | 1,25 mm            | Standard per moduli SFP/SFP+, alta densità, data center |
| **SC**      | 2,5 mm             | Cablaggio strutturato, FTTH, PON                        |
| **FC**      | 2,5 mm (vite)      | Strumentazione di misura, alta stabilità meccanica      |
| **ST**      | 2,5 mm (baionetta) | LAN legacy, applicazioni industriali                    |
| **MPO/MTP** | Multifibra (12-24) | Backbone ad alta densità 40G/100G, data center          |
### Moduli Transceiver

I moduli **SFP (Small Form-factor Pluggable)** integrano trasmettitore (laser) e ricevitore (fotorivelatore) in un unico package estraibile a caldo:
- **SFP**: fino a 1 Gbit/s
- **SFP+**: fino a 10 Gbit/s
- **SFP28**: 25 Gbit/s
- **QSFP+**: 4×10 = 40 Gbit/s
- **QSFP28**: 4×25 = 100 Gbit/s
### Giunti a Fusione

Per connessioni **permanenti** si usano i **giunti a fusione**: le due estremità di fibra vengono riscaldate a ~2000°C da una scarica elettrica tra elettrodi di tungsteno, fondendosi insieme. La tensione superficiale produce un **auto-allineamento** che minimizza le perdite. I valori di attenuazione tipici sono nell'ordine dei **centesimi di dB**, con alta stabilità meccanica e termica , ideali per le reti backbone e i cavi sottomarini.
### Plastic Optical Fiber (POF)

La **POF (Plastic Optical Fiber)** è una variante a basso costo con core in **PMMA** (polimetilmetacrilato) di diametro circa **1 mm**. Caratteristiche:

- Estremamente flessibile e facile da connettere (nessun allineamento critico)
- Attenuazione elevata (~100-200 dB/km), adatta solo a distanze < 50 m
- Bit rate fino a **1 Gbit/s** su distanze < 50 m
- **Applicazioni**: cablaggio domestico, reti automotive (standard MOST), sistemi A/V consumer