---
tags:
  - tcp
  - reti
  - algoritmi
---
L'efficienza di una connessione TCP è il rapporto tra i byte utili che l’applicazione vuole trasferire (_payload TCP_) e i byte complessivamente trasmessi includendo gli header (TCP+IP).

- **Dati utili** -> payload TCP
- **Dati totali** -> payload TCP + header TCP + header IP
- **Overhead** -> header TCP + header IP

 Inviando pochi byte per volta, l'overhead percentuale cresce molto perché ogni invio porta con sé header e spesso anche ACK separati.

>[!note] Struttura Header TCP
>![[Pasted image 20260213201254.png]]

## Segmenti

TCP invia dati come flusso ma li trasporta in segmenti: durante il _three-way handshake_ viene negoziata la **MSS** (_Maximum Segment Size_), cioè la massima quantità di payload TCP che conviene mettere in un singolo segmento (in pratica dipende dalla **MTU** del path e dalle opzioni). 

Questo è importante perché molte tecniche di efficienza accumulano piccoli write dell’applicazione in un buffer finché non si può spedire un segmento più pieno (fino al _MSS_).
### Window

Il campo _window_ nell'header TCP indica quanti byte, a partire dall'ACK corrente, il ricevente è disposto ad accettare. Rimane il limite storico del campo window a 16 bit quindi il massimo consentito è di 65535 byte.

>[!important] Window -> Controllo di flusso, non di congestione

Dal punto di vista prestazionale, ogni aggiornamento di finestra (window update) può diventare traffico extra: per questo TCP cerca di ridurre anche il numero di notifiche (ACK e variazioni di finestra), non solo i pacchetti dati.​

>[!example] Esempio Telnet/SSH
>Nei servizi interattivi (_Telnet_ o _SSH_ su terminale remoto), l’applicazione genera spesso input minuscolo e frequente (es. 1 carattere per tasto premuto), quindi TCP rischia di spedire molti segmenti piccoli. 
>
>Nel caso peggiore, 1 byte applicativo può generare 3 segmenti TCP (data, ACK, window update) e quindi molti byte di overhead rispetto al payload: questo è esattamente il “_small-packet problem_” discusso anche in Nagle/RFC 896.
>
>Sequenza di eventi:
>1) l’utente preme un tasto
>2) TCP prepara un segment, IP lo incapsula in un datagram (pacchetto IP) e lo invia
>	- pacchetto 1: 41 byte (20 IP header + 20 TCP header + 1 telnet)
>3) lo strato TCP ricevente riscontra il segment
>	- pacchetto 2: 40 byte (20 IP header + 20 TCP header)
>4) quando il server telnet legge il carattere, allora TCP notifica una variazione della finestra di un byte
>	- pacchetto 3: 40 byte (20 IP header + 20 TCP header)
>
>Se il server invia un eco al client allora saranno scambiati altri 121 byte in senso opposto.

## Strategie

Le strategie si distinguono due famiglie:​
- **Ridurre il numero di pacchetti inviati**: cumulare piccoli segmenti in segmenti più grandi, quindi ridurre la quantità di notifiche (ACK e window change).​
- **Ridurre il numero di pacchetti perduti**: evitare congestione TODO 
### Delayed ACK

La tecnica dei riscontri ritardati (_delayed acknowledgments_) ritarda ACK e notifiche di finestra per un breve intervallo, di solito tra 200 e 500 ms, così da accorpare i riscontri e ridurre overhead. 

Il vincolo di questa tecnica si trova nelle specifiche di host TCP: l'ACK non deve essere ritardato eccessivamente, e in particolare il ritardo deve essere minore di $0.5 s$. Inoltre, in un flusso di segmenti pieni, si raccomanda un ACK almeno ogni due segmenti.

>[!warning] Tiny writes
>Resta il problema che il mittente può comunque spedire tanti piccoli segmenti (es. 41 byte per carattere nell'esempio di Telnet).
## Algoritmo di Nagle

L'algoritmo di Nagle può essere usato come strategia per ridurre i pacchetti piccoli quando il produttore (applicazione mittente) invia pochi byte alla volta. 

Il funzionamento di questo algoritmo è il seguente: 
- invia subito il primo segmento, che può essere anche di 1 byte
- **non** inviare altri piccoli segmenti finché una di queste condizioni non si verifica
	1) il segment precedente è stato riscontrato
	2) è stata raggiunta nella coda di spedizione la dimensione massima di un segment

Questa tecnica migliora moltissimo l’efficienza su link con **RTT alto** (_WAN_), perché durante un _RTT_ si accumulano più caratteri e possono essere impacchettati insieme, mentre su LAN con **RTT basso** l’effetto è meno evidente.
### Interazione tra Nagle e delayed ACK

In pratica Nagle e i delayed ACK possono interagire creando attese percepibili nelle app molto interattive: Nagle aspetta un ACK prima di mandare altro piccolo traffico, e delayed ACK aspetta un po’ prima di mandare l’ACK, quindi la latenza può aumentare. Per questo molte applicazioni interattive disabilitano Nagle (es. via _TCP_NODELAY_) quando preferiscono latenza minima a costo di più pacchetti.
## Silly Window Syndrome

Il caso complementare avviene quando l’applicazione ricevente consuma lentamente (anche 1 byte alla volta), il buffer di ricezione si riempie e la finestra advertised diventa zero. Appena l'app legge 1 byte, il TCP ricevente potrebbe annunciare un aumento di finestra minuscolo, e il mittente finisce per inviare segmenti piccolissimi (1 byte per segmento). Questo comportamento è la _silly window syndrome_ (**SWS**).
### Soluzione di Clark

La soluzione di Clark (lato ricevente) ha il seguente funzionamento: 
1) non annunciare micro-incrementi di _window_
2) aspetta finché lo spazio libero non è abbastanza, tipicamente almeno metà buffer o almeno una _MSS_
3) solo allora invia un window update. 

Così il mittente può tornare a spedire segmenti di dimensione sensata, evitando overhead enorme e congestione autoinflitta.
