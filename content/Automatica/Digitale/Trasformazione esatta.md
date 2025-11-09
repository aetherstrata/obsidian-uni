---
tags:
  - sistema
  - controllo
  - digitale
---
Preso un sistema di controllo $C(S)$, si può ricavare la sua antitrasformata $c(t)$.
$$
C(S) = \frac{1}{s-p} \quad\overset{\mathcal{L^{-1}}}{\longrightarrow}\quad e^{pt}
$$
Questo segnale viene campionato con un certo periodo $T_c$.
$$
c_c(t) = \sum_{k=0}^{\infty} e^{pt}\,\delta_0(t-kT_c) =  \sum_{k=0}^{\infty} e^{pkT_c}\,\delta_0(t-kT_c)
$$
Per finire si applica la [[Trasformata Z]].
$$
C(Z) = \sum_{k=0}^{\infty} e^{pkT_c}\,z^{-k} = \sum_{k=0}^{\infty} \left(e^{pT_c}z^{-1}\right)^k \quad\longrightarrow\quad \frac{1}{1-e^{pT_c}z^{-1}}
$$
Per calcolare i loro guadagni si applicano i rispettivi teoremi del valor finale su un'ingresso a gradino.
$$
\begin{align*}
G_s &= \lim_{s\to0} s \frac{1}{s} \frac{1}{s-p} = - \frac{1}{p}\\
G_z &= \lim_{z\to1} \left(1-z^{-1}\right) \frac{1}{1-z^{-1}}  \frac{1}{1-e^{pT_c}z^{-1}} = \frac{1}{1-e^{pT_c}}
\end{align*}
$$
I guadagni sono diversi in quanto la trasformazione è stata fatta attraverso una risposta impulsiva e non a gradino.
## Correzione del guadagno

Per correggere il guadagno si può usare la sintesi diretta, dividendo il controllore per $\dfrac{1}{1-e^{pT_c}}$ e moltiplicandolo per $-\dfrac{1}{p}$.
