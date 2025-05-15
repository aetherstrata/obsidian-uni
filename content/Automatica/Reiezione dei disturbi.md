---
tags:
  - sistema
  - controllo
---
Sulla reiezione dei disturbi si può applicare lo stesso ragionamento usato per studiare il comportamento in [[Regime Permanente Polinomiale]]. Un disturbo è un tipo di ingresso non misurabile che causa una variazione indesiderata nell'uscita.

![[Pasted image 20250515212809.png]]

Si comincia calcolando la funzione di trasferimento rispetto al disturbo $Z(S)$ in forma canonica.
$$
W_Z(S)=\frac{G_2(S)}{1+G_1(S)G_2(S)H(S)} \quad \text{con} \quad Z(S) = \frac{1}{s^{k+1}}
$$
Per ottenere il valore dell'uscita in $t\to\infty$ si può applicare il [[Trasformata di Laplace#Teorema del valor finale|Teorema del valor finale]] e porlo uguale a $0$ per annullare il disturbo.
$$
\begin{align*}
\lim_{s\to0}sW_Z(S)Z(S) &= s\,\frac{G_2(S)}{1+G_1(S)G_2(S)H(S)}\,\frac{1}{s^{k+1}} =\\
&= \frac{G_2(S)}{1+G_1(S)G_2(S)H(S)}\,\frac{1}{s^{k}} = 0
\end{align*}
$$
L'unico modo per interagire con il disturbo è modificare le funzioni che precedono l'ingresso del disturbo, quindi in questo caso $G_1(S)$. Da questa si possono separare i suoi integratori.
$$
G_1(S) = \frac{G_1'(s) }{s^h}
$$
Ponendo $G_1'(0) = K_{G1}$ e $G_2(0) = K_{G2}$, si ottiene la relazione tra l'errore in uscita e i gradi del polinomio.
$$
\begin{align*}
\lim_{t\to\infty}e(t) &= \lim_{s\to0} \frac{G_2(S)}{1+\frac{G_1'(s) }{s^h}G_2(S)H(S)}\,\frac{1}{s^{k}} =\\
&= \lim_{s\to0} \frac{G_2(S)}{s^h+G_1'(s)G_2(S)H(S)}\,s^{h-k} = 0\\
&= \lim_{s\to0} \frac{K_{G2}}{s^h+K_{G1}K_{G2}H(S)}\,s^{h-k} = 0
\end{align*}
$$
- $h>k \to e_Z = 0$
- $h<k \to e_Z = \infty$
- $h=k \to e_Z = \dfrac{K_{G2}}{K_{G1}K_{G2}\frac{1}{K_d}}$
Quindi, per azzerare il disturbo, bisogna progettare il sistema in modo che sia nel caso h>k, cioè che abbia almeno un integratore a monte in più del grado del disturbo. Un sistema che rigetta un disturbo costante è detto astatico.
$$
\begin{array}{r|ccc}
h \\ k\ \ \ \ \ \
& 0 & 1 & 2 \\ 
  \hline
0\ \ & \delta_{-1} &  0 & 0 \\ 
1\ \ & \delta_{-2} & \delta_{-1} & 0 \\ 
2\ \ & \delta_{-3} & \delta_{-2} & \delta_{-1} \\ 
\end{array} 
$$
Quindi ogni integratore nella catena diretta riduce il tipo del disturbo di un grado.