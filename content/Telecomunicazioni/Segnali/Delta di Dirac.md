---
tags:
  - segnale
---
La funzione Delta di Dirac (anche detto impulso unitario o matematico), è una funzione che vale 1 nell'origine e 0 altrove.
$$
\delta (t) = \begin{cases}
1 & \rightarrow & t = 0 \\
0 & \rightarrow & altrove
\end{cases}
$$
>[!info] Grafico 
>![[dirac_grafico.svg]]
## Definizione
La Delta di Dirac può essere definita come una distribuzione.

>[!summary] Distribuzione uniforme e normale
>$$
>\begin{align*}
>\text{distribuzione normale} \qquad y_a(x)&= \frac{1}{|a|\sqrt\pi}\ e^{\large \frac{x}{a}^2}\\
>\text{distribuzione uniforme} \qquad y_a(x)&= \begin{cases}
>\small\dfrac{1}{2a} & \rightarrow & -a < x < a \\
>0 & \rightarrow & altrove
>\end{cases}
>\end{align*}
>$$
>![[distribuzioni_unitarie.png|center]]
>$$
>\text{Grafico delle distribuzioni per } a=0.5
>$$

Da qui si può vedere come, ponendo $a \rightarrow 0$, l'andamento può essere approssimato come una funzione che vale $+\infty$ nell'origine e 0 altrimenti.
$$
\delta (x) \simeq
\begin{cases}
+\infty \ & \rightarrow\ x=  0 \\
\hphantom{+}0 \ & \rightarrow\ x \neq 0
\end{cases}
$$
A questa approssimazione si aggiunge un'altra proprietà da rispettare, ovvero che il suo integrale valga sempre 1, come le distribuzioni di partenza. 
$$
\int_{-\infty}^{\infty}{\delta(t)\ dx} = 1
$$
Da qui di ottiene la definizione della funzione Delta di Dirac, che non è una funzione nel senso tradizionale del termine.
### Relazione con la funzione gradino
La funzione  Delta di Dirac è la derivata della [[Segnale Gradino|funzione gradino]]. Ponendo una costante $a \in \mathbb{R}$ e una funzione a valori reali $y(t)$, si ottiene questo integrale.
$$
\int y(t)\ \delta(t-a)\ dt\ =\ y(a)\ u(t-a) + C
$$
## Proprietà
### Traslazione
$$
\delta (t-T) = \Biggl\{\begin{matrix}
\begin{align*}
1 \ & \rightarrow\ t = T \\
0 \ & \rightarrow\ altrove
\end{align*}
\end{matrix}
$$
>[!info] Grafico per $T = 1$
>![[dirac_ritardata.png]]

Facendo la [[Convoluzione]] tra la Delta di Dirac tempo-ritardata $\delta(t-T)$ e una funzione $f(t)$ si avrà la funzione valutata al tempo T.
$$
\int_{-\infty}^{\infty}f(t)\cdot\delta(t-T)\ dt = f(T)
$$
### Prodotto per uno scalare
$$
\int_{-\infty}^{\infty}a\ \delta(t)\cdot\phi(t)\ dt = a\int_{-\infty}^{\infty}\delta(t)\phi(t)dt
$$
### Cambio di scala
$$
\delta(at) = \frac{1}{|a|}\delta(t)\quad \text{per}\ a \neq 0
$$
### Convoluzione
$$
\nu(t)*\delta(t-\tau) = \nu(t-\tau)
$$
### Campionamento
$$
\int_{-\infty}^{\infty}\nu(t)\cdot\delta(t-\tau)\ dt=\nu(\tau)
$$
## Delta di Kroneker

La Delta di Kroneker è la versione discreta della Delta di Dirac, infatti questa funzione è definita solo per i numeri interi.
$$
\delta[n] = \begin{cases}
1 & \rightarrow & n = 0 \\
0 & \rightarrow & altrove
\end{cases}
$$
>[!info] Grafico
>![[kroneker_grafico.png]]

La Delta di Kroneker mantiene le stesse proprietà della Delta di Dirac, infatti anche la sua area è uguale a 1.
$$
\sum_{n=-\infty}^{\infty} \delta[n] = 1
$$
