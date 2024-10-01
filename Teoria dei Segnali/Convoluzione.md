La **convoluzione** è un'operazione tra due [[Segnale|segnali]].
$$\large z(t) = x(t) * y(t) = \int_{-\infty}^{\infty} x(\tau) \cdot y(t-\tau)\ d\tau$$
La variabile di uscita è $t$.
## Proprietà
### Commutativa
$$z(t)\ =\ x(t) * y(t)\ =\ y(t) * x(t)$$
### Associativa
$$z(t)\ =\ x(t) * [y(t) * w(t)]\ =\ [x(t) * y(t)] * w(t)$$
### Distributiva
$$z(t)\ =\ x(t) * [y(t) + w(t)]\ =\ [x(t) * y(t)] + [x(t) * w(t)]$$
## Esercizi
### Convoluzione tra [[Segnale Esponenziale|esponenziale unilatero]] e [[Segnale Gradino|gradino]]

Dati due segnali $x(t) = e^{-\alpha t}u(t)$ e $y(t) = u(t)$, la loro convoluzione vale
$$\large z(t) = \int_{-\infty}^{\infty} e^{-\alpha\tau}u(\tau) \cdot u(t-\tau)\ d\tau$$
>[!info] Grafici dei segnali dentro l'integrale
>Per poter graficare queste funzioni è stato scelto arbitrariamente il parametro $\alpha = 0.5$ e $t=-2$. 
>
>![[1727789107.png]]
>$$\large x(\tau)\ =\ e^{-0.5\tau}u(\tau)$$
>---
>![[1727789936.png]]
>$$\large y(-2-\tau)=u(-2-\tau)$$

Si può notare che per $t<0$ i segnali non interagiscono (almeno uno di loro è nullo nell'intervallo). 
$$\begin{gather}
z(t) = 0 & \text{per} & t<0
\end{gather}$$
>[!info] Grafico per $\large t=2$
>![[1727795433.png]]

Invece per $t>0$, usando le proprietà del segnale gradino, l'integrale diventa definito in un intervallo limitato e si eliminano i gradini.
$$\large \int_{0}^{t}e^{-\alpha\tau}\cdot 1\ d\tau = \left.\frac{e^{-\alpha\tau}}{-\alpha}\right|_{0}^{t}=\frac{1-e^{-\alpha t}}{\alpha}$$
Quindi si può definire il segnale convoluto a tratti.
$$z(t) = \left\{\begin{matrix*}
0 & \longrightarrow & t < 0\\
\dfrac{1-e^{-\alpha t}}{\alpha} & \longrightarrow & t \ge 0\\
\end{matrix*}\right.$$
