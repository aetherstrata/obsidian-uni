---
tags:
  - segnale
  - teorema
---
## Trasformata di Fourier
Il Teorema di Parseval permette di calcolare velocemente la potenza di un segnale rappresentato come [[Trasformata di Fourier]].
$$
z(t)=x(t)*y^*(-t)=\int_{-\infty}^{\infty}x(\tau)y^*(t+\tau)d\tau
$$
Sfruttando la proprietà della [[#Convoluzione]], si può riscrivere come prodotto tra trasformate.
$$
z(t)=\int_{-\infty}^{\infty}X(f)Y^*(f)e^{i2\pi ft}df
$$
Valutando l'espressione in $t=0$, è evidente l'uguaglianza con l'operazione di prodotto scalare.
$$
z(0)=\underbrace{\int_{-\infty}^{\infty}x(\tau)y^*(\tau)d\tau}_{\left<x(t),y(t)\right>}=\underbrace{\int_{-\infty}^{\infty}X(f)Y^*(f)df}_{{\left<X(f),Y(f)\right>}}
$$
Se si pone $x(t) = y(t)$, quindi si fa la sua [[Correlazione#Autocorrelazione|autocorrelazione]], il risultato sarà l'[[Energia e potenza#Energia di un segnale analogico|energia]] del segnale.
$$
E_x=\int_{-\infty}^{\infty}\left|x(t)\right|^2dt=\int_{-\infty}^{\infty}x(\tau)x^*(\tau)d\tau=\int_{-\infty}^{\infty}X(f)X^*(f)df=\int_{-\infty}^{\infty}\left|X(f)\right|^2df
$$
## Trasformata discreta 
Con la proprietà della convoluzione circolare, si può ricavare l'energia del [[Segnale Campionato]] senza calcolare la [[Energia e potenza#Energia di un segnale discreto|somma dei moduli]].
$$
\sum_{n=-\infty}^{+\infty}x^*[n]x[n]=T \int_{-1/2T}^{1/2T}X(f)X^*(f)df = T\int_{-1/2T}^{1/2T}|X(f)|^2df
$$
Quindi l'energia di un segnale discreto si può calcolare integrando nel periodo il modulo quadrato della sua [[Trasformata Discreta di Fourier|trasformata discreta]].
$$
E_x=T\int_{-1/2T}^{1/2T}|X(f)|^2df
$$