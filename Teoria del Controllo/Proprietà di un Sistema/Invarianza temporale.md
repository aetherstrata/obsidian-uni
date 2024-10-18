---
tags:
  - proprietà
  - sistema
---
Un [[Sistema|sistema]] si dice **tempo-invariante** se la sua risposta ad un [[Segnale|segnale]] non dipende dalla posizione nel tempo in cui è applicato il segnale di ingresso.
$$\begin{align*}
x(t) &\to y(t) \\
x(t-\tau) &\to y(t-\tau)
\end{align*}$$
## Esempi
### Sistema di [[Modulazione|modulazione]]
Dato un sistema di modulazione si vuole verificare se è tempo-invariante.
$$x(t) \to y(t) = x(t)\cos\left(\frac{2\pi t}{T}\right)$$
Applicando un ritardo $\tau$ all'ingresso, il comportamento del sistema cambia.
$$x(t-\tau) \to x(t-\tau)\cos\left(\frac{2\pi t}{T}\right) \ne y(t-\tau) = x(t-\tau)\cos\left(2\pi\frac{t-\tau}{T}\right)$$
Il sistema non è tempo-invariante.