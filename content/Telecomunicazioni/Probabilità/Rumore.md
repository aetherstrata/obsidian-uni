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
Le variabili aleatorie sono incorrelate e possono essere fattorizzate.
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
Il *rapporto segnale-rumore* è il rapporto tra la potenza utile del segnale rispetto a quella del rumore.
$$
\text{SNR}=\frac{\text{potenza segnale}}{\text{potenza rumore}}
$$
### Dimostrazione
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
P_N=\frac{N_0}{2}|H(f)|^2
$$
Unendo i due termini si ottiene il rapporto segnale-rumore.
$$
\text{SNR}=\frac{c_m^2\left|\int_{-\infty}^{\infty}G(f)H(f)\,df\right|^2}{\frac{N_0}{2}|H(f)|^2}
$$