---
tags:
  - segnale
  - sistema
  - digitale
---
Una trasmissione si dice *digitale* se i dati sono codificati in sequenze di [[Entropia|bit]].

Si può avere una trasmissione digitale anche partendo da una sorgente analogica, attraverso un *convertitore analogico-digitale* che effettua il [[Telecomunicazioni/Teorema del Campionamento|campionamento]], la [[Quantizzazione|quantizzazione]] e, per finire, la codifica di sorgente per comprimere i dati e la codifica di canale per aggiungere ridondanza per la rivelazione e/o correzione degli errori.
$$
x(t)\longrightarrow\boxed{\vphantom{\int}\text{ campionatore }}\overset{X[n]}{\longrightarrow}\boxed{\vphantom{\int}\text{ quantizzatore }}\overset{X'[n]}{\longrightarrow}\boxed{\vphantom{\int}\text{ codificatore }}\longrightarrow C_n
$$
A questo poi si aggiunge un modulatore che effettua una codifica di linea con cui il segnale viene adattato alle caratteristiche del canale di trasmissione ed il segnale viene inviato nel canale.

Alla ricezione corrispondono le operazioni complementari a quanto detto (es. decodifica, demodulazione, ecc...).
$$
x(t)\longrightarrow\boxed{\vphantom{\int}\text{ ADC }}\longrightarrow\boxed{\vphantom{\int}\text{ canale }}\longrightarrow\boxed{\vphantom{\int}\text{ DAC }}\longrightarrow x(t)
$$
## Velocità
La velocità di trasmissione di un sistema si misura sul tempo $T_s$ necessario per trasmettere un simbolo.

L'inverso di questo tempo di simbolo è chiamato *frequenza di simbolo*, o *baud rate*.
$$
f_s=\frac{1}{T_s}
$$
Un sistema può anche trasmettere più [[Entropia|bit]] per simbolo, quindi per ottenere il bit rate è necessario moltiplicare la frequenza di simbolo per il numero di bit $N$.
$$
R=Nf_s=f_s \log_2 M
$$
## SNR
Il *rapporto segnale-rumore* è il rapporto tra la potenza utile del segnale rispetto a quella del rumore e deve essere il più alto possibile.
$$
\text{SNR}=\frac{\text{potenza segnale}}{\text{potenza rumore}}
$$
## Filtro adattato
Per rimuovere il rumore si applica un [[Filtro]] adattato alla fine del canale trasmissivo.
$$
x(t)\longrightarrow\boxed{\vphantom{\int}\text{ canale rumoroso }}\longrightarrow\boxed{\vphantom{\int}\text{ filtro - }h(t)\ }\longrightarrow y(t)
$$
L'uscita del sistema sarà la [[Convoluzione]] tra l'ingresso e la risposta impulsiva del filtro.
$$
y(t)=[x(t)+n(t)]*h(t)=x(t)*h(t)+n(t)*h(t)
$$
Supponendo di trasmettere solo un simbolo, la sommatoria dei simboli si elimina e rimarrà solo il simbolo in $t=0$. Per tutti gli altri simboli si applica la stessa statistica.
$$
\begin{align*}
y(t)&=\sum_m c_m \cdot g(t-mT_s)*h(t)+n(t)*h(t)=\\
&=\underbrace{c_m \cdot g(t)*h(t)}_{\text{segnale utile}}+n(t)*h(t)=\\
&=c_m\int_{-\infty}^{\infty}G(f)H(f)e^{i2\pi ft}df+n(t)*h(t)
\end{align*}
$$
Solo il segnale in $t=0$ è utile, quindi l'integrale si può semplificare.
$$
y(t)=c_m\int_{-\infty}^{\infty}G(f)H(f)\,df+n(t)*h(t)
$$
Si può ricavare la potenza dal modulo quadrato della [[Trasformata di Fourier]] del segnale.
$$
P_X=c_m^2\left|\int_{-\infty}^{\infty}G(f)H(f)\,df\right|^2
$$
A differenza del segnale $x(t)$, che è deterministico, il rumore è un processo aleatorio con statistica gaussiana e densità spettrale costante.
$$
P_N=\int_{-\infty}^{\infty}\frac{N_0}{2}|H(f)|^2\,df
$$
Unendo i due termini si ottiene il rapporto segnale-rumore.
$$
\text{SNR}=\frac{\displaystyle c_m^2\left|\int_{-\infty}^{\infty} G(f)H(f)\,df\right|^2}{\displaystyle \frac{N_0}{2} \int_{-\infty}^{\infty}|H(f)|^2\,df}
$$
Si può ricavare la funzione di trasferimento del filtro adattato usando la disuguaglianza di Schwarz, secondo cui il modulo quadrato dell'integrale del prodotto è minore o uguale del prodotto degli integrali dei moduli.
$$
\frac{\displaystyle c_m^2\left|\int_{-\infty}^{\infty} G(f)H(f)\,df\right|^2}{\displaystyle \frac{N_0}{2} \int_{-\infty}^{\infty}|H(f)|^2\,df}
\le
\frac{\displaystyle c_m^2 \int_{-\infty}^{\infty}|G(f)|^2df\ \cdot \int_{-\infty}^{\infty}|H(f)|^2df}{\displaystyle \frac{N_0}{2} \int_{-\infty}^{\infty}|H(f)|^2\,df}
$$
I due termini si equivalgono quando $H(f)=G^*(f)$, ovvero quando $h(t)=g^*(-t)$. L'impulso in banda base è sempre un segnale reale quindi si può omettere la stella di coniugazione.
$$
\text{SNR}=\frac{2\,\displaystyle c_m^2 \int_{-\infty}^{\infty}|G(f)|^2df}{N_0}=\frac{2\,c_m^2 E_G}{N_0}
$$
Questo rapporto influenza il [[Quantizzazione#Bit Error Rate|BER]] del sistema.
$$
\text{BER}=Q\left(\frac{\sqrt{\text{SNR}}_2-\sqrt{\text{SNR}}_1}{2}\right)=Q\left(\frac{C_2-C_1}{2}\sqrt{\frac{2E_G}{N_0}}\right)
$$
#### Sistema unipolare
Nei sistemi unipolari, uno dei due simboli ha valore nullo.
$$
\text{BER}=Q\left(C_2\sqrt{\frac{E_G}{2N_0}}\right)
$$
Il bit error rate si può scrivere anche come rapporto tra l'energia di un bit e la densità spettrale del rumore.
$$
E_B=\frac{C_2^2E_G+0}{2} \quad\longrightarrow\quad \text{BER}=Q\left(\sqrt{\frac{E_B}{N_0}}\right)
$$
#### Sistema bipolare
Nei sistemi bipolari, i due simboli hanno valori opposti.
$$
\text{BER}=Q\left(2\,C_2\sqrt{\frac{E_G}{2N_0}}\right)
$$
Il bit error rate si può scrivere anche come rapporto tra l'energia di un bit e la densità spettrale del rumore.
$$
E_B=\frac{2C_2^2E_G}{2} \quad\longrightarrow\quad \text{BER}=Q\left(\sqrt{\frac{2E_B}{N_0}}\right)
$$
## Sistema privo di errori
Un sistema è considerato privo di errori se il suo $\text{BER}$ è minore di $10^{-9}$, che equivale ad avere un argomento di $Q$ maggiore di $6$.

>[!info] Qui è studiato il caso di sistemi bipolari, per quelli unipolari basta cambiare i parametri

$$
\text{BER} = 10^{-9} \quad\longrightarrow\quad Q\left(\sqrt{\frac{2E_B}{N_0}}\right)=6 \quad\longrightarrow\quad \frac{E_b}{N_0}=18
$$
Il corrispondente valore in decibel di $18$ è $12.5 \text{ dB}$.

Seguendo il secondo teorema di Shannon, la capacità massima $C$ del canale trasmissivo con banda $B$ è legata al rapporto segnale-rumore.
$$
C=B\log_2\left(1+\frac{P_S}{P_N}\right)=B\log_2(1+\text{SNR})
$$
E questo influenza la quantità di simboli che si possono inviare.
$$
\begin{align*}
C&=B\log_2(1+\text{SNR})\\
2B\log_2 M&=B\log_2(1+\text{SNR})\\
\log_2 M^2&=\log_2(1+\text{SNR})\\
M&=\sqrt{1+\text{SNR}}
\end{align*}
$$
## Limite teorico di trasmissione
Sapendo che la potenza è proporzionale al rapporto tra l'energia e il tempo di bit, si può manipolare la formula della capacità trasmissiva.
$$
P_S=\frac{E_b}{T_b}=E_b\cdot C \quad\longrightarrow\quad C=B\log_2\left(1+\frac{E_b }{N_0}\cdot\frac{C}{B}\right)\\
$$
Allora si può esplicitare il rapporto tra $C$ e $B$.
$$
\begin{align*}
\frac{C}{B}&=\log_2\left(1+\frac{E_b }{N_0}\cdot\frac{C}{B}\right)\\
2^{\frac{C}{B}}&=1+\frac{E_b }{N_0}\cdot\frac{C}{B}\\
\frac{E_b}{N_0}&=\frac{2^{\frac{C}{B}}-1}{\frac{C}{B}}
\end{align*}
$$
In questa relazione è espressa la potenza di ricezione come prodotto tra l'energia di bit e la capacità trasmissiva. 

Le due grandezze $\dfrac{E_b}{N_0}$ e $\dfrac{C}{B}$ si possono analizzare anche sul grafico della funzione.

>[!info] Esempio di grafico
>![[1734470074.png]]

Il punto che interseca l'asse delle ascisse è di particolare interesse. 
$$
\begin{align*}
C&=B\log_2\left(1+\frac{E_b }{N_0}\cdot\frac{C}{B}\right)\\
\frac{N_0}{E_b}&=\frac{N_0B}{E_bC}\log_2\left(1+\frac{E_b }{N_0}\cdot\frac{C}{B}\right)
\end{align*}
$$
Si definisce una variabile $\lambda = \dfrac{E_b}{N_0}\cdot\dfrac{C}{B}$ e si cambia la base del logaritmo.
$$
\frac{N_0}{E_b}=\frac{1}{\lambda}\log_2(1+\lambda)=\frac{1}{\lambda}\frac{\ln(1+\lambda)}{\ln2}
$$
Calcolando il limite per $\lambda\to0$ si può ricavare il valore del punto. Tenendo conto che questo limite è riferito a $\dfrac{N_0}{E_b}$, il risultato risulta invertito rispetto al grafico.
$$
\lim_{\lambda\to0}\frac{\ln(1+\lambda)}{\lambda\ln2}=\frac{1}{\ln2} \quad\longrightarrow\quad\frac{E_b}{N_0}=\ln2=-1.6\text{ dB}
$$
Questo è il limite sotto al quale è impossibile andare.
## Saturazione della capacità
La capacità del canale indica la massima velocità teorica di trasferimento affinché l´[[Entropia|informazione]] possa essere trasmessa in modo affidabile.

Come detto nei paragrafi precedenti, la capacità trasmissiva di un canale è legata alla potenza del segnale ricevuto e del rumore.
$$
C=B\log_2\left(1+\frac{P_R}{N_0B}\right)
$$
### Banda illimitata
Disegnando il grafico della capacità in funzione della banda si osserva un asintoto orizzontale oltre il quale la capacità non può aumentare.

Quindi viene studiato il caso in cui il rapporto segnale-rumore sia piccolo ($\text{SNR}\ll0\text{ dB}$).
$$
\begin{align*}
C&=\frac{N_0B}{P_R}\cdot\frac{P_R}{N_0}\log_2\left(1+\frac{P_R}{N_0B}\right)=\\
\boxed{\ \lambda \coloneqq \dfrac{P_R}{N_0B}\ }\quad &= \frac{1}{\lambda}\cdot\frac{P_R}{N_0}\log_2(1+\lambda)=\\
&=\frac{P_R}{N_0}\frac{\ln(1+\lambda)}{\lambda\ln2}
\end{align*}
$$
Per calcolare il valore dell'asintoto si calcola il limite della funzione per $B\to\infty$, quindi quando $\lambda\to0$.
$$
C=\frac{P_R}{N_0}\cdot\lim_{\lambda\to0}\frac{\ln(1+\lambda)}{\lambda\ln2}=\frac{P_R}{N_0}\cdot\frac{1}{\ln2}
$$
Questo è chiamato *limite di potenza limitata*.
### Banda nulla
Disegnando il grafico della capacità in funzione della banda si osserva come la curva passa per l'origine.

Quindi viene studiato il caso in cui il rapporto segnale-rumore sia grande ($\text{SNR}\gg0\text{ dB}$). 

In questo caso il  termine $1$ dentro al logaritmo può essere trascurato.
$$
\begin{align*}
C&=B\log_2\frac{P_R}{N_0B}=\\
&=B\frac{\log_{10}\frac{P_R}{N_0B}}{\log_{10}2}=\\
&=B\frac{\text{SNR}_{\text{dB}}}{\log_{10}2}\approx \frac{B}{3}\text{SNR}_{\text{dB}}
\end{align*}
$$
Questo è chiamato *limite di banda limitata*.
## Esercizi
### Esercizio 3
Dato un canale trasmissivo con banda $B=10\text{ kHz}$, ricavare il valore del rapporto segnale-rumore necessario per avere capacità $C=50\text{ kb/s}$.

Dai dati si evince il rapporto $\dfrac{C}{B} = 5$, e di conseguenza che $1+\text{SNR}=2^5$.
$$
\text{SNR}=2^5-1=31=14.3\text{ dB}
$$
### Esercizio 4
Dato un canale trasmissivo con banda $B=1\text{ MHz}$ e rapporto segnale-rumore $\text{SNR}=-30\text{ dB}$, ricavare la capacità di Shannon.
$$
C=B\log_2(1-30\text{ dB})=B\log_2(1+10^{-3})=1.44\text{ kb/s}
$$
