---
tags:
  - proprietà
  - sistema
---
Dati due [[Segnale|segnali]] di ingresso $x_1$ e $x_2$, a cui corrispondono rispettivamente le uscite $y_1$ e $y_2$, il [[Sistema|sistema]] è lineare se a una combinazione lineare degli ingressi corrisponde una combinazione lineare delle uscite.
$$
\begin{align*}
x_1(t) &\to y_1(t)\\
x_2(t) &\to y_2(t) \\ \hline
ax_1(t) + bx_2(t) &\to ay_1(t)+by_2(t)
\end{align*}
$$
## Esempi
### Amplificatore costante
Si assume un sistema ideale che moltiplica gli ingressi per una costante $A$. 
$$
\begin{align*}
x_1(t) \to y_1(t) &= A\cdot x_1(t) \\
x_2(t) \to y_2(t) &= A\cdot x_2(t) \\
\end{align*}
$$

Si può dimostrare che questo sistema è lineare se questo comportamento è valido anche per la combinazione lineare degli ingressi.
$$
ax_1(t) + bx_2(t) \to A\left(ax_1(t)+bx_2(t)\right) = ay_1(t) + by_2(t)
$$
Il sistema è **lineare**.
### Amplificatore quadratico
Si assume un sistema ideale che restituisce il quadrato del segnale di ingresso $\left|x(t)\right|^2$.
$$
\begin{align*}
x_1(t) \to y_1(t) &= \left|x_1(t)\right|^2 \\
x_2(t) \to y_2(t) &= \left|x_2(t)\right|^2 \\
\end{align*}
$$
Calcolando l'uscita di una combinazione lineare di ingressi si può notare che il comportamento del sistema è diverso.
$$
\begin{align*}
ax_1(t) + bx_2(t) \to \left|ax_1(t)+bx_2(t)\right|^2 &\ne a\left|x_1(t)\right|^2 + b\left|x_2(t)\right|^2 =\\
&= ay_1(t)+by_2(t)
\end{align*}
$$
Il sistema è **non lineare**.
### Somma con una costante
Si assume un sistema ideale che aggiunte una certa quantità costante $A$ ai segnali in ingresso.
$$
\begin{align*}
x_1(t) \to y_1(t) &= x_1(t) + A \\
x_2(t) \to y_2(t) &= x_2(t) + A \\
\end{align*}
$$
Verificando che il comportamento sia lo stesso anche per le combinazioni lineari di segnali, si nota che l'uscita non è la stessa.
$$
\begin{align*}
ax_1(t) + bx_2(t) \to ax_1(t)+bx_2(t) + A &\ne ax_1(t) +bx_2(t) + 2A =\\
&= ay_1(t) + by_2(t)
\end{align*}
$$
Il sistema è **non lineare**.
