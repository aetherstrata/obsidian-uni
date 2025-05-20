---
tags:
  - sistema
  - controllo
  - proprietà
---
I sistemi a fase non minima sono sistemi di controllo che, a ciclo aperto, hanno uno o più zeri
e/o poli che giacciono nel semipiano positivo, cioè sono a parte reale positiva. Per i sistemi con poli a parte reale positiva l‘instabilità è certa, tuttavia per sistemi che presentano solo zeri a parte
reale positiva si può ottenere la stabilità.

Il problema principale di questi sistemi è che la fase tende a scendere sotto i $-180^{\circ}$, non essendoci termini che permettono di farla risalire, cioè poli a parte reale negativa.
## Comportamento

Preso un sistema a controreazione, in cui si ipotizza che in $N(S)$ siano presenti zeri a parte reale positiva, si devono calcolare i poli della funzione $W(S)$ al variare di $K$.

![[1747753429.png]]
$$
W(S) = \frac{G(S)}{1+G(S)} = \frac{K\cdot\frac{N(S)}{D(S)}}{1 + K\cdot\frac{N(S)}{D(S)}} = \frac{K\cdot N(S)}{D(S) + K\cdot N(S)}
$$
Studiando il comportamento al variare di $K$ si ottengono questi risultati:
$$
\lim_{K\to0}W(S)  = \lim_{K\to0} \frac{K\cdot N(S)}{D(S) + K\cdot N(S)} = \frac{1}{D(S)}
$$
Per $K\to0$, i poli di $W(S)$ saranno pari alle radici di $D(S)$, che non sono mai a parte reale positiva; quindi il sistema è stabile.
$$
\lim_{K\to\infty}W(S)  = \lim_{K\to\infty} \frac{K\cdot N(S)}{D(S) + K\cdot N(S)} 
$$
Per $K\to\infty$, il termine $D(S)$ sarà trascurabile, e quindi i poli di $W(S)$ saranno pari alle radici di $N(S)$, che sono a parte reale positiva quindi il sistema è instabile.

Quindi, per guadagni elevati, c'è il rischio di spostare i poli nel semipiano destro e farli diventare a parte reale positiva, il che implica un’instabilità sicura.