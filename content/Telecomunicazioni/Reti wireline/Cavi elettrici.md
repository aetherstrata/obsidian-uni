---
tags:
  - wireline
  - canale
---
## Doppino telefonico
### Struttura Fisica

Il **doppino telefonico** è la linea di trasmissione più diffusa nelle reti di accesso fisse. È costituito da **due conduttori in rame** di diametro compreso tra **0,4 e 0,9 mm**, avvolti a elica l'uno attorno all'altro (processo di _binatura_) e ricoperti da una guaina isolante in PVC. L'impedenza caratteristica del doppino è di $100 \text{-} 120\ \Omega$. A riposo, sul doppino è presente una tensione continua di **48 V DC** fornita dalla centrale telefonica, durante lo squillo la tensione sale a **75–90 V AC** a 25 Hz.

>[!important] Twisted Pair
>La binatura serve a far sì che i **campi elettromagnetici esterni** agiscano in modo mediamente uguale su entrambi i conduttori della coppia. Questo effetto, combinato con la trasmissione differenziale (§2), permette di sopprimere efficacemente il rumore di modo comune. Più serratto è l'intreccio (più alto il numero di twists per metro), maggiore è l'immunità ai disturbi e alle interferenze elettromagnetiche.

### Trasmissione Differenziale

Il segnale non viene trasmesso su un singolo filo rispetto a una massa, ma come **differenza di potenziale** tra i due conduttori della coppia. Il ricevitore acquisisce solo la componente differenziale $V_{diff}=V^+-V^-$​, mentre qualsiasi disturbo che si sovrappone ugualmente a entrambi i fili (rumore di **modo comune**) viene cancellato automaticamente.

Il parametro che misura questa capacità di reiezione si chiama **CMRR** (*Common-Mode Rejection Ratio*): più alto è il CMRR, più efficace è la soppressione del rumore indotto dall'esterno (es. campi a 50 Hz dalla rete elettrica, interferenze radio). Questa tecnica è alla base anche di standard ad alta velocità come Ethernet, USB e HDMI.

### Diafonia (Crosstalk)

Quando più doppini sono raggruppati nello stesso cavo (es. un cavo da 25 o 50 coppie), i campi elettromagnetici generati dalla corrente alternata su ogni coppia inducono interferenza sulle coppie adiacenti. Questo fenomeno è detto **diafonia** (_crosstalk_) e rappresenta uno dei principali limiti prestazionali del doppino.

Si distinguono due tipi principali:

- **NEXT (Near-End Crosstalk)**: interferenza misurata alla stessa estremità da cui si trasmette. È il tipo più critico perché il segnale disturbarante è forte (non ha ancora percorso il cavo).
- **FEXT (Far-End Crosstalk)**: interferenza misurata all'estremità opposta rispetto al trasmettitore. È attenuata dalla lunghezza del cavo ed è in genere meno critica del NEXT.

Per ridurre la diafonia tra coppie diverse nello stesso cavo, ogni coppia è realizzata con una **frequenza di binatura diversa** dalle altre, in modo che gli accoppiamenti si compensino statisticamente. Nei sistemi moderni (VDSL2, G.fast) la tecnica del **Vectoring** misura e cancella attivamente la diafonia tra le linee.

### Parametri Elettrici

|Parametro|Valore tipico|
|---|---|
|Diametro conduttori|0,4 – 0,9 mm|
|Impedenza caratteristica|100 – 120 Ω|
|Tensione a riposo|48 V DC|
|Tensione di squillo|75 – 90 V AC, 25 Hz|
|Attenuazione (vs fibra)|~10 dB/100 m (vs 0,2 dB/km della fibra)|
|Banda utilizzabile massima|fino a ~212 MHz (G.fast)|

>[!tip] Rapporto con l'attenuazione
>L'**attenuazione** cresce con la frequenza e con la lunghezza del cavo, il che limita direttamente il bit rate raggiungibile: più lungo è il tratto in rame, minore è la banda sfruttabile.

### Prestazioni in Funzione della Distanza

Le prestazioni di ogni tecnologia DSL degradano rapidamente con la distanza, perché l'attenuazione del doppino cresce con la frequenza. A parità di tecnologia, raddoppiare la lunghezza può dimezzare il bit rate:

|Tecnologia|Velocità (DL / UL)|Distanza max|Banda usata|
|---|---|---|---|
|**POTS**|voce analogica|diversi km|3,1 kHz|
|**ISDN**|128 / 128 kbit/s|~5 km|~80 kHz|
|**ADSL2+**|24 / 3,5 Mbit/s|~4 km|2,2 MHz|
|**VDSL2 17a**|100 / 50 Mbit/s|~1 km|17,6 MHz|
|**G.fast**|1000 / 500 Mbit/s|~250 m|106–212 MHz|
### Connettore RJ11

La sigla **RJ11** (Registered Jack tipo 11) identifica il connettore standard usato per le linee telefoniche e i modem ADSL. Si tratta di un connettore modulare a **6 posizioni**, ma con soli **2 contatti attivi** (configurazione **6P2C**): i pin centrali **3 e 4** portano i due fili del doppino telefonico.

#### Confronto RJ11 vs RJ45

|Caratteristica|RJ11|RJ45|
|---|---|---|
|Posizioni|6|8|
|Contatti utilizzati|2 (o 4)|8|
|Velocità massima|~24 Mbit/s (ADSL)|fino a 10 Gbit/s (Ethernet)|
|Applicazione tipica|Telefonia, ADSL|Ethernet LAN|
|Standard di cablaggio|6P2C / 6P4C|T-568A / T-568B|

Il connettore RJ11 è fisicamente più piccolo dell'RJ45 e può essere inserito meccanicamente in una porta RJ45, ma non viceversa; la combinazione non è tuttavia raccomandata per uso operativo.
### Tipologie di Doppino

Quando più coppie sono raggruppate in un unico cavo (tipico dei cavi Ethernet), si distingue la schermatura:

- **UTP** (*Unshielded Twisted Pair*): nessuna schermatura aggiuntiva. Economico, flessibile, sufficiente per distanze brevi. Standard per Ethernet fino a 10 Gbit/s con Cat.6A.
- **FTP** (*Foiled Twisted Pair*): una schermatura in foglio di alluminio avvolge tutte le coppie insieme. Compromesso tra costo e immunità EMI.
- **STP** (*Shielded Twisted Pair*): ogni coppia è schermate individualmente, più una schermatura esterna comune. Massima protezione da interferenze, usato in ambienti industriali o ad alta densità di disturbi.
### Vantaggi e Svantaggi

**Vantaggi:**

- Infrastruttura **capillare già esistente** in tutta Italia: installata dalla SIP negli anni '60, nessun costo di scavo
- **Basso costo** del rame rispetto ad alternative come la fibra
- Compatibile con architetture **FTTC (Fiber-to-the-Cabinet)** e con la tecnica del **vectoring**
- Supporta successive generazioni di tecnologia (da POTS a G.fast) senza sostituire il cavo fisico

**Svantaggi:**

- Le **prestazioni peggiorano fortemente con la distanza**: attenuazione crescente con frequenza e lunghezza
- **Suscettibile a interferenze elettromagnetiche**: diafonia, rumori industriali, frequenze radio
- La banda fisica è intrinsecamente limitata rispetto alla fibra ottica (~212 MHz vs ~50 THz)
- Genera campi elettromagnetici che interferiscono con le coppie adiacenti (diafonia)