---
tags:
  - segnale
  - funzione
---
Il segnale gradino è una funzione discontinua che vale $0$ per argomenti negativi e $1$ per argomenti positivi.
$$
u(t)=\begin{cases}
1 &  \text{ per } & t\geq0\\
0 &  \text{ per } & t<0
\end{cases}
$$
Un'altra notazione usata è $\delta_{-1}(t)$ per alcune sue [[#Proprietà]].

>[!info] Grafico
>![[segnale_gradino.png]]
## Proprietà
### Derivata
 La sua derivata è una [[Delta di Dirac|delta di Dirac]].
$$
\frac{d}{dt}u(t) = \delta(t)
$$
### Integrale
 Il suo integrale è una [[Segnale Rampa|rampa]].
$$
\int_{-\infty}^{\infty}u(t)\ dt = R(t)
$$
## Gradino tempo-discreto

La variante tempo-discreta del segnale gradino è definita come una funzione su numeri interi che vale $1$ per tutti i numeri interi maggiori o uguali a $0$, altrimenti vale $0$.
$$
u\left[n\right] = \begin{cases}
1 & \text{ per } & n \geq 0 \\
0 & \text{ per } & n < 0
\end{cases}
$$

>[!info] Grafico
>![[gradino_discreto.png]]
