---
tags:
  - wireline
  - canale
  - cavo
---
## Struttura Fisica

Il **cavo coassiale** è un mezzo trasmissivo guidato composto da quattro strati concentrici che condividono lo stesso asse (_co-axial_):

1. **Anima (conduttore centrale)**: filo di rame puro (purezza ~99,9%), solitamente pieno per basse frequenze o trecciato per maggiore flessibilità. È il conduttore su cui viaggia il segnale.
2. **Dielettrico**: materiale isolante, tipicamente **polietilene espanso** (o solido), che separa l'anima dallo schermo esterno e determina l'impedenza caratteristica del cavo. Nei cavi più pregiati viene usato polietilene con struttura alveolare (aria + polietilene) per ridurre le perdite.
3. **Schermo (maglia)**: costituito da un **nastro metallico** (alluminio o rame, eventualmente in sandwich Al-PET-Al) e/o da una **treccia metallica** intrecciata ad alta copertura. Funge da secondo conduttore e da schermatura elettromagnetica. Il nastro è più efficace alle alte frequenze, la treccia alle basse.
4. **Guaina esterna**: rivestimento protettivo in **PVC** (per uso interno, flessibile), **Polietilene PE** (per uso esterno/interramento, impermeabile) o **LSZH** (_Low Smoke Zero Halogen_, per luoghi pubblici ad alto afflusso).

Il campo elettromagnetico si propaga nello spazio compreso tra l'anima e la maglia (nel dielettrico), rimanendo completamente confinato all'interno del cavo grazie alla simmetria cilindrica - caratteristica che conferisce al coassiale l'elevata immunità alle interferenze EMI.
### Impedenza Caratteristica

L'**impedenza caratteristica** $Z_0$ dipende dal rapporto tra il diametro esterno del dielettrico $D$ e il diametro dell'anima $d$, nonché dalla costante dielettrica $\varepsilon_r$​:
$$
Z_0 = \frac{138}{\sqrt{\varepsilon_r}} \log_{10}\!\left(\frac{D}{d}\right) \; [\Omega]
$$
Esistono due tipologie standard:

| Tipo                      | Applicazione principale                                                                                  |
| ------------------------- | -------------------------------------------------------------------------------------------------------- |
| **Coassiale 50 $\Omega$** | Trasmissioni digitali, radiofrequenza, sistemi radar, reti Ethernet, applicazioni militari e industriali |
| **Coassiale 75 $\Omega$** | Trasmissioni televisive (antenne TV, CATV, SAT), segnali video, reti HFC                                 |

La scelta di 50 $\Omega$ nasce dal compromesso ottimale tra la massima potenza trasferibile (~30 $\Omega$) e la minima attenuazione (~77 $\Omega$); il 75 $\Omega$ è ottimizzato per la minima attenuazione, ideale per le lunghe distribuzioni televisive.

## Codici RG e Classi di Schermatura

I cavi coassiali sono classificati secondo il codice **RG** (_Radio Guide_), un sistema di origine militare USA, e le **classi di schermatura** (norma EN 50117):

|Codice|Impedenza|Diametro|Applicazione tipica|
|---|---|---|---|
|**RG-6**|75 $\Omega$|~6,9 mm|TV via cavo, SAT, CATV (uso più diffuso)|
|**RG-11**|75 $\Omega$|~10,3 mm|Tratte lunghe, distribuzione CATV in edifici|
|**RG-59**|75 $\Omega$|~6,1 mm|Video analogico, CCTV, distanze brevi|
|**RG-58**|50 $\Omega$|~5 mm|Reti LAN Ethernet 10BASE2, radioamatori|
|**RG-8**|50 $\Omega$|~10,3 mm|Trasmettitori radio, applicazioni ad alta potenza|

Le **classi di schermatura** (A, B, C in ordine crescente di prestazioni) misurano l'**impedenza di trasferimento** del cavo secondo EN 50117: la classe A++ offre la massima attenuazione della schermatura, rendendo il cavo adatto agli ambienti con forte interferenza.
## Attenuazione ed Effetto Pelle

L'**attenuazione** di un cavo coassiale è la riduzione dell'ampiezza del segnale in funzione della lunghezza e della frequenza, espressa in **dB/100 m**. Il valore tipico è **superiore a 10 dB/100 m**, ma dipende fortemente dal tipo di cavo e dalla frequenza di esercizio.
### Effetto pelle (Skin Effect)

Il fenomeno fisico dominante alle alte frequenze è l'**effetto pelle**: la corrente alternata tende a distribuirsi in modo non uniforme nel conduttore, concentrandosi sulla superficie esterna piuttosto che nell'intero volume. La profondità di penetrazione (spessore di pelle) è:
$$
\delta = \sqrt{\frac{\rho}{\pi f \mu}}
$$
dove $\rho$ è la resistività del materiale, $f$ la frequenza e $\mu$ la permeabilità magnetica. All'aumentare di $f$, $\delta$ diminuisce: solo la superficie del conduttore trasporta corrente, la sezione utile si riduce e la resistenza aumenta, incrementando le perdite. Per questo motivo, a parità di corrente applicata, un cavo coassiale alle alte frequenze dissipa più potenza rispetto alle basse frequenze.

Nel cavo coassiale l'effetto pelle ha anche un ruolo **positivo**: la corrente nello schermo scorre anch'essa solo sulla superficie interna della maglia, isolando completamente il campo interno dal mondo esterno, il che garantisce l'elevata schermatura EMI caratteristica del coassiale.

## Banda Passante e Distanza

La **banda passante** di un cavo coassiale dipende dal tipo di costruzione e dall'applicazione:

|Tipo di cavo|Banda passante|Applicazione|
|---|---|---|
|Cavo TV standard|0 – 5 MHz (analogico)|Televisione analogica POTS-era|
|Cavo CATV moderno|fino a **1 GHz**|Distribuzione televisiva via cavo, HFC|
|Cavi coassiali ad alta prestazione|oltre **10 GHz**|Radar, microonde, strumentazione RF|
|Cavi semigidi/rigidi specializzati|**DC – 50 GHz+**|Applicazioni militari, test e misure|

Il cavo coassiale supera nettamente il doppino telefonico nella banda trasportabile: il rame telefonico arriva a 212 MHz (G.fast), mentre un buon cavo coassiale per CATV gestisce facilmente 1 GHz. Tuttavia, l'attenuazione rimane **crescente con la frequenza e la distanza**: raddoppiare la frequenza aumenta l'attenuazione di circa 3 dB su ogni 100 m, rendendo necessari ripetitori o amplificatori per tratte lunghe.
### Ripetitori vs Amplificatori

> Nelle reti coassiali non sono sufficienti semplici **amplificatori**, perché questi aumentano il livello del segnale ma amplificano anche il **rumore** e la **distorsione** accumulati. Servono invece **rigeneratori di segnale (repeater)**, che ricostruiscono il segnale digitale da zero, eliminando il rumore. Per i segnali analogici (televisione) si usano amplificatori ad alta linearità posti a intervalli regolari (tipicamente ogni 500–1000 m nelle reti CATV).

## Connettori per Cavi Coassiali

I principali tipi di connettori coassiali, scelti in funzione della frequenza e dell'applicazione:

|Connettore|Impedenza|Freq. max|Applicazione|
|---|---|---|---|
|**BNC**|50/75 $\Omega$|~11 GHz|Video, strumentazione, LAN 10BASE2|
|**F** (vite)|75 $\Omega$|~1 GHz|TV terrestre, SAT, modem CATV|
|**IEC 169-2**|75 $\Omega$|~1 GHz|Prese d'antenna TV da parete (standard EU)|
|**SMA**|50 $\Omega$|~18 GHz|Telecomunicazioni RF, antenne Wi-Fi/LTE|
|**N**|50 $\Omega$|~18 GHz|Stazioni radio base, sistemi wireless|
|**TNC**|50 $\Omega$|~11 GHz|Applicazioni militari e avioniche|

## Reti HFC (Hybrid Fibre-Coaxial)

L'applicazione più rilevante del cavo coassiale nelle moderne reti di telecomunicazione è l'architettura **HFC (Hybrid Fibre-Coaxial)**, utilizzata dagli operatori di televisione via cavo dagli anni '90.
### Schema di funzionamento

![[hfc_schema.png]]

1. Il segnale parte dalla **centrale (headend)** in formato ottico tramite fibra, coprendo distanze di 10–30 km senza amplificazione
2. Il **nodo ottico** converte il segnale da ottico a RF e lo immette sul segmento coassiale
3. Sul segmento coassiale (< 500 m tipicamente), il segnale viene distribuito a 500–2000 utenti tramite amplificatori in cascata

Questa architettura sfrutta la **fibra** per le tratte lunghe (immune all'attenuazione) e il **coassiale** per la distribuzione capillare nell'ultimo miglio, massimizzando il compromesso tra costo e prestazioni. Il protocollo utilizzato per la trasmissione dati su HFC è **DOCSIS** (Data Over Cable Service Interface Specification), che prevede le versioni 3.1 (fino a 10 Gbit/s) e 4.0 (fino a 10 Gbit/s simmetrici).

### Vantaggi

- **Alta immunità alle interferenze EMI**: il campo è completamente confinato tra anima e maglia, non si irradia né raccoglie disturbi esterni
- **Ampia banda passante**: superiore al doppino telefonico, adatta a segnali RF fino a 1 GHz (CATV) o oltre (applicazioni speciali)
- **Minore attenuazione rispetto al doppino** a parità di frequenza, grazie alla geometria coassiale
### Svantaggi

- **Rigidità meccanica**: difficile da posare in curva, richiede raggi di curvatura minimi per non deformare il dielettrico e variare l'impedenza
- **Costo più elevato** rispetto al doppino, sia per il materiale che per la posa
- **Attenuazione comunque crescente** con la frequenza (effetto pelle) e la distanza, necessità di amplificatori/ripetitori su tratte lunghe
- **Terminazione critica**: riflessi e disadattamento di impedenza generano onde stazionarie (VSWR elevato) che degradano le prestazioni se il cavo non è correttamente terminato