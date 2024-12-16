---
tags:
  - modulazione
  - segnale
  - digitale
---
La modulazione di ampiezza in quadratura, anche detta **QAM**, trasmette il segnale in ingresso distribuendolo su due portanti alla stessa frequenza ma sfasate di $90^\circ$, un coseno e un seno, e modulando le loro ampiezze.

Questo metodo permette di trasmettere due segnali sulla stessa frequenza di banda grazie al fatto che sono ortogonali tra loro.
## Descrizione
Il segnale è diviso in due componenti $x_1(t)$ e $x_2(t)$.

Prima di essere trasmesse, le componenti sono moltiplicate per l'oscillatore locale, che trasla il segnale sulla frequenza portante e crea lo sfasamento.
$$
z(t)=x_1(t)\cos(2\pi f_0t)+x_2(t)\sin(2\pi f_0t)
$$
Per demodulare il segnale ricevuto, si applica lo stesso procedimento della trasmissione e si aggiunge un [[Filtro]] passa-basso in cascata.
$$
\begin{align*}
y_1(t)&=2\,z(t)\cos(2\pi f_0t)=\\
&=2\,x_1(t)\cos^2(2\pi f_0t)+2\,x_2(t)\sin(2\pi f_0t)\cos(2\pi f_0t)=\\
&=x_1(t)[1+\cos(4\pi f_0t)]+x_2(t)\sin(4\pi f_0t)=\\
&=x_1(t)+x_1(t)\cos(4\pi f_0t)+x_2(t)\sin(4\pi f_0t)=\\
\boxed{\text{LPF}}\quad&=x_1(t)\\\\
\hline \\
y_2(t)&=2\,z(t)\sin(2\pi f_0t)=\\
&=2\,x_1(t)\cos(2\pi f_0t)\sin(2\pi f_0t)+2\,x_2(t)\sin^2(2\pi f_0t)=\\
&=x_1(t)\sin(4\pi f_0t)+x_2(t)[1-\cos(4\pi f_0t)]=\\
&=x_1(t)\sin(4\pi f_0t)+x_2(t)-x_2(t)\cos(4\pi f_0t)=\\
\end{align*}
$$

Il segnale è quindi diviso in due componenti:
- $I(t)$ è la componente in fase
- $Q(t)$ è la componente di quadratura
