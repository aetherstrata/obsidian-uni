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
Un processo si dice **stazionario** se le sue gerarchie del *primo ordine*[^1] non dipendono dal tempo e le sue gerarchie del *secondo ordine*[^2]  non dipendono da $t_1$ e $t_2$ separatamente ma dalla loro *differenza*.

>[!tip] Differenza nulla
>Nel caso in cui la differenza tra $t_1$ e $t_2$ sia nulla, allora la correlazione del processo stazionario avrà la seguente forma, che è uguale alla [[Energia e potenza|potenza]] del processo.
>$$
>R_X(0)=\mathbb{E}[x(t)x(t)]=\mathbb{E}[x^2(t)]=P_x
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