---
tags:
  - sistema
  - controllo
---
Un regolatore PID è una tipologia di controllori industriali molto usata e diffusa, con un'implementazione molto semplice.

![[Pasted image 20250525163432.png]]

Questo controllore è formato da tre componenti, solitamente sviluppate in parallelo, con diversi coefficienti, che possono assumere diverse forme.
$$
\begin{align*}
P\left(1+I\frac{1}{s} + D\frac{N}{1+N\frac{1}{s}}\right) &\quad \text{Forma ideale}\\
P+I\frac{1}{s} + D\frac{N}{1+N\frac{1}{s}} &\quad \text{Forma parallela}
\end{align*}
$$
Il controllore PID ha una componente derivativa, quindi non sarebbe realizzabile, per cui si usa una approssimazione.
## Comportamento
Ogni componente del controllore ha azioni diversi sul sistema