---
tags:
  - sistema
  - analisi
  - stabilità
---
Il criterio di Routh stabilisce alcune condizioni da rispettare per assicurarsi che il sistema sia stabile.

Per essere stabile, tutte le radici del polinomio caratteristico dio un sistema devono giacere nel semipiano sinistro, ovvero devono avere parte reale negativa.
$$
\forall p,\ \Re\{p\}<0
$$
>[!info] Grafico
> ![[1747248920.png]]
> 
> Se la radice dovesse avere parte reale positiva, la sua evoluzione non si annullerebbe nel tempo.

## Condizione necessaria
La condizione necessaria per determinare la stabilità di un sistema è che tutti i segni dei coefficienti delle radici devono essere uguali.
$$
\begin{align*}
s^4 + 3s^3 -5s^2 + s + 2 &\quad\text{instabile} \\
s^4 + 4s^3 + 2s^2 + s + 1&\quad\text{c.n. soddisfatta}
\end{align*}
$$
## Condizione sufficiente
La condizione sufficiente affinché un sistema sia stabile è che tutti i coefficienti sulla prima colonna della tabella di Routh abbiano lo stesso segno.

Si comincia inserendo nelle prime due righe della tabella i coefficienti delle radici, dall'alto in basso, da sinistra a destra.
$$
s^4 + 2s^3 + 3s^2 + 10s + 8 \quad \longrightarrow \quad \begin{array}{r|l}
\begin{matrix}
1 \\ 2 \\ 3 \\ 4 \\ 5
\end{matrix} & \begin{matrix}
1 & 3 & 8\\
2 & 10\\ \\ \\ \\
\end{matrix}
\end{array}
$$
Per colmare le righe sottostanti si calcola il determinante tra i due coefficienti soprastanti della prima colonna e quelli della colonna a destra di quella interessata. Infine si divide il valore per l'ultimo valore della prima colonna.
$$
\begin{array}{r|l}
\begin{matrix}
1 \\ 2 \\ 3 \\ 4 \\ 5
\end{matrix} & \begin{matrix}
1 & 3 & 8\\
2 & 10\\ \\ \\ \\
\end{matrix}
\end{array}
\quad \longrightarrow \quad 
\begin{array}{r|l}
\begin{matrix}
1 \\ 2 \\ 3 \\ 4 \\ 5
\end{matrix} & \begin{matrix}
\hphantom{-}1 & 3 & 8\\
\hphantom{-}2 & 10 & 0\\ 
-2 & 8 & 0\\ 
18 & 0\\
\hphantom{-}8\\
\end{matrix}
\end{array}
$$
I coefficienti nella prima colonna non hanno tutti lo stesso segno, quindi il sistema non avrà un andamento stabile. Si possono contare i cambiamenti di segno per ottenere quante radici giacciono nel semipiano destro. In questo caso sono $2$.