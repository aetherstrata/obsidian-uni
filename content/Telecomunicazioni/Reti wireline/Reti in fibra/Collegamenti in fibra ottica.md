---
tags:
  - cavo
  - fibra
  - physical
  - infrastruttura
---
Un collegamento in fibra ottica è composto da tre elementi fondamentali: un **trasmettitore ottico**, il **canale di comunicazione** (la fibra), e un **ricevitore ottico**. Il trasmettitore comprende un codificatore, un modulatore e un laser; il ricevitore contiene un filtro ottico, un fotorivelatore e un filtro elettrico per recuperare i bit. Nei sistemi terrestri, ogni tratto (span) è tipicamente di circa **100 km**, al termine del quale si inserisce un amplificatore ottico prima di ripetere il collegamento.

## Principi Fisici

L'energia di un fotone è descritta dalla relazione di Planck-Einstein: $E = h\nu$, dove $h$ è la costante di Planck e $\nu$ la frequenza. I tre meccanismi fondamentali di interazione tra fotone e materia sono:

- **Assorbimento**: il fotone cede la sua energia a un elettrone, portandolo dalla banda di valenza a quella di conduzione. Il fotone cessa di esistere
- **Emissione spontanea**: un elettrone eccitato decade spontaneamente generando un fotone di fase e direzione casuali. Questo è il principio del LED
- **Emissione stimolata**: un fotone incidente su un atomo eccitato provoca il rilascio di un secondo fotone _identico_ (stessa frequenza, fase, direzione). Questo è il principio del **LASER** (*Light Amplification Stimulated Emission Radiation*)

![[Pasted image 20260610231953.png]]
## Sorgenti Ottiche

Le sorgenti devono emettere nelle **tre finestre di bassa attenuazione** della silice: ~850 nm (I finestra), ~1310 nm (II), ~1550 nm (III). I LED a semiconduttore hanno una larghezza spettrale di $30\text{-}60\ \text{nm}$ e potenza emessa inferiore a −10 dBm; sono economici ma poco adatti alle telecomunicazioni ad alta capacità a causa della radiazione incoerente. I laser producono luce coerente grazie alla cavità risonante, e il materiale semiconduttore determina la lunghezza d'onda: $\text{GaAs}$ per la prima finestra, $\text{InGaAsP}$ per la seconda e terza.
## Tipi di Laser a Semiconduttore

| Tipo                                  | Linewidth               | Range tipico         | Applicazioni                            |
| ------------------------------------- | ----------------------- | -------------------- | --------------------------------------- |
| **Fabry-Pérot (FP)**                  | 3-10 nm (500-1750 GHz)  | fino a 20 km         | FTTH lato utente, breve distanza        |
| **DFB (Distributed Feedback)**        | ~0.1 nm (12.5 GHz)      | 40 km+               | Long-haul, DWDM, 5G CPRI/eCPRI          |
| **VCSEL**                             | stretta, monomodo       | ~300 m a 10 Gbps     | Data center, fibra multimodale a 850 nm |
| **DBR (Distributed Bragg Reflector)** | stretta, sintonizzabile | media/lunga distanza | Simile DFB, più accordabile             |

![[Pasted image 20260610231636.png]]

Il laser **Fabry-Pérot (FP)** è il tipo di laser a semiconduttore più semplice: la cavità risonante è formata dalle **due facce clivate** del cristallo semiconduttore, che fungono da specchi parzialmente riflettenti. Poiché la cavità supporta più modi longitudinali contemporaneamente, lo spettro di emissione risulta relativamente largo (3–10 nm), rendendolo inadatto al DWDM ma sufficiente per i collegamenti a breve distanza dove la dispersione cromatica non è critica.

![[Pasted image 20260610231705.png]]

Il laser **DFB** utilizza un reticolo di Bragg integrato nel mezzo attivo che seleziona una sola lunghezza d'onda con feedback distribuito, garantendo emissione a singolo modo longitudinale e alta stabilità in frequenza. 

![[Pasted image 20260610231836.png]]

Il **VCSEL** emette perpendicolarmente al wafer (surface-emitting), ha bassissimo consumo e profilo circolare del fascio, ideale per il test in produzione e le connessioni intra-data-center.
## Modulazione del Segnale

### Modulazione Diretta (DML)

Nella **modulazione diretta**, il segnale dati viene convertito in corrente di iniezione dal laser driver, modulando direttamente la potenza luminosa uscente. Il laser opera sopra la corrente di soglia $I_{th}$​: al di sotto non c'è emissione laser (solo spontanea), al di sopra la potenza cresce linearmente con la corrente. Questo approccio è semplice ed economico, ma soffre di **chirp** (variazione di frequenza durante la transizione 0 -> 1), che limita la distanza e il bit rate.

![[Pasted image 20260610232352.png]]

### Modulazione Esterna (EOM)

Nella **modulazione esterna**, il laser opera in onda continua (CW) e un modulatore separato impone il segnale sulla luce. Il modulatore più usato è il **Mach-Zehnder Modulator (MZM)**: la luce viene divisa in due rami in un materiale elettro-ottico; applicando una tensione si varia l'indice di rifrazione e quindi la differenza di fase tra i due percorsi; alla ricombinazione si ha interferenza costruttiva o distruttiva che converte la fase in ampiezza. I modulatori EA (Electro-Absorption) sono più compatti e integrabili sullo stesso chip del laser DFB.

![[Pasted image 20260610232416.png]]

### On-Off Keying (OOK)

La modulazione più semplice è l'**OOK**: modulazione di ampiezza su due livelli (bit 1 = luce, bit 0 = no luce). In ricezione si usa la **rivelazione diretta**: il segnale va direttamente al fotorivelatore che misura l'intenstensità, con relazione $I = R \cdot |E_{sig}|^2$, dove $R$ è la responsività (A/W).
## Modulazione Coerente

I sistemi coerenti sfruttano **ampiezza, fase e polarizzazione** del campo ottico, avvicinandosi al limite teorico di Shannon. Si usa un **modulatore IQ** che combina due portanti ortogonali (sfasate di 90°) per trasmettere le componenti in-phase (I) e in-quadratura (Q) del segnale, implementato otticamente con due MZM in parallelo.

In ricezione, il segnale ricevuto $E_s$ viene fatto interferire con un **oscillatore locale** (laser locale) $E_{LO}$​ alla stessa lunghezza d'onda in un **90° optical hybrid**, che produce quattro uscite combinate inviate a fotodiodi differenziali. La fotocorrente complessa risultante è:
$$
\tilde{i}(t) \propto 2 E_s E_{LO}^*
$$
cioè proporzionale al campo del segnale (non alla sua potenza), permettendo di recuperare fase e ampiezza. Ciò abilita formati come **QPSK, 16-QAM, 64-QAM** e la trasmissione su **due polarizzazioni ortogonali** (DP - Dual Polarization), raddoppiando la capacità a parità di banda. Un sistema DP-QPSK a 400 Gb/s usa tipicamente $4\times50$ Gbaud con **DSP** (*Digital Signal Processing*) per correggere dispersione cromatica, PMD e rumore di fase.
## Fotorivelatori

### Fotodiodo PIN

Il **fotodiodo PIN** sfrutta l'effetto fotoelettrico: un fotone con energia $E = h\nu \geq E_g$ viene assorbito nello strato intrinseco (I) generando una coppia elettrone-lacuna. Polarizzato inversamente, in assenza di luce non scorre corrente; ogni fotone assorbito produce una coppia che genera la **fotocorrente** $I_p = R \cdot P_{in}$. Lo strato intrinseco controlla la larghezza della zona di svuotamento, riducendo la corrente diffusiva e aumentando la velocità di risposta.

![[Pasted image 20260610232639.png]]

**Parametri tipici del PIN:**

| Parametro                 | Valore tipico           |
| ------------------------- | ----------------------- |
| Responsività              | 0.8 - 0.95 A/W          |
| Banda (3 dB)              | fino a 50 GHz           |
| Corrente di buio          | 1 - 10 nA               |
| Tensione polariz. inversa | 5 - 15 V                |
| NEP                       | $\approx10^{-14}$ W/√Hz |

![[Pasted image 20260610232734.png]]

### Fotodiodo a Valanga (APD)

L'**APD (Avalanche PhotoDiode)** aggiunge una regione di moltiplicazione ad alto campo elettrico (~100 V) dove le coppie fotogenerate innescano ionizzazione a impatto: un fotone può generare $10\text{-}100$ coppie secondarie (guadagno M). Ciò aumenta la responsività effettiva di M volte, ma a scapito di maggiore corrente di buio, tensione di polarizzazione più elevata (30-100 V) e banda ridotta (fino a 20 GHz). Il caso estremo è lo **SPAD (Single Photon Avalanche Diode)**: APD operato oltre la tensione di breakdown, in grado di rilevare singoli fotoni, usato nella crittografia quantistica (QKD).

### Scelta del Materiale

| Materiale  | Range spettrale    | Velocità | Corrente di buio | Costo |
| ---------- | ------------------ | -------- | ---------------- | ----- |
| **Si**     | Visibile - 1100 nm | Alta     | Bassa            | Basso |
| **Ge**     | NIR                | Bassa    | Alta             | Basso |
| **InGaAs** | 900 - 1700 nm      | Alta     | Bassa            | Medio |
| **GaAs**   | Visibile - 870 nm  | Alta     | Bassa            | Medio |

$\text{Si}$ e $\text{Ge}$ hanno **bandgap indiretto**: non possono lascerare, ma possono assorbire fotoni. Per la terza finestra (1550 nm) il materiale di riferimento è **InGaAs**.
## Amplificatori Ottici

### EDFA (Erbium Doped Fiber Amplifier)

L'**EDFA** droga la fibra con ioni Erbio ($\text{Er}^{3+}$) che vengono eccitati da un **laser di pompa** a 980 nm o 1480 nm. I fotoni del segnale a 1550 nm inducono emissione stimolata dagli ioni eccitati, amplificando il segnale senza conversione elettro-ottica. L'EDFA è **trasparente al bit rate e al formato di modulazione**: amplifica tutti i canali WDM contemporaneamente in banda C (1530-1565 nm) e L (1565-1625 nm).

**Parametri tipici EDFA:**

| Parametro              | Valore                              |
| ---------------------- | ----------------------------------- |
| Guadagno               | 20 - 40 dB                          |
| Noise Figure (NF)      | 3 - 5 dB (limite quantistico: 3 dB) |
| Numero canali WDM      | fino a 80 (spaziatura 50 GHz)       |
| Potenza di saturazione | +10 ÷ +17 dBm                       |
| Consumo                | ~15 W                               |

Il rumore è generato dall'**ASE (Amplified Spontaneous Emission)**: gli ioni eccitati decadono spontaneamente emettendo fotoni a banda larga non correlati con il segnale. Si usa in tre configurazioni: come **preamplificatore** (prima del ricevitore), **booster** (dopo il trasmettitore) o **line amplifier** (lungo la tratta).

### Amplificatori Raman

Gli **amplificatori Raman** sfruttano lo **Scattering di Raman Stimolato (SRS)**: un urto anelastico tra fotoni di pompa e fononi del reticolo cristallino della fibra trasferisce energia dal pompa al segnale. Non richiedono fibra drogata: usano direttamente la fibra di trasmissione, con pompa a circa 100 nm sotto la lunghezza d'onda del segnale (es. 1450 nm per la banda C). I vantaggi rispetto all'EDFA sono una **figura di rumore inferiore** (migliore OSNR), amplificazione distribuita lungo la fibra e una banda molto più ampia (dalla banda O alla banda L estesa, 1300-1550 nm). Si usano prevalentemente in configurazione **contropropagante** per ridurre le interferenze con il segnale.

### SOA (Semiconductor Optical Amplifier)

Il **SOA** è strutturalmente simile a un laser, ma con trattamenti anti-riflesso sulle facce per evitare le riflessioni e permettere amplificazione a singolo passaggio. È una giunzione PN polarizzata direttamente: i fotoni incidenti inducono emissione stimolata senza cavità risonante. Guadagno fino a 30 dB, ma noise figure pessima (7-9 dB) a causa dell'emissione spontanea amplificata. Il vantaggio è la larghissima banda (40-100 nm), l'integrabilità su chip (PIC) e il funzionamento come switch ottico controllato via corrente.

### Confronto tra Amplificatori

|Parametro|EDFA|Raman|SOA|
|---|---|---|---|
|Guadagno|20-40 dB|10-20 dB|20-30 dB|
|Noise Figure|3-5 dB|< 3 dB (ottico)|7-9 dB|
|Banda|Banda C/L|O-L (ampia)|40-100 nm|
|Trasparenza bit rate|✅|✅|✅|
|Fibra speciale richiesta|✅ EDF|❌|❌ chip|
|Applicazione principale|Long-haul, backbone|Sottomarina, coerente|Switch, accesso, PIC|

## Switch Ottici

La commutazione nel dominio ottico evita costose conversioni O-E-O. Le principali tecnologie di switch sono:
- **MEMS (Micro-Electro-Mechanical Systems)**: specchietti mobili fabbricati su silicio che deflettono fisicamente il fascio ottico; perdite 1-3 dB, tempi di commutazione 1-10 ms, matrici fino a 1000×1000 porte
- **Thermo-Optic (TO)**: variazione dell'indice di rifrazione con la temperatura ($\Delta T \rightarrow \Delta n \rightarrow \Delta \varphi$); integrabili in PLC/PIC, tempi 1-10 ms
- **Electro-Optic (EO)**: effetto Pockels (campo elettrico -> $\Delta n$), tempi < 1 ns, perdite 3-6 dB
- **SOA-based**: commutazione on/off per iniezione di corrente; guadagno netto (0 dB di perdita), tempi ~1 ns, integrabili in PIC
## Nodi di Commutazione Ottica

### OXC (Optical Cross Connect)

L'**OXC** instrada segnali da una fibra di ingresso a una fibra di uscita senza conversione O-E-O, trasparente al bit rate e al protocollo. Le architetture variano da **Wavelength Blocking** (commuta l'intera fibra WDM, semplice ma non flessibile) a **Wavelength Selective** (demultiplexa, commuta per singola $\lambda$, ri-multiplexa). Le prestazioni tipiche sono: perdite di inserzione < 5 dB, crosstalk < −35 dB, capacità fino a $1000\times1000$ porte. L'OXC utilizza un **backplane ottico integrato**, senza cablaggio esterno, che riduce ingombro e consumi di 2/3 rispetto al ROADM.
### ROADM (Reconfigurable Optical Add-Drop Multiplexer)

Il **ROADM** è un nodo WDM riconfigurabile da remoto che può aggiungere (add), estrarre (drop) o lasciare passare selettivamente i canali ottici via software (GMPLS). È basato sul **WSS (Wavelength Selective Switch)**: la luce viene dispersa da un reticolo di diffrazione su un array **LCoS (Liquid Crystal on Silicon)**, i cui pixel controllati elettricamente riflettono selettivamente le lunghezze d'onda verso le porte di uscita desiderate. Il ROADM ha architettura a moduli separati connessi tramite patch cord esterne, il che rende la scalabilità più complessa rispetto all'OXC, ma rimane lo standard nelle reti di trasporto metropolitane e dorsali.
### Photonic Integrated Circuits (PIC)

I **PIC** integrano su un singolo chip funzioni ottiche multiple: laser, modulatori, fotodiodi, amplificatori e switch. La tecnologia **Silicon Photonics** (compatibile con i processi CMOS) ha abbattuto i costi di produzione; i **Co-Packaged Optics (CPO)** montano il chip fotonico direttamente accanto alla CPU per ridurre il consumo energetico. L'evoluzione verso PIC programmabili (simili agli FPGA elettronici) integra migliaia di componenti ottici, abilitando acceleratori hardware per AI, sensori LiDAR compatti e processori quantistici ottici.
## Formulario di Riferimento

| Formula                                            | Descrizione                            |
| -------------------------------------------------- | -------------------------------------- |
| $E = h\nu$                                         | Energia del fotone                     |
| $I_p = R \cdot P_{in}$                             | Fotocorrente (PIN/APD)                 |
| $\eta = \frac{h\nu_0}{e} \cdot R$                  | Efficienza quantica                    |
| $\tilde{i}(t) \propto 2 E_s E_{LO}^*$​             | Fotocorrente coerente (optical hybrid) |
| $\text{NF} = \text{SNR}_{in}​ / \text{SNR}_{out}$​ | Noise Figure amplificatore             |
