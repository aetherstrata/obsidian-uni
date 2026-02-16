---
tags:
  - tcp
  - reti
  - algoritmi
---
Il problema alla base del controllo di congestione in TCP è che, quando i router ricevono più pacchetti di quanti riescano a smaltire, formano code:
- se le code saturano si scartano pacchetti
- anche prima dello scarto si accumula _queueing delay_.

TCP, non avendo visibilità interna sui router, deduce la congestione osservando soprattutto le perdite (timeout o evidenza via ACK duplicati), assumendo che su Internet l'errore di trasmissione sia meno probabile della perdita da congestione.
## Due finestre

In TCP esistono due finestre distinte:
- la finestra di _controllo di flusso_ (**fwnd**, imposta dal ricevente tramite advertised window)
- la finestra di _controllo di congestione_ (**cwnd**, auto-limitazione del mittente). 

In pratica, la quantità di dati trasmessi che il mittente può tenere sulla rete è limitata da $min⁡(\text{fwnd},\text{cwnd})$, quindi anche se il ricevente è pronto a ricevere molto (_fwnd_ alta), TCP può scegliere un passo più prudente se sospetta congestione (_cwnd_ bassa). 

>[!example] Esempio
>- Se _fwnd_ = 8 KB e _cwnd_ = 4 KB → invio massimo 4 KB.​
>- Se _fwnd_ = 8 KB e _cwnd_ = 32 KB → invio massimo 8 KB

Con _cwnd_ costante, dopo ogni RTT il mittente può rinnovare l’invio fino a _cwnd_ segmenti, mantenendo un ritmo stabile dettato dall'ACK clock.

Questo è il punto chiave: _fwnd_ protegge il ricevente, _cwnd_ protegge la rete (e indirettamente anche l’applicazione).​

## Funzionamento
### Slow start

All'avvio della connessione (o dopo un reset aggressivo della _cwnd_), TCP parte con _cwnd_ piccola, tipicamente **~1 MSS** (ma può anche essere più alto in base all'implementazione), e la incrementa di 1 MSS per ogni nuovo ACK ricevuto prima del timeout.

Se un segment viene perso (va in timeout), allora _cwnd_ viene riportata al valore iniziale.

A parità di **RTT** (_round trip time_), questo produce una crescita circa esponenziale (raddoppio per RTT) finché non avvengono timeout o si raggiunge una soglia. La motivazione è stimare rapidamente la capacità effettiva del percorso (path) senza inondare subito la rete: la banda viene testata e si costruisce un clock di ACK che dà ritmo all'invio. 

Storicamente questo approccio nasce dal lavoro di Jacobson/Karels (1988) in risposta ai collassi da congestione osservati nelle reti dell’epoca.​
### Congestion avoidance

Per evitare che la crescita esponenziale porti rapidamente a overflow delle code, si introduce **ssthresh** (slow start threshold) e si passa a **congestion avoidance** quando cwnd supera ssthresh.

Il funzionamento dell'algoritmo combinato è il seguente:
1) Inizializza _cwnd_ a 1 MSS.​
2) A ogni ACK:
    - Se **cwnd ≤ ssthresh**: incremento di 1 MSS (fase slow start).​
    - Se **cwnd > ssthresh**: incremento di $\text{MSS}^2/\text{cwnd}$ per ACK (fase _congestion avoidance_).​
3) Se congestione rilevata con timeout:
    - ssthresh = metà del minimo tra _fwnd_ e _cwnd_ (almeno 2 MSS).​
    - cwnd = 1 MSS.​

>[!note] Crescita lineare
>In un **RTT**  arrivano circa $\text{cwnd}/\text{MSS}$ ACK, quindi l'aumento totale per **RTT** è di circa $\dfrac{\text{cwnd}}{\text{MSS}}\cdot\dfrac{\text{MSS}^2}{\text{cwnd}} = \text{MSS}$, quindi di $\sim 1\, \text{MSS}$.

>[!tip] TCP Tahoe vs Reno
>- **Tahoe** (1988): usa [[#slow start]] + [[#congestion avoidance]], ma non include [[#fast retransmit]]/[[#fast recovery]].​
>- **Reno**: aggiunge [[#fast retransmit e fast recovery]], producendo l’andamento “a dente di sega” (saw-teeth).​  
>    
>RFC 5681 formalizza/descrive il comportamento moderno “classico” del controllo congestione TCP (post-Tahoe/Reno).​
### Timeout

A ciascun segment è associato un _timeout di ritrasmissione_ (**RTO**) che viene fatto partire quando il segment viene spedito. Se il segment non è riscontrato entro il tempo in cui il timer di ritrasmissione raggiunge il timeout allora il segment viene ritrasmesso.

La scelta del _timeout di ritrasmissione_ è delicata: 
- **troppo basso**: genera ritrasmissioni spurie che peggiorano la congestione
- **troppo alto**: rallenta il recupero dalle perdite e riduce throughput

Trade-off:
- RTO troppo breve: ritrasmissioni inutili → più carico, possibile congestione indotta.​​
- RTO troppo lungo: recupero lento dalle perdite → bassa efficienza.​

Questo tipo di timer è più difficile da scegliere rispetto a un timer del data link perché:
- su link singolo, RTT/ACK-time è più prevedibile
- sulla rete, varianza e valore atteso del tempo di ACK cambiano nel tempo → RTO deve adattarsi.​

## Stima di Jacobson

Per misurare il ritardo del round trip si può usare l'algoritmo di Jacobson. Il suo funzionamento è il seguente:
1) Quando un segmento parte, si misura il tempo di arrivo dell'ACK $M$ (_RTT_ osservato).​
2) La stima RTT viene aggiornata con media esponenziale: $\text{RTT} = \alpha \text{RTT} + (1−\alpha) M$ (scelta che permette implementazioni efficienti con shift), tipicamente $\alpha = 7/8$.​
3) Timeout inizialmente modellato come $\beta\cdot\text{RTT}$ (con $\beta$ costante), ma si osserva che non basta.​ 
4) Si introduce una stima della variabilità $D$ (deviazione media): $D = \alpha D + (1−\alpha)|\text{RTT} − M|$
5) Si imposta il nuovo valore di timeout = $\text{RTT} + 4D$ (diverse implementazioni possono usare altre formule).

>[!tip] Valori delle variabili
>La variabile **$\alpha$** è un fattore di smoothing che determina quanto pesa il vecchio
valore di **RTT**.
>La variabile **$\beta$** non può avere un valore costante perché non risponderebbe bene ad aumenti di varianza. Jacobson lo ha reso proporzionale alla deviazione standard della distribuzione dei tempi di arrivo.

### Algoritmo di Karn

Se un segmento viene ritrasmesso, l'ACK che arriva dopo non dice se sta confermando la prima trasmissione o la ritrasmissione; usare quel campione per stimare RTT falserebbe SRTT e quindi l’RTO. 

Karn risolve il problema ignorando i campioni RTT dei segmenti ritrasmessi e introducendo un backoff esponenziale (raddoppio dell'RTO per quel segmento/episodio) finché non si torna a una situazione stabile.

Questo si sposa con le regole standard di gestione del timer (RTO) che prevedono backoff quando il timer scade, proprio per ridurre pressione sulla rete in condizioni di congestione o blackhole temporanei.
### Fast retransmit e fast recovery

Un segmento fuori sequenza induce ACK duplicati immediati (non ritardati) e che 3 duplicate ACK con lo stesso ACK number sono una forte evidenza di perdita (più che semplice riordino). 

Con **fast retransmit** il mittente ritrasmette subito il segmento mancante senza aspettare il timeout, riducendo drasticamente il tempo di recupero quando la rete sta ancora consegnando dati. 

Con **fast recovery** (TCP Reno) non si torna a _cwnd_ minima come dopo un timeout: si riduce (tipicamente a circa metà tramite **ssthresh**) ma si continua in [[#congestion avoidance]], perché l’arrivo di duplicate ACK suggerisce che dei dati stanno ancora fluendo e quindi non serve ripartire da slow start (che sarebbe troppo drastico). 

TCP Tahoe, invece, storicamente implementava slow start + congestion avoidance ma non la coppia fast retransmit/fast recovery, quindi reagiva in modo più conservativo alle perdite.