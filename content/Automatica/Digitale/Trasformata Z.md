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
