---
tags:
  - sistema
  - controllo
---
Un regolatore PID è una tipologia di controllori industriali molto usata e diffusa, con un'implementazione molto semplice.

![[pid_diag.png]]

Questo controllore è formato da tre componenti, *Proporzionale* - *Integrativo* - *Derivativo*, solitamente sviluppate in parallelo, con diversi coefficienti, che possono assumere diverse forme.
$$
\begin{align*}
P\left(1+I\frac{1}{s} + D\frac{N}{1+N\frac{1}{s}}\right) &\quad \text{Forma ideale}\\
P+I\frac{1}{s} + D\frac{N}{1+N\frac{1}{s}} &\quad \text{Forma parallela}
\end{align*}
$$
Il controllore PID ha una componente derivativa, quindi non sarebbe realizzabile, per cui si usa una approssimazione.
## Comportamento
Ogni componente del controllore ha azioni diversi sul sistema e variano con i coefficienti.
### Azione proporzionale
Questa azione è in grado di aumentare la banda passante e ridurre l‘effetto di variazioni parametriche e/o disturbi. Tuttavia riduce i margini di stabilità.
$$
W(S) = \frac{K_pP(S)}{1+K_pP(S)} \quad ; \quad K_p\to\infty\quad\Longrightarrow\quad W(S) \to 1
$$
### Azione integrativa
Azzera l’effetto di disturbi o variazioni parametriche a valle sull'uscita a regime, tuttavia riduce la banda passante e i margini di stabilità.
$$
W(S) = \frac{K_iP(S)}{s+K_iP(S)} \quad ; \quad s\to0 \approxeq t\to\infty\quad\Longrightarrow\quad W(S) \to 1
$$
### Azione derivativa
Prevede l'andamento dell'errore $e(t)$, migliora i margini di stabilità $(+90^{\circ})$ e riduce la sovraelongazione, tuttavia enfatizza le alte frequenze dove spesso sono presenti rumori.