---
tags:
  - sistema
  - statistica
---
Il rumore è un segnale indesiderato di disturbo delle informazioni in un [[Sistema]]. Il rumore viene definito come una somma di oscillazioni irregolari, intermittenti e/o [[Processo aleatorio|statisticamente casuali]].
## Proprietà
### Additivo
Il rumore è *additivo* se si somma al processo interessato.
$$
z(t)=x(t)+y(t)
$$
### Gaussiano
Il rumore è *gaussiano* se rispetta le caratteristiche di una [[Distribuzione di Probabilità#Distribuzione normale|distribuzione normale]].
### Bianco
Il rumore è *bianco* se ha valore atteso nullo e spettro densità di potenza costante.
$$
\mathbb{E}[w(t)]=0\quad;\quad G_W(f)=\frac{N_0}{2}
$$
>[!tip] Rumore nei sistemi reali
>Dato che questo tipo di rumore ha densità spettrale di potenza teoricamente costante, la sua potenza è infinita. Nella realtà questo è irrealizzabile e si limita lo studio del rumore nella banda del segnale.
>$$
>P_W=\int_{-B}^{B}G(f)df=\int_{-B}^{B}\frac{N_0}{2}df=N_0B
>$$
## AWGN
Il rumore gaussiano bianco additivo rappresenta il **rumore termico** generato dall'agitazione dei portatori di carica.
### Proprietà di un segnale disturbato
Le variabili aleatorie sono incorrelate e possono essere fattorizzate (in questo corso).
#### Valore atteso
$$
\begin{align*}
\mathbb{E}[z(t)]&=\int_{-\infty}^{\infty}z(t)P_Z(z)\,dz =\\
&=\iint_{-\infty}^{\infty}[x(t)+y(t)]P_{X|Y}(x,y)\,dxdy=\\
&=\int_{-\infty}^{\infty}x(t)P_X(x)\,dx+\int_{-\infty}^{\infty}y(t)P_Y(y)\,dy=\\
&=\mu_X+\mu_Y
\end{align*}
$$
#### Varianza
$$
\sigma^2_Z=\sigma^2_X+\sigma^2_Y
$$
#### Correlazione
$$
\begin{align*}
R_Z(\tau)&=\mathbb{E}[z(t+\tau)z(t)]=\\
&=\mathbb{E}[(x(t+\tau)+y(t+\tau))(x(t)+y(t))]=\\
&=\underbrace{\mathbb{E}[(x(t+\tau)x(t))]}_{R_X(\tau)}+\underbrace{\mathbb{E}[(y(t+\tau)y(t))]}_{R_Y(\tau)}\mathrel{+}\\
&\quad\mathrel{+}\mathbb{E}[(x(t+\tau)y(t))]+\mathbb{E}[(y(t+\tau)x(t))]=\\
&=R_X(\tau)+R_Y(\tau)+2\mu_X\mu_Y
\end{align*}
$$
#### Spettro densità di potenza
$$
R_Z(\tau)\overset{\mathcal{F}}{\longrightarrow}G_Z(f)=G_X(f)+G_Y(f)+2\mu_X\mu_Y\cdot\delta(f)
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
BER = 10^{-9} \quad\longrightarrow\quad Q\left(\sqrt{\frac{2E_B}{N_0}}\right)=6 \quad\longrightarrow\quad \frac{E_b}{N_0}=18
$$
Il corrispondente valore in decibel di $18$ è $12.5 \text{ dB}$.

Seguendo il secondo teorema di Shannon, la capacità massima $C$ del canale trasmissivo con banda $B$ è legata al rapporto segnale-rumore.
$$
C=B\log_2\left(1+\frac{P_S}{P_N}\right)=B\log_2(1+\text{SNR})
$$E questo influenza la quantità di simboli che si possono inviare.
$$
\begin{align*}
C&=B\log_2(1+\text{SNR})\\
2B\log_2 M&=B\log_2(1+\text{SNR})\\
\log_2 M^2&=\log_2(1+\text{SNR})\\
M&=\sqrt{1+\text{SNR}}
\end{align*}
$$
## Puppa
Sapendo che la potenza è proporzionale al rapporto tra l'energia e il tempo di bit, si può manipolare la formula della capacità trasmissiva.
$$
P_S=\frac{E_b}{T_b}=E_b\cdot C \quad\longrightarrow\quad C=B\log_2\left(1+\frac{E_b\cdot C}{N_0\cdot B}\right)\\
$$
Allora si può esplicitare il rapporto tra $C$ e $B$.
$$
\begin{align*}
\frac{C}{B}&=\log_2\left(1+\frac{E_b\cdot C}{N_0\cdot B}\right)\\
2^{\frac{C}{B}}&=1+\frac{E_b\cdot C}{N_0\cdot B}\\
\frac{E_b}{N_0}&=\frac{2^{\frac{C}{B}}-1}{\frac{C}{B}}
\end{align*}
$$
In questa relazione è espressa la potenza di ricezione come prodotto tra l'energia di bit e la capacità trasmissiva. 

Le due grandezze $\dfrac{E_b}{N_0}$ e $\dfrac{C}{B}$ si possono analizzare anche sul grafico della funzione.

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
