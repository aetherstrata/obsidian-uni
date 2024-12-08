---
tags:
  - statistica
  - segnale
---
Un processo aleatorio è la versione [[Teoria della probabilità|probabilistica]] di un [[Sistema#Sistema dinamico|sistema dinamico]] che rappresenta la variazione in modo casuale nel tempo di una grandezza.
## Definizione
### Variabile aleatoria
Un processo viene definito partendo da un insieme di [[Variabile Aleatoria|variabili aleatorie]] $\set{X(t),\ t\in\mathbb{R}}$ dipendenti dal tempo $t$, definite su un opportuno spazio campionario $\Omega$.
### Processo stazionario
Un processo si dice **stazionario** se le sue gerarchie del *primo ordine*[^1] non dipendono dal tempo e le sue gerarchie del *secondo ordine*[^2]  non dipendono da $t_1$ e $t_2$ separatamente ma dalla loro *differenza* $\tau$.
$$
R_X(\tau)=\mathbb{E}[x(t+\tau)x(t)]
$$
Applicando la [[Trasformata di Fourier]] alla correlazione di un processo stazionario si ottiene lo **spettro densità di potenza**.
$$
R_X(\tau)\overset{\mathcal{F}}{\longrightarrow}G_X(f)=\int_{-\infty}^{\infty}R_X(\tau)e^{-i2\pi f\tau}\,d\tau
$$
Di conseguenza, esiste anche la trasformazione inversa $G_X(\tau)\overset{\mathcal{F^{-1}}}{\longrightarrow}R_X(f)$.
$$
G_X(f)\overset{\mathcal{F^{-1}}}{\longrightarrow}R_X(\tau)=\int_{-\infty}^{\infty}G_X(f)e^{i2\pi f\tau}\,df
$$
>[!tip] Differenza nulla
>Nel caso in cui la differenza $\tau$ sia nulla, allora la correlazione del processo stazionario avrà la seguente forma, che è uguale alla [[Energia e potenza|potenza]] del processo.
>$$
>R_X(0)=\mathbb{E}[x(t)x(t)]=\mathbb{E}[x^2(t)]=P_x
>$$
>Applicando questa relazione alla trasformata dello spettro densità si ottiene una formula alternativa della potenza.
>$$
>P_X=R_X(0)=\int_{-\infty}^{\infty}G_X(f)\,df
>$$

[^1]:  valore medio, valore quadratico medio, varianza
[^2]:  correlazione e covarianza
### Processo ergodico
Un processo di dice **ergodico** se è un processo stazionario se, per ogni momento di $t$, la media temporale è uguale alla media d'insieme.
$$
\mathbb{E}[x(t)]=\int_{-\infty}^{\infty}x(t)P_X(x)\,dx=\lim_{\Delta T\to\infty}\frac{1}{\Delta T}\int_{-\Delta T/2}^{\Delta T/2}x(t)\,dt
$$
Questa proprietà può essere applicata alla definizione della potenza.
$$
P_X=\mathbb{E}[x^2(t)]=\lim_{\Delta T\to\infty}\frac{1}{\Delta T}\int_{-\Delta T/2}^{\Delta T/2}x^2(t)\,dt
$$
## Esempi
### Coseno con ampiezza aleatoria
Studiare il processo $x(t)=A\cos\left(\frac{2\pi t}{T}\right)$, dove $A$ è una variabile aleatoria con distribuzione di probabilità uniforme tra $0$ e $1$.
$$
P_A(a)=\operatorname{rect}\left(a-\frac{1}{2}\right)
$$
>[!info] Casi particolari
>Segnato un istante di tempo $\overline{t_1} = T$, l'uscita del processo è proprio uguale al valore della variabile aleatoria.
>$$
>X_1=x(\overline{t_1}=T)=A
>$$
>---
>Segnato un istante di tempo $\overline{t_2}=\frac{T}{2}$, l'uscita del processo è uguale all'opposto della variabile aleatoria.
>$$
>X_2=x\left(\overline{t_2}=\frac{T}{2}\right)=-A
>$$
>Da questo segue che la distribuzione di probabilità della variabile aleatoria $X_2$ è ribaltata rispetto ad $A$.
>$$
>P_{X_2}(x_2)=\operatorname{rect}\left(x_2+\frac{1}{2}\right)
>$$
>---
>Segnato un istante di tempo $\overline{t_3}=\frac{T}{4}$, l'uscita del processo è sempre nulla.
>$$
>X_3=x\left(\overline{t_3}=\frac{T}{4}\right)=0
>$$
>Da questo segue che la distribuzione di probabilità della variabile aleatoria $X_3$ è una [[Delta di Dirac]].
>$$
>P_{X_3}=\delta(x_3)
>$$

#### Valore medio
$$
\begin{align*}
\mathbb{E}[X]&=\int_{-\infty}^{\infty}x(t)P_X(x)\,dx=\\
&=\int_{-\infty}^{\infty}A\cos\left(\frac{2\pi t}{T}\right)P_A(A)\,dA=\\
&=\int_{0}^{1}A\cos\left(\frac{2\pi t}{T}\right)\,dA=\\
&=\frac{1}{2}\cos\left(\frac{2\pi t}{T}\right)
\end{align*}
$$
#### Valore quadratico medio
$$
\begin{align*}
\mathbb{E}[X^2]&=\int_{-\infty}^{\infty}x^2(t)P_X(x)\,dx=\\
&=\int_{-\infty}^{\infty}A^2\cos^2\left(\frac{2\pi t}{T}\right)P_A(A)\,dA=\\
&=\int_{0}^{1}A^2\cos^2\left(\frac{2\pi t}{T}\right)\,dA=\\
&=\frac{1}{3}\cos^2\left(\frac{2\pi t}{T}\right)
\end{align*}
$$
#### Varianza
$$
\sigma^2_X=\mathbb{E}[X^2]-\mathbb{E}[X]^2=\frac{1}{3}\cos^2\left(\frac{2\pi t}{T}\right)-\frac{1}{4}\cos^2\left(\frac{2\pi t}{T}\right)=\frac{1}{12}\cos^2\left(\frac{2\pi t}{T}\right)
$$
#### Correlazione
$$
\begin{align*}
R_X(t_1,t_2)&=\int_{0}^{1}A^2\cos\left(\frac{2\pi t_1}{T}\right)\cos\left(\frac{2\pi t_2}{T}\right)\,dA=\\
&=\frac{1}{3}\cos\left(\frac{2\pi t_1}{T}\right)\cos\left(\frac{2\pi t_2}{T}\right)
\end{align*}
$$
### Coseno a fase aleatoria
Studiare il processo $x(t)=\cos\left(\frac{2\pi t}{T}+\phi\right)$, dove $\phi$ è una variabile aleatoria con distribuzione di probabilità uniforme tra $0$ e $2\pi$.
$$
P_\phi(\phi)=\frac{1}{2\pi}\operatorname{rect}\left(\frac{\phi-\pi}{2\pi}\right)
$$

Conoscendo le caratteristiche di una variabile aleatoria $Y|X\coloneqq y=\cos(x)$ si possono ricavare facilmente le caratteristiche di questo processo.
$$
P_X(x)=\frac{1}{2\pi}\operatorname{rect}\left(\frac{x-\pi}{2\pi}\right)\to P_Y(y)=\frac{1}{\sqrt{1-y^2}}\operatorname{rect}\left(\frac{y}{2}\right)
$$
#### Valore medio
$$
\mathbb{E}[X]=\int_{-\infty}^{\infty}\cos\left(\frac{2\pi t}{T}+\phi\right)P_\phi(\phi)\,d\phi=\int_{0}^{2\pi}\frac{1}{2\pi}\cos\left(\frac{2\pi t}{T}+\phi\right)\,d\phi=0
$$
#### Valore quadratico medio
$$
\begin{align*}
\mathbb{E}[X^2]&=\int_{-\infty}^{\infty}\cos^2\left(\frac{2\pi t}{T}+\phi\right)P_\phi(\phi)\,d\phi=\\
&=\int_{0}^{2\pi}\frac{1}{2\pi}\cos^2\left(\frac{2\pi t}{T}+\phi\right)\,d\phi=\\
&=\frac{1}{2\pi}\int_{0}^{2\pi}\left[\frac{1}{2}+\frac{1}{2}\cos\left(\frac{4\pi t}{T}+2\phi\right)\right]\,d\phi=\\
&=\frac{1}{4\pi}\int_{0}^{2\pi}d\phi=\\
&=\frac{1}{2}
\end{align*}
$$
#### Varianza
$$
\sigma^2_X=\mathbb{E}[X^2]-\mathbb{E}[X]^2=\frac{1}{2}
$$
#### Correlazione
$$
\begin{align*}
R_X(t_1,t_2)&=\int_{-\infty}^{\infty}\cos\left(\frac{2\pi t_1}{T}+\phi\right)\cos\left(\frac{2\pi t_2}{T}+\phi\right)P_\phi(\phi)\,d\phi=\\
&=\frac{1}{2}\int_{0}^{2\pi}\frac{1}{2\pi}\left[\cos\left(\frac{2\pi (t_1+t_2)}{T}+2\phi\right)+\cos\left(\frac{2\pi (t_1-t_2)}{T}\right)\right]\,d\phi=\\
&=\frac{1}{4\pi}\cos\left(\frac{2\pi (t_1-t_2)}{T}\right)\int_{0}^{2\pi}d\phi=\\
&=\frac{1}{2}\cos\left(\frac{2\pi (t_1-t_2)}{T}\right)
\end{align*}
$$
#### Covarianza
$$
C_X(t_1,t_2)=R_X(t_1,t_2)-\underbrace{\mathbb{E}[X_1]\mathbb{E}[X_2]}_0=\frac{1}{2}\cos\left(\frac{2\pi (t_1-t_2)}{T}\right)
$$
#### Grado di correlazione
$$
\rho_X(t_1,t_2)=\frac{C_X(t_1,t_2)}{\sqrt{\sigma^2_X(t_1)\sigma^2_X(t_2)}}=\cos\left(\frac{2\pi (t_1-t_2)}{T}\right)
$$
## Esercizi
### Esercizio 1
Dato un processo stazionario $x(t)$, con distribuzione di probabilità uniforme tra $-2$ e $10$, calcolare  l'autocorrelazione, il valore di $A$ della covarianza.
$$
C_X(\tau)=A\operatorname{tri}\left(\frac{\tau}{2}\right)
$$
Dato il seguente sistema, calcolare il valore atteso $\mu_Y$, la correlazione per $Y$ e la potenza.
$$
y(t)=x(t)+x(t-1)
$$
Per cominciare si ricavano alcune misure del processo.
$$
\begin{align*}
\mu_X&=\int_{-2}^{10}\frac{x}{12}\, dx=4\\
\mu_X^{(2)}&=\int_{-2}^{10}\frac{x^2}{12}\, dx=28\\
\sigma_X^2&=\mu_X^{(2)}-\mu_X^2=28-16=12
\end{align*}
$$
Applicando questi valori alla formula della covarianza si ottiene il valore di $A$.
$$
C_X(\tau)=A\operatorname{tri}\left(\frac{\tau}{2}\right)=\mathbb{E}[(x(t+\tau)-\mu_X)(x(t)-\mu_X)]
$$
Per $\tau=0$ la covarianza è uguale alla varianza.
$$
\begin{gather*}
\begin{aligned}
C_X(0)&=A\operatorname{tri}\left(\frac{0}{2}\right)=A\\
C_X(0)&=\mathbb{E}[(x(t)-\mu_X)^2]=\sigma_X^2=12\\\hline
\end{aligned} \\
\boxed{A=12} \\\\
C_X(\tau)=12\operatorname{tri}\left(\frac{\tau}{2}\right)
\end{gather*}
$$
L'autocorrelazione si può ricavare con la covarianza appena trovata.
$$
R_X(\tau)=C_X(\tau)+\mu_X^2=12\operatorname{tri}\left(\frac{\tau}{2}\right)+16
$$
Il valore atteso dell'uscita del sistema si calcola sommando i singoli valori attesi.
$$
\mu_Y=\mathbb{E}[x(t)]+\mathbb{E}[x(t-1)]=4+4=8
$$
La correlazione per $Y$ si ricava in funzione dei processi in ingresso.
$$
\begin{align*}
R_Y(\tau)&=\mathbb{E}[y(t+\tau)y(t)]=\\
&=\mathbb{E}[(x(t+\tau)+x(t+\tau-1))(x(t)+x(t-1))]=\\
&=\mathbb{E}[x(t+\tau)x(t)]+\mathbb{E}[x(t+\tau-1)x(t)]\mathrel{+}\\
&\quad\mathrel{+}\mathbb{E}[x(t+\tau)x(t-1)]+\mathbb{E}[x(t+\tau-1)x(t-1)]=\\
&=R_X(\tau)+R_X(\tau-1)+R_X(\tau+1)+R_X(\tau)=\\
&=2R_X(\tau)+R_X(\tau-1)+R_X(\tau+1)
\end{align*}
$$
Calcolare la correlazione in $0$ corrisponde alla potenza del processo.
$$
P_Y=2R_X(0)+R_X(-1)+R_X(1)=2\cdot28+22+22=100
$$
### Esercizio 2
Dato il seguente processo $x(t)$, a cui è aggiunto [[Rumore]] bianco, calcolare il valore atteso e la densità spettrale di potenza.
$$
x(t)=(A+2B)\cos(300\pi t-\theta)+n(t)
$$
Le distribuzioni di probabilità delle variabili aleatorie $A$, $B$ e $\theta$ sono le seguenti.
$$
\begin{align*}
P_A(a)&=\frac{1}{2}\operatorname{rect}\left(\frac{a}{2}\right)\\
P_B(b)&=\frac{1}{4}\operatorname{rect}\left(\frac{b}{4}\right)\\
P_\theta(\theta)&=\frac{1}{4\pi}\operatorname{rect}\left(\frac{\theta-2\pi}{4\pi}\right)\\
\end{align*}
$$
Il rumore presenta la seguente autocorrelazione.
$$
R_N(\tau)=10\delta(\tau)
$$
Da questo si può ricavare facilmente la potenza e la densità spettro di potenza del rumore.
$$
\begin{gather*}
G_N(f)=\mathcal{F}\set{R_N(\tau)}=\mathcal{F}\set{10\delta(\tau)}=10\\\\
P_N=R_N(0)=10\delta(0)=10
\end{gather*}
$$
Dato che tutte le variabili aleatorie sono indipendenti tra loro, possono essere fattorizzate nel calcolo delle statistiche.
$$
\begin{align*}
\mathbb{E}[x(t)]&=\mathbb{E}[(A+2B)\cos(300\pi t-\theta)+n(t)]=\\
&=\mathbb{E}[(A+2B)\cos(300\pi t-\theta)]+\cancel{\mathbb{E}[n(t)]}=\\
&=(\cancel{\mathbb{E}[A]}+2\cancel{\mathbb{E}[B]})\mathbb{E}[\cos(300\pi t-\theta)]=\\
&=0
\end{align*}
$$
Il processo ha valore atteso nullo. Ora si procede con il calcolo dell'autocorrelazione.
$$
\begin{align*}
R_X(\tau)&=\mathbb{E}[x(t+\tau)x(t)]=\\
&=\mathbb{E}[\set{(A+2B)\cos(300\pi (t+\tau)-\theta)+n(t+\tau)}\mathrel{\cdot}\\
&\quad\mathrel{\cdot}\set{(A+2B)\cos(300\pi t-\theta)+n(t)}]=\\
&=\mathbb{E}[(A+2B)^2\cos(300\pi (t+\tau)-\theta)\cos(300\pi t-\theta)]\mathrel{+}\\
&\quad\mathrel{+}\mathbb{E}[(A+2B)\cos(300\pi (t+\tau)-\theta)n(t)]\mathrel{+}\\
&\quad\mathrel{+}\mathbb{E}[(A+2B)\cos(300\pi t-\theta)n(t+\tau)]\mathrel{+}\\
&\quad\mathrel{+}\mathbb{E}[n(t+\tau)n(t)]
\end{align*}
$$
Anche qui si possono fattorizzare i termini e calcolare i valori attesi separatamente.
$$
\begin{gather*}
\begin{aligned}
\mathbb{E}[(A+2B)^2]&=\mathbb{E}[A^2]+4\mathbb{E}[B^2]+4\cancel{\mathbb{E}[A]}\cancel{\mathbb{E}[B]}=\\
&=\frac{1}{3}+\frac{16}{3}=\\
&=\frac{17}{3}\\\\\hline
\end{aligned}\\\\
\begin{aligned}
\mathbb{E}[\cos(300\pi&(t+\tau)-\theta)\cos(300\pi t-\theta)]=\\
&=\int_{0}^{4\pi}\frac{1}{4\pi}\cos(300\pi (t+\tau)-\theta)\cos(300\pi t-\theta)\,d\theta=\\
&=\frac{1}{8\pi}\int_{0}^{4\pi}\cos(300\pi(2t+\tau)-2\theta)\,d\theta + \frac{1}{8\pi}\int_{0}^{4\pi}\cos(300\pi\tau)\,d\theta=\\
&=\frac{1}{8\pi}\cos(300\pi\tau)\int_{0}^{4\pi}d\theta=\\
&=\frac{1}{2}\cos(300\pi\tau)
\end{aligned}
\end{gather*}
$$
I termini intermedi sono nulli, rimane solo $\mathbb{E}[(A+2B)^2]\mathbb{E}[\cos(300\pi(t+\tau)-\theta)\cos(300\pi t-\theta)]$ e $\mathbb{E}[n(t+\tau)n(t)]$, che è uguale a $R_N(\tau)$.
$$
R_X(\tau)=\frac{17}{3}\cdot\frac{1}{2}\cos(300\pi\tau)+10\delta(\tau)
$$
Applicando la [[Trasformata di Fourier]] all'autocorrelazione, si ottiene la densità spettrale di potenza.
$$
\begin{align*}
G_X(f)&=\mathcal{F}\left\{\frac{17}{3}\cdot\frac{1}{2}\cos(300\pi\tau)+10\delta(\tau)\right\}=\\
&=\frac{17}{6}\left[\frac{1}{2}\delta(f-150)+\frac{1}{2}\delta(f+150)\right]+10
\end{align*}
$$
### Esercizio 3
Dato un processo stazionario con potenza $P_X=19$ e covarianza $C_X(\tau)=3\operatorname{tri}(\tau)$, calcolare il valore atteso e la varianza. 
#### Parte 1
La varianza si ottiene valutando la covarianza a ritardo nullo.
$$
\sigma^2_X=C_X(0)=3\operatorname{tri}(0)=3
$$
Per ottenere il valore atteso si usa la formula computazionale della varianza in termini di momenti di origine zero.
$$
\sigma^2_X=\mathbb{E}[X^2]-\mathbb{E}[X]^2=P_X-\mu^2_X
$$
Invertendo la formula si ottiene il valore atteso.
$$
\mu_X=\sqrt{P_X-\sigma^2_X}=\sqrt{16}=4
$$
#### Parte 2
Questo processo passa attraverso un filtro con risposta rettangolare di base $3$. Calcolare il valore atteso dell'uscita $y(t)$.
$$
h(t)=\operatorname{rect}\left(\frac{t}{3}\right)
$$
L'uscita del sistema si può calcolare con la [[Convoluzione]] del segnale in entrata con la risposta impulsiva del sistema.
$$
y(t)=x(t)*h(t)=\int_{-\infty}^{\infty}x(t-t')h(t')\,dt'
$$
Sostituendo l'integrale di convoluzione nella formula del valore atteso si ottiene il risultato.
$$
\begin{align*}
\mathbb{E}[y(t)]&=\int_{-\infty}^{\infty}\underbrace{\mathbb{E}[x(t-t')]}_{\mathclap{\mu_X\text{ stazionario}}} h(t')\,dt'=\\
&=\mu_X\int_{-\infty}^{\infty}h(t')\,dt'=\\
&=4\int_{-3/2}^{3/2}dt'=\\
&=12
\end{align*}
$$
### Esercizio 4
Dato un processo aleatorio stazionario $x(t)$, con valore medio $\mu_X=-3$, potenza $P_X=12$ e coefficiente di correlazione $\rho_X(\tau)=\operatorname{sinc}(\tau/4)$, calcolare lo la densità spettrale di potenza.

Conoscendo la potenza e il valore atteso si può ottenere la varianza.
$$
\sigma^2_X=P_X-\mu^2_X=12-9=3
$$
