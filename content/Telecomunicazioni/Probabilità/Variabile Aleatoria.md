---
tags:
  - statistica
---
Una variabile aleatoria è la formalizzazione matematica di un oggetto o quantità che dipende da eventi casuali. 
## Definizione
Una variabile aleatoria è una funzione che associa ad ogni elemento dello spazio campionario $\Omega$ un valore reale $E$.
$$
X:\Omega\to E \subseteq \mathbb{R}
$$
## Esercizi
### Esercizio 1
Data una variabile aleatoria $X$ con funzione di densità nota, calcolare la funzione di densità di una variabile aleatoria $Y$ legata a $X$ quando la variabile varia tra $0$ e $\pi$.
$$
P_X(x)=\frac{1}{\pi}\operatorname{rect}\left(\frac{x-\pi/2}{\pi}\right) \quad ; \quad y=\cos(x)
$$
Si definisce $f(x)=y$ la funzione che lega $X$ a $Y$, e $g(y)=x$ la sua funzione inversa, 
$$
g(y)=\arccos(y)\quad\longrightarrow\quad g'(y)=\frac{1}{\sqrt{1-y²}}
$$
A questo punto si può applicare una trasformazione di variabile aleatoria.
$$
\begin{align*}
P_Y(y)&=P_X(g(y))\cdot |g'(y)| =\\
&=P_X(\arccos(y))\cdot\frac{1}{\sqrt{1-y²}}=\\
&=\frac{1}{\pi}\operatorname{rect}\left(\frac{\arccos(y)-\pi/2}{\pi}\right)\cdot\frac{1}{\sqrt{1-y²}}
\end{align*}
$$
Studiando il comportamento dell'arcoseno nella finestra si può ricavare il dominio della variabile in $y$.
$$
\begin{align*}
-\frac{1}{2}&<\frac{\arccos(y)-\pi/2}{\pi}<\frac{1}{2} \\
-\frac{\pi}{2}&<\arccos(y)-\pi/2<\frac{\pi}{2} \\
\vphantom{\int}0\,&<\arccos(y)<\pi \\
-1\,&<y<1
\end{align*}
$$
La funzione densità di $Y$ è dunque la seguente
$$
P_Y(y)=\frac{1}{\pi}\operatorname{rect}\left(\frac{y}{2}\right)\cdot\frac{1}{\sqrt{1-y²}}
$$
