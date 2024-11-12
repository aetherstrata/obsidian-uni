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
A questo punto, per ottenere la trasformata $X_c$ della sequenza, basta periodicizzare $X(f)$ sul dominio.
$$
X_c(f)=\frac{1}{T}\sum_{n=-\infty}^{+\infty}X\left(f-\frac{n}{T}\right)=2f_cT\sum_{n=-\infty}^{+\infty}\operatorname{tri}\left(\frac{f-nT}{2f_c}\right)
$$