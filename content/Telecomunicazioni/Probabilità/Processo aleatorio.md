---
tags:
  - statistica
  - segnale
---
Un processo aleatorio è la versione [[Teoria della probabilità|probabilistica]] di un [[Sistema#Sistema dinamico|sistema dinamico]] che rappresenta la variazione in modo casuale nel tempo di una grandezza.

## Definizione
### Variabile aleatoria
Un processo viene definito partendo da un insieme di [[Variabile Aleatoria|variabili aleatorie]] $\set{X(t),\ t\in\mathbb{R}}$ dipendenti dal tempo $t$, definite su un opportuno spazio campionario $\Omega$.
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
#### Momento del secondo ordine
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
### Coseno a fase aleatoria
Studiare il processo $x(t)=\cos\left(\frac{2\pi t}{T}+\phi\right)$, dove $\phi$ è una variabile aleatoria con distribuzione di probabilità uniforme tra $0$ e $2\pi$.
$$
P_\phi(\phi)=\frac{1}{2\pi}\operatorname{rect}\left(\frac{\phi-\pi}{2\pi}\right)
$$