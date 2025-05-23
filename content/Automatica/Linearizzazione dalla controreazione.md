---
tags:
  - sistema
  - controllo
  - proprietà
---
Uno degli effetti operati dalla chiusura in controreazione di un sistema è la linearizzazione della catena diretta all'aumentare del guadagno.
## Dimostrazione
### Sistema statico
La dimostrazione più semplice è con un sistema statico non lineare, nel quale le equazioni
differenziali che lo rappresentano non dipendono dal tempo:

![[Pasted image 20250523201327.png]]
$$
e=u-y\quad ;\quad v=Ke \quad;\quad v+v^3=y
$$
Applicando delle sostituzioni si ottiene il legame tra l'ingresso $u$ e l'uscita $y$.
$$
Ke+(Ke)^3 = K(u-y)+K^3(u-y)^3=y
$$
Si assume un guadagno $K>0$ e si divide tutto per $K^3$
$$
\frac{K(u-y) - y}{K^3} + (u-y)^3 = 0
$$
Se si pone $K \to\infty$ allora il termine a sinistra diventa trascurabile.
$$
(u-y)^3=0 \quad\longrightarrow\quad u-y=0 \quad\longrightarrow\quad u=y
$$
Quindi, partendo da un sistema non lineare statico, chiudendo in controreazione ed
aumentando il guadagno, si può rendere lineare. In questo caso si fa riferimento al guadagno
della catena diretta e, senza farlo tendere ad infinito, la relazione non sarebbe mai potuta
diventare lineare.
### Sistema dinamico
Si considera ora un sistema dinamico non lineare, le cui equazioni differenziali dipendono
dal tempo. In particolare si vuole analizzare un pendolo con un guadagno in velocità molto
elevato.
![[Pasted image 20250523202431.png|center|300]]
$$
u(t) = Md^2\ddot\theta + Mgd\sin\theta\qquad \text{(smorzamento trascurato)}
$$
Si vuole pilotare il sistema ad una velocità desiderata $v$. Allora il feedback sarà
$$
H(S)=\frac{1}{\epsilon} \quad\longrightarrow\quad u(t) = \frac{1}{\epsilon}(v-\dot\theta)
$$
dove $\dfrac{1}{\epsilon}$ è il guadagno del feedback e $\dot\theta$ è la velocità del pendolo.
$$
\begin{align*}
Md^2\ddot\theta + Mgd\sin\theta &= \frac{1}{\epsilon}(v-\dot\theta) \\
\epsilon Md^2\ddot\theta + \epsilon Mgd\sin\theta &= v-\dot\theta \\
\end{align*}
$$
Quando $\epsilon\to0$, quindi $\dfrac{1}{\epsilon}\to\infty$, i termini a sinistra dell'uguale si annullano.
$$
\begin{align*}
0 &= v-\dot\theta \\
v &= \dot\theta
\end{align*}
$$
Quindi, anche nel caso di sistemi dinamici non lineari, un alto guadagno può riportare la
linearità.

>[!warning] Stabilità
>Una domanda da porsi quando si applica questa tecnica è se, ponendo $\epsilon=0$, il sistema continuerà ad essere stabile. In questo caso il pendolo rimarrà stabile poiché le dinamiche cancellate erano tutte stabili.
