---
tags:
  - sistema
  - controllo
  - proprietà
---
In alcuni sistemi è possibile avere, nella catena di controreazione o in quella diretta, un ritardo finito, il quale fa in modo che il segnale di ingresso esca dall'uscita dopo un certo intervallo di tempo $\Delta T \ne 0$. L’uscita è quindi traslata nel tempo rispetto all'ingresso di una quantità $T$ che
rappresenta il ritardo.

Questo si traduce in un esponenziale aggiuntivo nel dominio di Laplace.
$$
\mathcal{L}\{f(t-T)\}=e^{-Ts}\mathcal{L}\{f\}\quad\longrightarrow\quad F'(S) = F(S)e^{-Ts}
$$
## Comportamento

Per comprendere come questo esponenziale altera il comportamento della funzione originale,
si può studiare la risposta a [[Regime Permanente Sinusoidale]].
$$
e^{-sT}\quad\overset{s=j\omega}{\longrightarrow}\quad e^{-j\omega T}
$$
### Modulo
Il modulo vale $1$, quindi $0$ dB. Di conseguenza influisce sull'andamento dei moduli della $F(S)$.
### Fase
La fase vale $−\omega T$, quindi scende linearmente con $\omega$. Essendo la scala di $\omega$ logaritmica,
ovvero con $\omega$ che cresce esponenzialmente su scala lineare, allora $-\omega T$ scenderà in
maniera esponenziale.
### Andamento
Analizzando il sistema, si possono avere due situazioni:
1. La fase del ritardo decresce esponenzialmente solo dopo l’omega di taglio $\omega_t \to$ In questo caso non va ad alterare negativamente il sistema.
2. La fase del ritardo decresce esponenzialmente prima dell’omega di taglio $\omega_t \to$ Questo fa sì che il margine di fase 𝑚𝜑 diminuisca, rischiando di farlo avvicinare troppo, se non superare, la linea dei -180° rendendo il sistema instabile.

Quindi, maggiore è il ritardo $T$, prima scenderà l’esponenziale, rischiando di portare il sistema
all'instabilità. Si potrebbe ristabilizzare spostando l’omega di taglio $\omega_t$ verso sinistra, cioè abbassando il guadagno, ma questo compromette la qualità del sistema di controllo sia dal punto di vista della riproduzione dei segnali, sia da quello della reiezione dei disturbi.