---
tags:
  - canale
  - wireless
  - rf
  - antenna
---

## Modalità di Propagazione RF

I segnali a radiofrequenza si propagano seguendo le equazioni di Maxwell, esattamente come la luce, ma a frequenze molto più basse e con lunghezze d'onda dell'ordine di centimetri o metri. Sulla crosta terrestre esistono **tre modalità di propagazione** principali:

- **Line of Sight (LoS)**: propagazione in linea diretta tra trasmettitore e ricevitore. Usata in tutti i sistemi moderni (Wi-Fi, 4G/5G, satellitare, ponti radio). È la tecnica fondamentale del corso.
- **Ground wave (onda di terra)**: il segnale segue la curvatura terrestre sfruttando l'interfaccia tra superficie e atmosfera. Usata per la radiodiffusione AM a basse frequenze (MF/HF). Marconi la credeva alla base del suo esperimento transatlantico del 1901, che in realtà sfruttò la propagazione ionosferica.
- **Propagazione ionosferica**: basata sulla riflessione da parte dello strato ionizzato dell'atmosfera, consente la propagazione oltre la portata ottica. Usata in passato per le HF; oggi quasi obsoleta per le telecomunicazioni civili.

## Path Loss

Il **path loss** (attenuazione di percorso) è il fenomeno fondamentale di qualsiasi sistema wireless: la potenza emessa dall'antenna si distribuisce su una superficie sferica di area $4\pi r^2$, quindi la densità di potenza decresce proporzionalmente a $1/r^2$. Per un'antenna isotropa, la densità di potenza a distanza $z$ vale:

$$
S_{TX} = \frac{P_{TX}}{4\pi z^2}​​
$$

La potenza ricevuta da un'antenna con area efficace $A_{RX}$​ è:

$$
P_{RX} = P_{TX} \cdot G_{TX} \cdot G_{RX} \cdot \frac{\lambda^2}{(4\pi z)^2}
$$

Questa è l'**equazione di Friis**, fondamentale per il calcolo del link budget. In forma logaritmica (dB), il **Free Space Path Loss** vale:

$$
\text{FSPL (dB)} = 10\log_{10}\left[\frac{\lambda^2}{(4\pi z)^2}\right] = 20\log_{10}(f) + 20\log_{10}(z) - 147.55
$$

Il **link budget** si calcola come somma di tutti i guadagni e tutte le perdite lungo il percorso:

$$
P_{RX} = P_{TX} + G_{TX} + G_{RX} - L_{FSPL} - L_P
$$

dove $L_P$​ include perdite aggiuntive (fading, ostacoli, attenuazione atmosferica, errori di puntamento). Il link budget serve a verificare se il segnale ricevuto supera la soglia minima di decodifica (sensitivity del ricevitore).

## Fading e Attenuazione Variabile

Oltre al path loss deterministico, esiste un'attenuazione **variabile nel tempo** chiamata **fading**, causata dalla variazione dei parametri fisici del canale radio (condizioni atmosferiche, posizione del ricevitore, ostacoli mobili). A differenza dei sistemi ottici dove la nebbia è il nemico principale, nei sistemi RF il fenomeno più critico è la **pioggia** (specialmente sopra i 10 GHz).

Il fading si distingue in due categorie in base all'origine:

- **Fading atmosferico**: variazioni lente dovute a pioggia, nebbia, assorbimento da vapor d'acqua e ossigeno
- **Multipath fading**: variazioni rapide dovute alla sovrapposizione di copie multiple del segnale - il fenomeno più critico nelle comunicazioni wireless moderne

## Multipath Fading

Il **multipath fading** si verifica quando il segnale RF raggiunge il ricevitore attraverso più percorsi distinti: uno diretto (LoS) e altri indiretti, generati da riflessioni su edifici, terreno e ostacoli. Il fenomeno è significativo solo quando le dimensioni degli ostacoli sono **confrontabili con la lunghezza d'onda**: a 2.4 GHz (Wi-Fi/Bluetooth) $\lambda \approx 12$ - comparabile con pareti e mobili.

Ogni percorso ha un proprio **ritardo** $\tau_k$​ e una propria **attenuazione** $\alpha_k$​. La risposta impulsiva del canale multipath è:

$$
h(t) = \sum_{k=0}^{N-1} \alpha_k \cdot \delta(t - \tau_k)
$$

La sovrapposizione al ricevitore può essere:

- **Interferenza costruttiva**: i segnali arrivano in fase -> il segnale si rafforza
- **Interferenza distruttiva**: i segnali arrivano in opposizione di fase -> il segnale si annulla (**deep fade**)

Il risultato è un **SNR variabile nel tempo** che può scendere a zero durante un deep fade, rendendo la comunicazione momentaneamente impossibile.

## Modelli di Multipath Fading

| Modello             | Condizione                                          | Distribuzione amplitudine | Uso tipico                           |
| ------------------- | --------------------------------------------------- | ------------------------- | ------------------------------------ |
| **Rayleigh fading** | Nessun percorso LoS dominante                       | Rayleigh                  | Ambienti urbani densi, NLoS          |
| **Rician fading**   | Presenza di un percorso LoS dominante + riflessioni | Rician (fattore K)        | Aree suburbane, ambienti semi-aperti |

## Fading Selettivo in Frequenza vs Flat Fading

Il multipath fading può influenzare lo spettro del segnale in due modi distinti:

- **Frequency Selective Fading**: la funzione di trasferimento del canale $H(f)$ varia significativamente all'interno della banda del segnale -> alcune frequenze sono molto attenuate, altre no. Si verifica quando la banda del segnale è **maggiore** della banda di coerenza del canale.
- **Flat Fading**: $H(f) \approx \text{costante}$ per tutte le frequenze del segnale -> l'intero segnale subisce la stessa attenuazione. Si verifica quando la banda del segnale è **minore** della banda di coerenza.

## Soluzione: OFDM + Prefisso Ciclico

L'**OFDM (Orthogonal Frequency Division Multiplexing)** combatte il multipath fading suddividendo la banda totale in **N** sottoportanti ortogonali a banda stretta. Ciascuna sottoportante sperimenta un flat fading indipendente, quindi un deep fade colpisce solo alcune sottoportanti lasciando le altre indenni. Il _prefisso ciclico_ (**CP**) è una copia della coda di ogni simbolo OFDM aggiunta all'inizio: se il CP è più lungo della risposta impulsiva del canale, elimina l'_Interferenza Inter-Simbolo_ (**ISI**) consentendo una semplice equalizzazione scalare complessa per ogni sottoportante. OFDM è la tecnica trasmissiva standard in Wi-Fi (802.11), 4G LTE e 5G NR.

## Spettro Elettromagnetico RF

### Classificazione IEEE e ITU

Le bande RF seguono due classificazioni parallele:

| Banda IEEE | Freq.        | $\lambda$         | Banda ITU   | Freq.     |
| ---------- | ------------ | ----------------- | ----------- | --------- |
| ELF        | 3–30 Hz      | 100.000–10.000 km | L-band      | 1–2 GHz   |
| VLF        | 3–30 kHz     | 100–10 km         | S-band      | 2–4 GHz   |
| MF         | 300–3000 kHz | 1000–100 m        | C-band      | 4–8 GHz   |
| HF         | 3–30 MHz     | 100–10 m          | X-band      | 8–12 GHz  |
| VHF        | 30–300 MHz   | 10–1 m            | Ku_uu​-band | 12–18 GHz |
| UHF        | 300–3000 MHz | 1 m–10 cm         | Ka_aa​-band | 26–40 GHz |
| SHF        | 3–30 GHz     | 10–1 cm           | -           | -         |
| EHF        | 30–300 GHz   | 10–1 mm           | -           | -         |

## Applicazioni per Banda (ITU)

- **L-band (1–2 GHz)**: sistemi satellitari, GNSS (GPS, Galileo)
- **C-band (4–8 GHz)**: satellitare, meno sensibile alla pioggia rispetto alle bande superiori
- **X-band (8–12 GHz)**: radar civili e militari, uso quasi esclusivamente per applicazioni di difesa
- **Ku​-band (12–18 GHz)**: TV satellitare, backhaul per reti cellulari
- **Ka​-band (26–40 GHz)**: satelliti ad alta capacità (Starlink, Viasat), 5G mmWave

## Frequenze ISM e Sistemi Consumer

Le bande **ISM** (_Industrial, Scientific and Medical_) sono libere da licenza e usate da Wi-Fi, Bluetooth e LoRaWAN:

| Sistema                     | Frequenza/bande                      |
| --------------------------- | ------------------------------------ |
| **Bluetooth** Classic + BLE | 2.4 GHz ISM                          |
| **Wi-Fi** (802.11)          | 2.4 GHz / 5 GHz / 6 GHz (Wi-Fi 6E/7) |
| **LoRaWAN**                 | EU: 868 MHz; USA: 915 MHz            |
| **4G/LTE**                  | 700–900 MHz (low), 1.5–2.6 GHz (mid) |
| **5G NR**                   | 700 MHz / 3.7 GHz / 26 GHz (mmWave)  |

## Propagazione 4G/5G in Ambiente Urbano

I segnali 4G e 5G si propagano principalmente in LoS tra antenna (**eNodeB** per 4G, **gNodeB** per 5G) e terminale; in ambienti urbani sfruttano riflessioni e diffrazione per aggirare gli ostacoli (NLoS). Le prestazioni dipendono fortemente dalla banda utilizzata:

- **Low-Band (700–900 MHz)**: bassa attenuazione, alta penetrazione negli edifici (indoor coverage), raggio fino a 10–30 km
- **Mid-Band (1.8–3.7 GHz)**: copertura intermedia, penetrazione moderata, portata di qualche km
- **High-Band / mmWave (24–40 GHz)**: altissima attenuazione - bloccato da fogliame, pioggia, persino dal corpo umano. Portata < 500 m; richiede Massive MIMO e beamforming per compensare le perdite

## Antenne

Un'antenna è un dispositivo che converte segnali elettrici in onde elettromagnetiche (trasmissione) e viceversa (ricezione). Le sue caratteristiche fondamentali sono il **diagramma di radiazione**, la **direttività** e il **guadagno**.

### Antenna Isotropica (Riferimento Teorico)

Un'antenna isotropica Irradia potenza **uniformemente** in tutte le direzioni con guadagno **0 dBi**. Non esiste fisicamente, ma è il riferimento rispetto a cui si misurano tutti i guadagni d'antenna in **dBi** (decibel rispetto all'isotropo).

### Antenne Omnidirezionali

Il **dipolo hertziano** ($\lambda/2$) e il **monopolo marconiano** ($\lambda/4$) sono le antenne omnidirezionali più comuni:

- **Dipolo hertziano** $\lambda/2$: due fili da $\lambda$/4 alimentati al centro. Direttività $D = 1.5\sin\theta$, guadagno = **1.76 dBi**. Irradia a toroide, praticamente uniforme nel piano orizzontale, con nulli nelle direzioni assiali.
- **Monopolo marconiano** $\lambda$/4: mezzo dipolo con piano di massa che funge da specchio. Il tetto di un'automobile fa da piano di massa, da qui l'antenna a stilo delle auto. Guadagno = **5.16 dBi**.

Nei dispositivi mobili moderni (smartphone, tablet) non c'è più l'antenna a stilo: sono usate **antenne a microstriscia (patch)** o **PIFA** (_Planar Inverted F Antenna_), ultra-piatte e integrabili nel PCB. Uno smartphone contiene tipicamente antenne separate per LTE (due, con polarizzazione orizzontale e verticale per MIMO), Wi-Fi, Bluetooth, GPS, NFC.

### Antenne Direzionali e Phased Array

Le **antenne direzionali** concentrano la potenza in una o poche direzioni principali (main lobe), riducendo le emissioni laterali (side lobes) e posteriori (back lobe). Per creare un fascio direzionale si usano due approcci principali:

- **Riflettore parabolico**: l'elemento radiante illumina la parabola che ri-collima l'energia in un fascio stretto. Usato nei ponti radio, antenne satellite, radar.
- **Phased array**: array di **N** antenne elementari (dipoli o patch) alimentate con **sfasamenti controllati** $\varphi_k$​. Variando i valori di fase si orienta il fascio elettronicamente senza muovere fisicamente l'antenna. È la base del **beamforming** e dei sistemi Massive MIMO per il 5G.

### Antenne per Sistemi 4G/5G

Le stazioni base [[Sistemi mobile#4G - LTE (Long Term Evolution)|4G]] (eNodeB) e [[Sistemi mobile#5G NR - New Radio|5G]] (gNodeB) usano principalmente **antenne a pannello settoriale** con le seguenti caratteristiche:

- Multibanda (più frequenze nella stessa struttura)
- Guadagno direzionale **15–18 dBi**
- Copertura angolare: settori di 60° / 90° / 120°
- Fascio verticale stretto con **downtilt** (inclinazione verso il basso, meccanica o elettronica) per limitare la copertura alla cella

## MIMO - Multiple Input Multiple Output

Il **MIMO** utilizza $N_T$​ antenne in trasmissione e $N_R$ antenne in ricezione, sfruttando la **diversità spaziale** come risorsa trasmissiva aggiuntiva. Non è una modulazione né una banda, ma una tecnica che trasforma il multipath da problema in risorsa. Il modello matematico è:

$$
\mathbf{Y} = \mathbf{H} \cdot \mathbf{X} + \mathbf{n}
$$

dove $\mathbf{H}$ è la matrice del canale di dimensioni $N_R \times N_T$​, ogni coefficiente $h_{ij}$​ descrive il percorso dall'antenna TX $i$ all'antenna RX $j$. Per stimare $\mathbf{H}$ si trasmettono segnali pilota noti, ottenendo il _Channel State Information_ (**CSI**).

![[Pasted image 20260616203105.png]]

### Modalità Operative

#### Beamforming (Precoding)

Il medesimo segnale viene trasmesso da tutte le antenne con sfasamenti calibrati, in modo che i fasci si sommino costruttivamente nella direzione del ricevitore. Risultato: massimizzazione della potenza ricevuta e riduzione del multipath fading. Richiede CSI al trasmettitore.

#### Spatial Multiplexing

Un flusso ad alta velocità viene diviso in $\min(N_T, N_R)$ flussi paralleli, ognuno trasmesso da un'antenna diversa nello **stesso canale in frequenza**. La capacità di canale scala come:

$$
C = \min(N_T, N_R) \cdot \log_2(1 + \text{SNR})
$$

Il ricevitore separa i flussi se arrivano da direzioni sufficientemente diverse. Non richiede necessariamente CSI al trasmettitore, ma le prestazioni migliorano se disponibile.

#### Diversity Coding

Lo stesso dato viene trasmesso su più antenne in modo ridondante. Non aumenta il throughput, ma riduce drasticamente la probabilità di errore: con ordine di diversità **d**:

$$
P_e \propto \text{SNR}^{-d}, \quad d_{\max} = N_T \cdot N_R
$$

### Multi-User MIMO (MU-MIMO)

Nel **SU-MIMO** (Single User), tutte le antenne servono un solo utente per volta. Nel **MU-MIMO** la stazione base suddivide il fascio spazialmente e serve **più utenti simultaneamente** nello stesso canale in frequenza, sfruttando la **Space Division Multiple Access (SDMA)**. Introdotto con Wi-Fi 5 (802.11ac), potenziato con Wi-Fi 6/6E/7. Richiede CSI al trasmettitore per il beamforming multiutente.

### Massive MIMO e 5G mmWave

Nel **Massive MIMO** le stazioni base 5G utilizzano centinaia o migliaia di antenne in array. Alle frequenze mmWave (24–40 GHz) la piccola lunghezza d'onda consente di integrare grandi array in volumi fisici compatti. Il beamforming ad alto guadagno compensa le elevate perdite atmosferiche e di percorso tipiche delle mmWave, rendendo praticabile la comunicazione a quelle frequenze.

## Propagazione Wi-Fi

Il Wi-Fi usa bande ISM a **2.4 GHz**, **5 GHz** e **6 GHz** (Wi-Fi 6E/7): all'aumentare della frequenza diminuisce la portata ma aumentano il bit rate e la robustezza alle interferenze. In ambienti indoor le riflessioni e l'assorbimento dei muri riducono il range effettivo: a 2.4 GHz con ostacoli la copertura scende a circa 30 m, contro i teorici 100 m in spazio libero. Le tecniche MIMO, MU-MIMO e OFDMA (introdotto con Wi-Fi 6) permettono di gestire efficacemente le interferenze multipath e di servire più dispositivi in parallelo.

> [!tip] Riepilogo delle tecniche contro il Multipath
> | Tecnica | Principio | Applicazione |
> | ------------------------ | -------------------------------------------------------------- | ---------------- |
> | **OFDM + CP** | Banda divisa in NNN sottoportanti narrow-band; CP elimina ISI | Wi-Fi, 4G, 5G |
> | **MIMO Diversity** | Ridondanza spaziale su più antenne | Wi-Fi, 4G, 5G |
> | **Beamforming** | Direzionalità riduce i percorsi di riflessione | 5G mmWave, radar |
> | **Spatial Multiplexing** | Sfrutta i percorsi multipli come canali paralleli indipendenti | Wi-Fi 6/7, 5G NR |
> | **Frequency Hopping** | Cambia frequenza per evitare null di fading | Bluetooth, GSM |
