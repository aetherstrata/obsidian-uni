---
tags:
  - wifi
  - pacchetti
  - data-link
---

Il livello **MAC** del Wi‑Fi genera i propri **frame**, in modo analogo a quanto fa Ethernet, ma con scopi diversi. In particolare, il livello MAC deve:

- gestire l’**accesso al mezzo trasmissivo**;
- definire il **formato dei frame**;
- gestire i meccanismi di **sicurezza**;
- supportare i meccanismi di _risparmio energetico_ (**power saving**).

Nei sistemi Wi‑Fi moderni, la gestione del consumo energetico è molto importante, quindi il MAC ha anche il compito di ridurre l’uso di potenza quando possibile.

## Accesso al canale

Il Wi‑Fi usa un meccanismo di accesso multiplo al canale basato su _Carrier Sense Multiple Access_, più precisamente **CSMA/CA** (_collision avoidance_), al contrario di Ethernet classica che usa **CSMA/CD** (_collision detection_). L’idea di base è questa:

1. prima di trasmettere, una stazione deve **ascoltare il mezzo**;
2. solo se il canale risulta libero può iniziare la procedura di trasmissione;
3. il livello MAC prende i pacchetti dei livelli superiori, ad esempio i pacchetti IP, e li inserisce in un **frame MAC**;
4. il frame viene poi passato al [[Livello fisico|livello fisico]] per la trasmissione.

Il MAC non usa gli indirizzi IP per la trasmissione sul mezzo radio, ma gli **indirizzi MAC** dei dispositivi coinvolti, ad esempio l’indirizzo dell’**Access Point** a cui il frame deve essere inviato.

### Protocolli di Accesso al Mezzo (CSMA/CA)

Il Wi-Fi utilizza il protocollo **CSMA/CA** (Carrier Sense Multiple Access with Collision Avoidance) per evitare le collisioni, poiché non può rilevarle come fa l'Ethernet con il Collision Detection. L'accesso al canale è regolato da due meccanismi principali:

- **DCF** (_Distributed Coordination Function_): Metodo decentralizzato standard, basato sui tempi di attesa e regole di backoff.
- **PCF** (_Point Coordination Function_): Metodo centralizzato (opzionale) in cui l'Access Point (AP) assegna priorità a specifici flussi, creando un periodo senza contesa (Contention Free Period).

### Tempi di Attesa e Priorità (IFS)

L'accesso al mezzo e le priorità dei pacchetti sono gestiti tramite gli **IFS** (_InterFrame Space_). Più l'IFS è breve, maggiore è la priorità del pacchetto.

- **SIFS** (_Short IFS_): Tempo brevissimo, massima priorità; usato per frame di controllo essenziali come ACK (Acknowledgement), RTS (Request to Send) e CTS (Clear to Send).
- **PIFS** (_PCF IFS_): Tempo intermedio; usato per il traffico in tempo reale (video, voce) sotto la gestione del protocollo PCF.
- **DIFS** (_DCF IFS_): Tempo più lungo, priorità base; usato per il traffico dati standard (es. traffico IP) sotto la gestione del protocollo DCF.

### Regole di Accesso e Meccanismo di Backoff

Per trasmettere un pacchetto dati standard, una stazione deve seguire una precisa sequenza per evitare collisioni simultanee:

1. La stazione verifica se il canale è libero (Carrier Sense).
2. Se occupato, attende che si liberi.
3. Quando il canale si libera, attende obbligatoriamente il tempo **DIFS**.
4. Se il canale è ancora libero dopo il DIFS, la stazione avvia un timer di **Backoff** casuale scelto all'interno di una Contention Window.
5. Il timer scala solo mentre il canale è libero; se qualcun altro inizia a trasmettere, il timer si congela e riprenderà quando il mezzo tornerà libero.
6. Quando il backoff arriva a zero, la stazione trasmette i dati.
7. Se la trasmissione va a buon fine, il destinatario attende un tempo **SIFS** (molto breve) e risponde con un **ACK**.

### Il Problema del Nodo Nascosto e RTS/CTS

Il problema del "nodo nascosto" si verifica quando due stazioni non si possono rilevare tra loro perché troppo lontane, ma entrambe vedono lo stesso AP, finendo per trasmettere contemporaneamente e causando collisioni. Per risolvere questo problema, si utilizza il meccanismo di handshake RTS/CTS.

- La stazione mittente invia un breve frame **RTS** (_Request to Send_).
- L'AP risponde dopo un SIFS con un frame **CTS** (_Clear to Send_) in broadcast.
- Il CTS notifica alla stazione richiedente che può trasmettere e avvisa tutte le altre stazioni (anche quelle nascoste al mittente originale) di rimanere in silenzio.
- La stazione invia i Dati e l'AP conclude inviando un ACK.

### Network Allocation Vector (NAV)

Il **NAV** è un meccanismo di **Virtual Carrier Sense** che permette alle stazioni di risparmiare energia evitando di ascoltare fisicamente il canale in continuazione.

- I frame _RTS_ e _CTS_ contengono la durata prevista dell'intera transazione (dati + ACK).
- Le stazioni in ascolto leggono questo valore e aggiornano il proprio **NAV**, un timer interno.
- Finché il **NAV** non scade, le stazioni considerano il canale virtualmente occupato e non provano a trasmettere, disattivando temporaneamente i circuiti di ascolto.

## Tipi di frame MAC

Nel Wi‑Fi i frame MAC si dividono in **tre categorie principali**:

- Frame di _dati_ (**Data Transfer**), usati per trasportare le informazioni utente.
- Frame di _controllo_ (**Control**), usati per gestire l’accesso al canale.
- Frame di _gestione_ (**management**), usati per autenticazione, associazione, beacon, probe e altre procedure di gestione della rete.

### Frame di controllo

I frame di controllo servono perché nel Wi‑Fi non è possibile rilevare una collisione come in Ethernet mentre si sta trasmettendo. Per questo motivo:

- dopo aver inviato un frame, il mittente si aspetta un **ACK** (**Acknowledgement**);
- se l’ACK non arriva, il frame viene ritrasmesso;
- dopo un certo numero di tentativi falliti, la trasmissione viene interrotta.

I principali frame di controllo sono:

- **ACK**, per confermare che un frame è stato ricevuto correttamente;
- **RTS** (_Request to Send_), per chiedere il permesso di trasmettere;
- **CTS** (_Clear to Send_), inviato dall’Access Point per autorizzare la trasmissione.

L’uso di RTS e CTS non è sempre obbligatorio, perché introduce overhead e quindi consuma banda.

### Frame di gestione

I frame di management servono per la gestione della rete Wi‑Fi. Tra questi ci sono:

- **autenticazione**;
- **associazione** e **disassociazione**;
- gestione delle **chiavi di sicurezza**;
- **beacon**, cioè i frame periodici trasmessi dall’Access Point;
- **probe request** e **probe response**.

## Struttura generale del frame MAC

Il frame MAC del Wi‑Fi contiene diversi campi. I più importanti sono:

- **Frame Control**, che descrive il tipo di frame e varie informazioni di controllo;
- **Duration/Connection ID**, che indica per quanto tempo il mezzo resterà occupato;
- fino a **quattro campi indirizzo**;
- **Sequence Control**, per la gestione dell’ordine dei frame;
- eventuali campi aggiuntivi, come **HT Control**;
- **Frame Body**, cioè i dati veri e propri;
- **FCS** (**Frame Check Sequence**), cioè il controllo finale con **CRC a 32 bit**.

### Frame Control

Nel campo **Frame Control** troviamo informazioni come:

- la **versione del protocollo**;
- il **tipo di frame**: management, control oppure data;
- il **sottotipo**, ad esempio ACK, RTS, CTS, beacon, association request, ecc.;
- i bit **To DS** e **From DS**;
- indicazioni su crittografia, presenza di altri dati e altri flag di controllo.

### Duration/ID e NAV

Il campo **Duration/ID** è importante perché comunica per quanto tempo il mezzo sarà occupato. Le altre stazioni leggono questa informazione e aggiornano un contatore interno chiamato **NAV** (**Network Allocation Vector**).

Il **NAV** serve a far capire alle stazioni che il canale deve essere considerato occupato per quel certo intervallo di tempo, anche se in quel momento non stanno fisicamente ascoltando una trasmissione utile per loro.

### I quattro indirizzi

Una delle particolarità del frame MAC del Wi‑Fi è che può contenere fino a **quattro indirizzi**.

In Ethernet di solito bastano:

- **Destination Address (DA)**;
- **Source Address (SA)**.

Nel Wi‑Fi, invece, la situazione è più complessa perché i frame possono passare attraverso un **Access Point** e, in alcuni casi, anche tra **due Access Point**.

#### Caso diretto: stazione verso Access Point

Se un client deve trasmettere un frame verso la rete tramite un Access Point, nel frame compaiono tipicamente tre indirizzi:

- **Address 1**: il destinatario radio immediato, cioè l’Access Point (**Receiver Address / BSSID**).
- **Address 2**: la stazione che sta trasmettendo, cioè il mittente radio (**Transmitter Address / Source Address**).
- **Address 3**: la destinazione finale del frame nella rete.

In pratica:

- il frame viene mandato via radio all’Access Point;
- l’Access Point poi lo inoltra verso la destinazione finale.

#### Caso opposto: Access Point verso stazione

Se invece il frame arriva dalla rete verso il client tramite l’Access Point, i ruoli degli indirizzi cambiano:

- un indirizzo identifica il destinatario radio immediato, cioè la stazione;
- un altro identifica l’Access Point che sta trasmettendo via radio;
- un altro ancora rappresenta la sorgente finale del traffico.

Per questo motivo, nel Wi‑Fi i campi di indirizzamento sono più flessibili rispetto a Ethernet.

#### Il quarto indirizzo

Il **quarto indirizzo** serve in scenari particolari, ad esempio quando due Access Point sono collegati **via wireless** tra loro, cioè in modalità **wireless bridge** o **WDS**.

In questo caso bisogna distinguere:

- il mittente radio;
- il destinatario radio;
- la sorgente finale;
- la destinazione finale.

Per questo possono servire tutti e quattro gli indirizzi.

### Significato di To DS e From DS

I bit **To DS** e **From DS** servono a indicare la direzione del frame rispetto al **Distribution System (DS)**:

- **To DS = 1**: il frame sta andando verso il Distribution System.
- **From DS = 1**: il frame proviene dal Distribution System.

I casi tipici sono:

- **To DS = 1, From DS = 0**: stazione che invia verso l’Access Point.
- **To DS = 0, From DS = 1**: Access Point che invia verso la stazione.
- **To DS = 1, From DS = 1**: collegamento tra Access Point in modalità bridge wireless, quindi può servire anche il quarto indirizzo.
- **To DS = 0, From DS = 0**: comunicazioni che non coinvolgono il DS, ad esempio alcuni frame di management o reti ad hoc.

## Accesso alla rete

### Procedure di Scanning

Per potersi connettere a una rete wireless, una stazione deve prima scoprire quali Access Point sono disponibili nelle vicinanze. Questo processo si chiama scanning e può avvenire in due modalità distinte, a seconda delle impostazioni del sistema operativo e delle priorità energetiche.

#### Scanning passivo

Lo scanning passivo avviene quando il client si sintonizza sui vari canali radio e si mette semplicemente in ascolto.

L'Access Point trasmette periodicamente (di solito ogni 100 millisecondi) frame di _management_ chiamati Beacon, contenenti il SSID, le velocità supportate e i parametri di rete.

Il vantaggio principale dello scanning passivo è il notevole risparmio energetico, che lo rende ideale per i dispositivi alimentati a batteria.

Lo svantaggio dello scanning passivo è la lentezza, poiché il client deve sostare su ogni canale per un tempo sufficiente ad intercettare un eventuale Beacon.

#### Scanning attivo

Lo scanning attivo prevede invece che il client prenda l'iniziativa, inviando su tutti i canali dei frame di management chiamati Probe Request.

Gli Access Point in ascolto che soddisfano i criteri della richiesta rispondono direttamente alla stazione con un frame di Probe Response.

Questo metodo è molto più veloce e permette connessioni rapide, ma comporta un maggiore consumo di energia per la trasmissione attiva.

Lo scanning attivo espone maggiormente la privacy del dispositivo, poiché spesso i client trasmettono in chiaro i nomi delle reti a cui si sono connessi in passato per cercarle nell'etere.

### Autenticazione e Sicurezza

Dopo aver individuato la rete, la stazione deve completare il processo di autenticazione e la successiva associazione all'Access Point. Nel tempo, lo standard IEEE 802.11 ha introdotto diverse tecnologie per garantire l'accesso sicuro al mezzo condiviso.

#### Rete aperta

L'autenticazione **Open System** è il metodo basilare dove non avviene alcuna verifica crittografica delle credenziali a livello MAC.

In un sistema aperto la stazione invia la richiesta e l'Access Point l'accetta sempre, delegando la reale sicurezza ai livelli superiori della rete (come un Captive Portal o l'uso di una VPN).

#### Wired Equivalent Privacy

Il protocollo **WEP** (_Wired Equivalent Privacy_) è lo standard di sicurezza originale, oggi deprecato e totalmente insicuro a causa di gravi vulnerabilità nella gestione dei vettori di inizializzazione (IV) dell'algoritmo RC4.

Con il WEP, un attaccante in ascolto passivo può decifrare la chiave di rete semplicemente catturando un numero sufficiente di pacchetti dati perché rimane la stessa durante tutta la sessione.

#### Wi-Fi Protected Access

Il protocollo **WPA** (_Wi-Fi Protected Access_) è stato introdotto come soluzione ponte per sostituire il WEP utilizzando lo stesso hardware, implementando il **TKIP** per variare dinamicamente le chiavi di cifratura.

Il **WPA2** rappresenta l'attuale standard di base per la sicurezza wireless, introducendo l'uso obbligatorio dell'algoritmo di crittografia forte AES-CCMP.

Il **WPA3** è lo standard di ultima generazione, progettato per bloccare gli attacchi a dizionario offline grazie al protocollo di handshake **SAE** (_Simultaneous Authentication of Equals_) e per garantire la Forward Secrecy.

Le implementazioni WPA si dividono in Personal, basate su una password condivisa (PSK), ed Enterprise, che richiedono un server RADIUS per autenticare individualmente ogni utente della rete.

## Power Management nel Wi-Fi

Poiché l'AP è generalmente collegato alla rete elettrica mentre i dispositivi client vanno a batteria, il Wi-Fi prevede funzioni di risparmio energetico (Power Save).

- Una stazione invia un frame all'AP impostando un flag specifico nel livello MAC per avvisare che sta entrando in **Sleep Mode**.
- Da quel momento, l'AP non invia più pacchetti a quella stazione, ma li memorizza nei propri buffer interni.
- Quando la stazione si "sveglia" periodicamente, interroga l'AP per verificare se ci sono dati in sospeso e, in caso affermativo, richiede la trasmissione dei pacchetti bufferizzati.
