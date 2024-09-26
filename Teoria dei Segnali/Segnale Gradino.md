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