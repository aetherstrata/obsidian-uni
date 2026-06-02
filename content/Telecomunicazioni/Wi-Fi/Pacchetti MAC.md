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

1) prima di trasmettere, una stazione deve **ascoltare il mezzo**;
2) solo se il canale risulta libero può iniziare la procedura di trasmissione;
3) il livello MAC prende i pacchetti dei livelli superiori, ad esempio i pacchetti IP, e li inserisce in un **frame MAC**;
4) il frame viene poi passato al [[Telecomunicazioni/Wi-Fi/Pacchetti fisici|livello fisico]] per la trasmissione.

Il MAC non usa gli indirizzi IP per la trasmissione sul mezzo radio, ma gli **indirizzi MAC** dei dispositivi coinvolti, ad esempio l’indirizzo dell’**Access Point** a cui il frame deve essere inviato.

## Tipi di frame MAC

Nel Wi‑Fi i frame MAC si dividono in **tre categorie principali**:

- Frame di _dati_ (**Data Transfer**), usati per trasportare le informazioni utente.
- Frame di _controllo_ (**, usati per gestire l’accesso al canale.
    
- **Frame di management**, usati per autenticazione, associazione, beacon, probe e altre procedure di gestione della rete.
    

## Frame di controllo

I frame di controllo servono perché nel Wi‑Fi non è possibile rilevare una collisione come in Ethernet mentre si sta trasmettendo. Per questo motivo:

- dopo aver inviato un frame, il mittente si aspetta un **ACK** (**Acknowledgement**);
    
- se l’ACK non arriva, il frame viene ritrasmesso;
    
- dopo un certo numero di tentativi, la trasmissione viene interrotta.
    

I principali frame di controllo sono:

- **ACK**, per confermare che un frame è stato ricevuto correttamente;
    
- **RTS** (**Request to Send**), per chiedere il permesso di trasmettere;
    
- **CTS** (**Clear to Send**), inviato dall’Access Point per autorizzare la trasmissione.
    

## Problema del nodo nascosto

Nel Wi‑Fi può verificarsi il problema del **nodo nascosto** (**hidden node**):

- un utente lontano dall’Access Point può stare trasmettendo;
    
- io non lo sento, perché il suo segnale è troppo debole nel mio punto;
    
- a me il canale sembra libero, quindi inizio a trasmettere;
    
- l’Access Point riceve due trasmissioni contemporanee e può verificarsi una collisione.
    

Per ridurre questo problema si usa il meccanismo **RTS/CTS**:

- la stazione invia un **RTS** all’Access Point;
    
- l’Access Point risponde con un **CTS**;
    
- tutte le altre stazioni che ricevono il CTS capiscono che il canale sarà occupato e aspettano;
    
- la stazione autorizzata trasmette i dati;
    
- al termine riceve l’**ACK**.
    

L’uso di RTS e CTS non è sempre obbligatorio, perché introduce overhead e quindi consuma banda.

## Frame di management

I frame di management servono per la gestione della rete Wi‑Fi. Tra questi ci sono:

- **autenticazione**;
    
- **associazione** e **disassociazione**;
    
- gestione delle **chiavi di sicurezza**;
    
- **beacon**, cioè i frame periodici trasmessi dall’Access Point;
    
- **probe request** e **probe response**.
    

Quindi, in sintesi, i frame MAC del Wi‑Fi sono di tre tipi:

- **management**;
    
- **control**;
    
- **data**.
    

## Struttura generale del frame MAC

Il frame MAC del Wi‑Fi contiene diversi campi. I più importanti sono:

- **Frame Control**, che descrive il tipo di frame e varie informazioni di controllo;
    
- **Duration/ID**, che indica per quanto tempo il mezzo resterà occupato;
    
- fino a **quattro campi indirizzo**;
    
- **Sequence Control**, per la gestione dell’ordine dei frame;
    
- eventuali campi aggiuntivi, come **HT Control**;
    
- **Frame Body**, cioè i dati veri e propri;
    
- **FCS** (**Frame Check Sequence**), cioè il controllo finale con **CRC a 32 bit**.
    

## Frame Control

Nel campo **Frame Control** troviamo informazioni come:

- la **versione del protocollo**;
    
- il **tipo di frame**: management, control oppure data;
    
- il **sottotipo**, ad esempio ACK, RTS, CTS, beacon, association request, ecc.;
    
- i bit **To DS** e **From DS**;
    
- indicazioni su crittografia, presenza di altri dati e altri flag di controllo.
    

## Duration/ID e NAV

Il campo **Duration/ID** è importante perché comunica per quanto tempo il mezzo sarà occupato. Le altre stazioni leggono questa informazione e aggiornano un contatore interno chiamato **NAV** (**Network Allocation Vector**).

Il **NAV** serve a far capire alle stazioni che il canale deve essere considerato occupato per quel certo intervallo di tempo, anche se in quel momento non stanno fisicamente ascoltando una trasmissione utile per loro.

## I quattro indirizzi

Una delle particolarità del frame MAC del Wi‑Fi è che può contenere fino a **quattro indirizzi**. In Ethernet di solito bastano:

- **Destination Address (DA)**;
    
- **Source Address (SA)**.
    

Nel Wi‑Fi, invece, la situazione è più complessa perché i frame possono passare attraverso un **Access Point** e, in alcuni casi, anche tra **due Access Point**.

## Caso normale: stazione verso Access Point

Se un client deve trasmettere un frame verso la rete tramite un Access Point, nel frame compaiono tipicamente tre indirizzi:

- **Address 1**: il destinatario radio immediato, cioè l’Access Point (**Receiver Address / BSSID**).
    
- **Address 2**: la stazione che sta trasmettendo, cioè il mittente radio (**Transmitter Address / Source Address**).
    
- **Address 3**: la destinazione finale del frame nella rete.
    

In pratica:

- il frame viene mandato via radio all’Access Point;
    
- l’Access Point poi lo inoltra verso la destinazione finale.
    

## Caso opposto: Access Point verso stazione

Se invece il frame arriva dalla rete verso il client tramite l’Access Point, i ruoli degli indirizzi cambiano:

- un indirizzo identifica il destinatario radio immediato, cioè la stazione;
    
- un altro identifica l’Access Point che sta trasmettendo via radio;
    
- un altro ancora rappresenta la sorgente finale del traffico.
    

Per questo motivo, nel Wi‑Fi i campi di indirizzamento sono più flessibili rispetto a Ethernet.

## Perché esiste il quarto indirizzo

Il **quarto indirizzo** serve in scenari particolari, ad esempio quando due Access Point sono collegati **via wireless** tra loro, cioè in modalità **wireless bridge** o **WDS**.

In questo caso bisogna distinguere:

- il mittente radio;
    
- il destinatario radio;
    
- la sorgente finale;
    
- la destinazione finale.
    

Per questo possono servire tutti e quattro gli indirizzi.

## Significato di To DS e From DS

I bit **To DS** e **From DS** servono a indicare la direzione del frame rispetto al **Distribution System (DS)**:

- **To DS = 1**: il frame sta andando verso il Distribution System.
    
- **From DS = 1**: il frame proviene dal Distribution System.
    

I casi tipici sono:

- **To DS = 1, From DS = 0**: stazione che invia verso l’Access Point.
    
- **To DS = 0, From DS = 1**: Access Point che invia verso la stazione.
    
- **To DS = 1, From DS = 1**: collegamento tra Access Point in modalità bridge wireless, quindi può servire anche il quarto indirizzo.
    
- **To DS = 0, From DS = 0**: comunicazioni che non coinvolgono il DS, ad esempio alcuni frame di management o reti ad hoc.
    

## Osservazione finale

La struttura del frame MAC Wi‑Fi è più complessa di quella Ethernet perché deve gestire:

- l’accesso condiviso al mezzo radio;
    
- la presenza dell’Access Point come nodo intermedio;
    
- i meccanismi di controllo come ACK, RTS e CTS;
    
- la mobilità e il risparmio energetico;
    
- scenari speciali come il collegamento wireless tra Access Point.

## Power Management nel Wi-Fi

Poiché l'AP è generalmente collegato alla rete elettrica mentre i dispositivi client vanno a batteria, il Wi-Fi prevede funzioni di risparmio energetico (Power Save).

- Una stazione invia un frame all'AP impostando un flag specifico nel livello MAC per avvisare che sta entrando in **Sleep Mode**.
- Da quel momento, l'AP non invia più pacchetti a quella stazione, ma li memorizza nei propri buffer interni.
- Quando la stazione si "sveglia" periodicamente, interroga l'AP per verificare se ci sono dati in sospeso e, in caso affermativo, richiede la trasmissione dei pacchetti bufferizzati.