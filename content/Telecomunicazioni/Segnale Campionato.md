---
tags:
  - segnale
---
Il segnale campionato è il prodotto tra un [[Segnale]] analogico originale e un treno di [[Delta di Dirac|impulsi]].
$$
x_c(t)=x(t)\cdot\pi(t)=\sum_{n=-\infty}^{+\infty}x(t)\delta(t-nT)=\sum_{n=-\infty}^{+\infty}x(nT)\delta(t-nT)
$$
>[!info] Grafico di un segnale campionato
>![[campionamento.png|center]]

La [[Trasformata Discreta di Fourier]] del segnale campionato si può anche ottenere a partire dalla periodicizzazaione della [[Trasformata di Fourier]] del segnale originale
$$
X_c(f)=X(f)*\Pi(f)=\frac{1}{T}\sum_{n=-\infty}^{+\infty}X\left(f-\frac{n}{T}\right)
$$
## Esempi
### Esercizio 1
Data la seguente sequenza $x[n]$, calcolare la sua trasformata.
$$
x[n]=\left[\frac{\sin(2\pi f_c nT)}{\pi n}\right]^2=4f_c^2T^2\operatorname{sinc}^2 (2f_cnT)
$$
La sequenza può essere estratta dal segnale $x(t)$ ponendo $nT = t$.
$$
x(t)=4f_c^2T^2\operatorname{sinc}^2(2f_ct)
$$
La trasformata di Fourier del segnale si può calcolare con la proprietà di scala.
$$
X(f)=4f_c^2T^2\frac{1}{2f_c}\operatorname{tri}\left(\frac{f}{2f_c}\right)=2f_cT^2\operatorname{tri}\left(\frac{f}{2f_c}\right)
$$
A questo punto, per ottenere la trasformata $X_c(f)$ della sequenza, basta periodicizzare $X(f)$ sul dominio.
$$
X_c(f)=\frac{1}{T}\sum_{n=-\infty}^{+\infty}X\left(f-\frac{n}{T}\right)=2f_cT\sum_{n=-\infty}^{+\infty}\operatorname{tri}\left(\frac{f-nT}{2f_c}\right)
$$
### Esercizio 2
Data la seguente sequenza $x[n]$, calcolare la sua trasformata.
$$
x[n]=\frac{\sin\left(\frac{\pi n}{3}\right)}{\pi n}\cos\left(\frac{2\pi n}{6}\right)
$$La sequenza può essere estratta dal segnale $x(t)$ ponendo $nT = t$. Dato che $T$ non esiste nella formula viene posto $T=1$.
$$
x(t)=\frac{\sin\left(\frac{\pi t}{3}\right)}{\pi t}\cos\left(\frac{2\pi t}{6}\right)=\frac{1}{3}\operatorname{sinc}\left(\frac{t}{3}\right)\cos\left(\frac{2\pi t}{6}\right)
$$
Si può calcolare la trasformata del segnale usando la [[Trasformata di Fourier#Convoluzione|proprietà di convoluzione]].
$$
\begin{align*}
X(f)&=\operatorname{rect}(3f)*\left[\frac{1}{2}\delta\left(f-\frac{1}{6}\right)+\frac{1}{2}\delta\left(f+\frac{1}{6}\right)\right]=\\
&=\frac{1}{2}\operatorname{rect}\left[3\left(f-\frac{1}{6}\right)\right]+\frac{1}{2}\operatorname{rect}\left[3\left(f+\frac{1}{6}\right)\right]
\end{align*}
$$
A questo punto, per ottenere la trasformata $X_c(f)$ della sequenza, si periodicizza $X(f)$ sul dominio.
$$
X_c(f)=\frac{1}{2}\sum_{n=-\infty}^{+\infty}\operatorname{rect}\left[3\left(f-n-\frac{1}{6}\right)\right]+\operatorname{rect}\left[3\left(f-n+\frac{1}{6}\right)\right]
$$
### Esercizio 3
Data la seguente sequenza $x[n]$, calcolare la sua trasformata.
$$
x[n]=\left[\frac{\sin\left(\frac{\pi n}{3}\right)}{\pi n}\right]^2=\frac{1}{9}\operatorname{sinc}^2\left(\frac{n}{3}\right)
$$
La sequenza può essere estratta dal segnale $x(t)$ ponendo $nT = t$. Dato che $T$ non esiste nella formula viene posto $T=1$.
$$
x(t)=\frac{1}{9}\operatorname{sinc}^2\left(\frac{t}{3}\right)
$$
Si può calcolare facilmente la trasformata del segnale conoscendo la trasformata elementare del [[Seno Cardinale]].
$$
X(f)=\frac{1}{3}\operatorname{tri}(3f)
$$
A questo punto, per ottenere la trasformata $X_c(f)$ della sequenza, si periodicizza $X(f)$ sul dominio.
$$
X_c(f)=\frac{1}{3}\sum_{n=-\infty}^{+\infty}\operatorname{tri}\left[3(f-n)\right]
$$
