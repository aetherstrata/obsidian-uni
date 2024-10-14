---
tags:
  - segnale
  - sistema
---
La risposta impulsiva è la funzione caratterizzante di un [[Filtro|filtro]] e descrive il suo comportamento in risposta ad un [[Delta di Dirac|impulso]]. 
$$\delta(t) \to\boxed{\text{ sistema }}\to h(t)$$
## Esempi

Data la riposta impulsiva $h(t) = 2\delta(t-2) + u(t-2)$, calcolare l'uscita del filtro quando è applicato in ingresso un gradino $u(t)$.
$$y(t) = u(t)*[2\delta(t-2) + u(t-2)] = [u(t)*2\delta(t-2)] + [u(t)*u(t-2)]$$
Applicando la proprietà distributiva, si ottengono due convoluzioni elementari da risolvere:

- Per la convoluzione con la [[Delta di Dirac]] basta traslare la funzione.
$$u(t)*2\delta(t-2) = 2u(t-2)$$
- Per la convoluzione con il [[Segnale Gradino|gradino]] il risultato sarà una [[Segnale Rampa|rampa]].
$$\begin{align}
u(t)*u(t-2) &= \int_{-\infty}^{\infty}u(\tau)u(t-\tau-2)\ d\tau = \int_{0}^{\infty}u(t-\tau-2)\ d\tau=\\
&= \int_{0}^{t-2}\ d\tau = \tau\Big|_{0}^{t-2}=(t-2)u(t-2)
\end{align}$$
L'uscita seguirà quindi la funzione $y(t) = 2u(t-2) + (t-2)u(t-2) = t\cdot u(t-2)$.
