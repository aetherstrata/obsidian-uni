---
tags:
  - segnale
  - errori
  - digitale
---
I codici di *Reed-Solomon* sono **codici a blocchi lineari non binari**. A differenza di altri sistemi (come Hamming) che proteggono i singoli bit, il codice **RS** raggruppa i bit in simboli (spesso byte da 8 bit) e opera su di essi. 

L'idea fondamentale si basa sull'**interpolazione polinomiale** e sui Campi di Galois, noti come campi finiti $GF(q)$.
## Modello matematico

### Rappresentazione dei dati
I simboli del messaggio da inviare (ad esempio $k=239$ simboli) diventano i coefficienti di un polinomio associato al messaggio, $P(x)$, o alternativamente, punti su cui far passare un polinomio di grado $k−1$.
### Creazione della ridondanza
Si definisce matematicamente un *polinomio generatore* $g(x)$. Il trasmettitore effettua una divisione polinomiale dividendo $P(x)$ per $g(x)$. Il resto di questa divisione polinomiale diventerà il blocco di controllo, ovvero i simboli di parità.
### Il pacchetto trasmesso
I simboli di controllo vengono uniti ai $k$ simboli originari, producendo una codeword finale  lunga $n$ simboli. Il pacchetto inviato $RS(n,k)$ è costruito in modo tale che il suo intero polinomio associato risulti un multiplo esatto del polinomio generatore $g(x)$.
## Decodifica e Correzione degli Errori
Quando il ricevitore ottiene il pacchetto, la sequenza potrebbe essere stata alterata dal rumore (es. graffi su un CD o interferenze ottiche/radio).
### Rilevamento (Calcolo della Sindrome)
Il ricevitore divide il polinomio del pacchetto ricevuto per lo stesso polinomio generatore $g(x)$. Se il resto è zero, il pacchetto è intatto. Se il resto è diverso da zero, il valore del resto (chiamato *sindrome*) indica non solo che c'è un errore, ma contiene in forma crittografata la posizione e l'ampiezza dell'errore.
### Calcolo della soluzione
Per ricostruire il messaggio originario senza ritrasmissione, il processore in ricezione risolve dei sistemi di equazioni per estrarre dalla sindrome il *vettore d'errore*, lo sottrae al pacchetto corrotto e ripristinano il polinomio originale.
## Capacità di correzione
La formula della capacità di correzione di un codice di Reed-Solomon è la seguente:
$$
t=\frac{n−k}{2}
$$
Ciò significa che il codice può correggere un numero di simboli errati pari alla **metà del numero dei simboli di parità aggiunti**.

>[!example] Esempio
> Nel codice $RS(255,239)$ i simboli di parità sono $16$. La metà di $16$ è $8$. Il sistema correggerà ciecamente **qualsiasi** combinazione in cui vengano danneggiati fino a $8$ byte, indifferentemente da quanti bit siano alterati dentro quei byte.
## Burst Error
Il grande vantaggio dei codici di Reed-Solomon rispetto ad altri codici è la sua resistenza ai **Burst Error** (errori a raffica). 

Siccome **RS** non guarda i singoli bit ma lavora su simboli interi (come byte a 8 bit):

- Se un disturbo radio prolungato cambia 4 bit all'interno dello stesso byte, per il codice RS quello conta come **1 solo simbolo errato**, ed è facilmente risolvibile.
- RS impiega solo una porzione di ridondanza per sistemare l'intero byte. Se si usasse un normale codice FEC sui singoli bit, un treno di 8 bit consecutivi errati devasterebbe il sistema; per RS, è semplicemente la correzione di un singolo simbolo su un massimo di 8 consentiti.

## Applicazioni

Nonostante l'algoritmo sia stato inventato nel 1960, la sua applicazione pratica esplose negli anni successivi, man mano che l'elettronica cominciò a sostenere i pesanti calcoli matematici richiesti.

- **Storage Ottico:** È alla base dei CD, DVD e Blu-Ray. Grazie a questo codice, un CD graffiato (che perde consecutivamente migliaia di bit sulla traccia fisica) può essere letto perfettamente dal lettore CD senza saltare.
- **Esplorazioni Spaziali:** È stato impiegato dalla NASA per le sonde _Voyager_ e _Galileo_, che trasmettevano immagini dei pianeti con potenze bassissime, recuperando segnali dal rumore di fondo cosmico.
- **Codici QR:** I moderni QR Code integrano il codice Reed-Solomon al loro interno. È per questo che si possono sporcare, coprire o strappare via un intero angolo di un QR Code (fino al 30% della superficie) e la fotocamera dello smartphone riuscirà lo stesso a leggere il link completo.
- **Telecomunicazioni (Fibra Ottica e TV):** Il $RS(255,239)$ è stato per anni lo standard ITU per lo strato di trasporto ottico ([[Optical Transport Network|OTN]]) delle reti in fibra ed è tuttora integrato nei sistemi di TV digitale (DVB-T).