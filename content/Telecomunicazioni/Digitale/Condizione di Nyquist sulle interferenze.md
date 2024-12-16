---
tags:
  - proprietà
  - segnale
  - digitale
---
L'impulso portante deve avere valore unitario nell'origine e nullo nei multipli del tempo di simbolo. Se la funzione $G(t)$ avesse valore non nullo in un istante $nT$, ci sarebbero interferenze tra i simboli da trasmettere. Questo fenomeno è chiamato **ISI** (interferenza intersimbolica).
$$
g(t)=\begin{cases}
1 & t=0 \\
0 & t=nT
\end{cases}
$$
