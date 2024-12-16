---
tags:
  - modulazione
  - segnale
  - digitale
---
La modulazione a codifica di fase, detta anche **PSK**, è un tipo di modulazione passa-banda in cui i simboli del segnale sono codificati nella fase di quadratura della portante.

>[!info] Costellazione 8-PSK
>![[8PSK_Gray_Coded.svg|center|bg-white]]

## Descrizione
Tutti i simboli hanno la stessa ampiezza $A$ e fasi equidistanti tra loro.

Dovendo poter trasmettere $M$ simboli diversi, allora la circonferenza sarà divisa da $M$ angoli uguali di ampiezza $\theta$.
$$
\theta = \frac{2\pi}{M}
$$
Per calcolare la distanza $d$ tra due simboli bisogna ricordare che questa è la lunghezza di una corda tra due punti di una circonferenza.
$$
d=2A\sin\left(\frac{\theta}{2}\right)
$$
I simboli sono, quindi, codificati scegliendo opportunamente la fase iniziale $\phi_m$.
$$
z(t)=A\cos(2\pi f_0t+\phi_m)=A\cos(\phi_m)\cos(2\pi f_0t)-A\sin(\phi_m)\sin(2\pi f_0t)
$$
Per riottenere i simboli si moltiplica il segnale per l'oscillatore locale e si calcola la fase.
$$
\begin{align*}
a_m&=A\cos(\phi_m)\\
b_m&=A\sin(\phi_m)
\end{align*}
$$
