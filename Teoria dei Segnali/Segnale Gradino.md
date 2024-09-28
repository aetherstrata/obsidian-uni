---
tags:
  - segnale
  - funzione
---
Il segnale gradino è una funzione discontinua che vale $0$ per argomenti negativi e $1$ per argomenti positivi.
$$u(t)=\Biggl\{\begin{matrix}\begin{align}
1 & \quad \longrightarrow \quad t\geq0\\
0 & \quad \longrightarrow \quad t<0
\end{align}\end{matrix}$$
Un'altra notazione molto usata è $\delta_{-1}(t)$ per alcune sue proprietà.

>[!info] Grafico
>![[segnale_gradino.png]]
## Proprietà
### Derivata

 La sua derivata è una [[Delta di Dirac|delta di Dirac]].
$$\frac{d}{dt}\ \delta_{-1}(t) = \delta(t)$$
### Integrale

 Il suo integrale è una [[Segnale Rampa|rampa]].
$$\int_{-\infty}^{\infty}\delta_{-1}(t)\ dt = \delta_{-2}(t)$$
## Gradino tempo-discreto

La variante tempo-discreta del segnale gradino è definita come una funzione su numeri interi che vale $1$ per tutti i numeri interi maggiori o uguali a $0$, altrimenti vale $0$.
$$u\left[n\right] = \left\{\begin{matrix*}
1 & \longrightarrow & n \geq 0 \\
0 & \longrightarrow & n < 0
\end{matrix*}\right.$$
>[!info] Grafico
>![[gradino_discreto.png]]
