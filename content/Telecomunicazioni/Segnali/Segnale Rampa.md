---
tags:
  - segnale
  - funzione
---
Il segnale rampa è una funzione discontinua che vale $0$ per argomenti negativi e $x$ per argomenti positivi.
$$
R(x) = \begin{cases}
x & \text{ per } & x\ge0 \\
0 & \text{ per } & x<0
\end{cases}
$$
Un'altra notazione usata è $\delta_{-2}(t)$ per alcune sue [[#Proprietà]].

>[!summary] Composizione di gradini
>Si può creare un segnale rampa moltiplicando l´argomento per un [[Segnale Gradino|gradino]].
>$$
>R(t)=t\cdot u(t)
>$$
## Proprietà
### Derivazione
La sua derivata è il [[Segnale Gradino]].
$$
\frac{d}{dt}R(t)=u(t)\quad\text{se}\quad x\ne0
$$
