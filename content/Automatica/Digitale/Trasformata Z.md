---
tags:
  - trasformata
  - digitale
  - sistema
  - controllo
---
La trasformata Z è una trasformata integrale, simile alla [[Trasformata di Laplace]], che permette di trasformare funzioni discrete in funzioni più semplici da trattare. In questo ambito si usa per trasformare delle equazioni alle differenze.
$$
Z\{y_k\} = Y_k(Z) = \sum_{k=0}^\infty y_k\,z^{-k}
$$
Per studiare questa sommatoria è utile ricorrere a una proprietà della serie geometrica.
$$
\sum_{i=0}^\infty x^i \quad\longrightarrow\quad \text{converge per }|x|<1 \quad\longrightarrow\quad \frac{1}{1-x}
$$
Si prende come esempio un [[Segnale Esponenziale]] $y(t) = e^{pt}$, che viene poi campionato con un intervallo $T_c$.
$$
y(t) = e^{pt} \quad\longrightarrow\quad y_c(t) = y(t)\sum_{k=0}^\infty\delta_0(t-kT_c) = \sum_{k=0}^\infty e^{pkT_c}\delta_0(t-kT_c)
$$
Ogni valore di questa sequenza vale quindi $e^{pkT_c}$. A questo punto si applica la trasformata Z.
$$
\{y_k\} = e^{pkT_c} \quad\overset{Z}{\longrightarrow}\quad \sum_{k=0}^\infty e^{pkT_c}\,z^{-k}
$$
Raccogliendo l'esponente $k$, questa serie diventa molto simile alla serie geometrica.
$$
\sum_{k=0}^\infty e^{pkT_c}\,z^{-k} = \sum_{k=0}^\infty \left(e^{pT_c}z^{-1}\right)^k \quad\longrightarrow\quad \frac{1}{1-e^{pT_c}z^{-1}}
$$
Quindi si può studiare la regione di convergenza del termine $|e^{pT_c}z^{-1}| < 1$. L'esponenziale è sempre positivo e può essere portato fuori dal modulo. 
$$
\begin{align*}
|z^{-1}|e^{pT_c} &< 1 \\
|z^{-1}| &< e^{-pT_c} \\
|z| &> e^{pT_c}
\end{align*}
$$
La serie converge su tutto il piano fuori dal cerchio di raggio $e^{pT_c}$.
## Relazione con il dominio di Laplace
$$
\begin{align*}
x(t) = e^{pt} &\quad\overset{\mathcal{L}}{\longrightarrow}\quad X(S) = \frac{1}{s-p}\\
x[k] = e^{pkt_c}&\quad\overset{Z}{\longrightarrow}\quad X(Z) = \frac{z}{z-e^{pT_C}}
\end{align*}
$$Il polo che era in $p$ nel dominio continuo si sposta in $e^{pT_c}$ nel dominio discreto.
$$
p \quad\longrightarrow\quad e^{pT_c}
$$
## Gradino
Per avere un gradino unitario si pone $p=0$ e si ha di conseguenza $e^{0t}$.
$$
x[k] = e^{0kt_c} \quad\overset{Z}{\longrightarrow}\quad X(Z) = \frac{z}{z-e^{0T_C}} = \frac{z}{z-1}
$$
A differenza del dominio di Laplace, in cui il gradino ha polo in $0$, nel discreto questo ha polo in $1$.
## Ritardo
Scrivere l'andamento della funzione discreta originale e confrontarla con la stessa funzione moltiplicata per $z^{-1}$
$$
\begin{alignat}{7}
y(z) ={}&& y_0 &&{}+ y_1z^{-1} &&{}+ y_2z^{-2} &&{}+ y_3z^{-3} &&{}+ y_4z^{-4} &&{}+ \cdots \\[4pt]
t ={}&& 0 && T_c\quad && 2T_c\quad && 3T_c\quad && 4T_c\quad &&+ \cdots \\[4pt]
y(z)z^{-1} ={}&&  && y_0z^{-1} &&{}+ y_1z^{-2} &&{}+ y_2z^{-3} &&{}+ y_3z^{-4} &&{}+ \cdots \\[4pt]
\end{alignat}
$$
Questo non fa altro che ritardare di un campione la sequenza, quindi il termine $z^{-1}$ è l'operatore di ritardo unitario.
## Teorema del valore finale
$$
\lim_{z\to1}\left(1-z^{-1}\right)Y(z) = \lim_{k\to\infty}\left\{y_k\right\}
$$