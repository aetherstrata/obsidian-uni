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
## Trasformazioni
Una variabile aleatoria $Y$ può essere legata a un'altra variabile aleatoria $X$ tramite una funzione $f(x)$.
$$
y=f(x) : \mathbb{R}\to\mathbb{R}
$$
Di questa funzione si assume esista la funzione inversa $g(y)$.

Si può ottenere la [[Distribuzione di Probabilità#Distribuzione cumulativa|funzione di ripartizione]] di $Y=f(X)$ in termini della funzione di ripartizione di $X$.
$$
F_Y(y)=P\set{Y\le y}=P\set{f(X)\le y}=P\set{X\le g(y)}=F_X(g(y))
$$
La densità di probabilità si può ottenere calcolando la derivata della funzione composta.
$$
f_Y(y)=F'_Y(y)=F'_X(g(y))=f_X(g(y))\cdot |g'(y)|
$$
Il valore assoluto tiene conto dei casi in cui la funzione $f$ sia decrescente nel dominio di $X$.
## Esercizi
### Esercizio 1
Data una variabile aleatoria $X$ con funzione di densità nota, calcolare la funzione di densità di una variabile aleatoria $Y$ legata a $X$ quando la variabile varia tra $0$ e $\pi$.
$$
P_X(x)=\frac{1}{\pi}\operatorname{rect}\left(\frac{x-\pi/2}{\pi}\right) \quad ; \quad y=\cos(x)
$$
Si definisce $f(x)=y$ la funzione che lega $X$ a $Y$, e $g(y)=x$ la sua funzione inversa, 
$$
g(y)=\arccos(y)\quad\overset{d}{\underset{dy}{\longrightarrow}}\quad g'(y)=\frac{1}{\sqrt{1-y²}}
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
