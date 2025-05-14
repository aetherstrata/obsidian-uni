---
tags:
  - trasformata
  - sistema
  - operatore
---
La trasformata di Laplace è un operatore integrale che associa ad una funzione a variabile reale una funzione a variabile complessa.
$$
\mathcal{L}\{f\}=\int^{+\infty}_{-\infty}f(t)\,e^{-st}\,dt
$$
Nell'ingegneria dei sistemi la trasformata è usata nella sua versione unilatera.
$$
\mathcal{L}\{f\}=\int^{\infty}_{0}f(t)\,e^{-st}\,dt
$$
Dove $s$ è un numero complesso espresso come $\sigma\pm i\omega$.
## Proprietà
### Linearità
Ad una combinazione lineare di funzioni corrisponde una combinazione lineare di funzioni nel dominio di Laplace
$$
\mathcal{L}\left[\sum^n_{i=1}K_i\cdot f_i(t)\right] = \sum^n_{i=1}K_i\cdot \mathcal{L}\{f_i(t)\}
$$
### Derivazione
$$
\begin{align*}
\mathcal{L}\{f'\} &= s\mathcal{L}\{f\} - f(0^-) \\
\mathcal{L}\{f''\} &= s^2\mathcal{L}\{f\} -sf(0^-) -f'(0^-)\\
\mathcal{L}\{f^{(n)}\} &= s^n \mathcal{L}\{f\} - \sum_{k=1}^n s^{n-k}\frac{d^{k-1}f(0)}{dt^{k-1}}
\end{align*}
$$
### Integrazione
$$
\mathcal{L}\left\{\int_0^tf(\tau)\,d\tau\right\} = \frac{1}{s}\mathcal{L}\{f\}
$$
### Convoluzione
$$
\mathcal{L}\{f*g\} = \mathcal{L}\{f\}\mathcal{L}\{g\}
$$
### Traslazione nel tempo
$$
\mathcal{L}\{f(t-a)\delta_{-1}(t-a)\} = e^{-as}\mathcal{L}\{f\}(s)
$$
### Traslazione nel dominio di Laplace
$$
\mathcal{L}\{e^{-as}f(t)\} = \mathcal{L}\{f\}(s-a)
$$
