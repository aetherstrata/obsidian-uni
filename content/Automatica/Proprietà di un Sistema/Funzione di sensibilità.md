---
tags:
  - analisi
  - sistema
  - controllo
---
La funzione di sensibilità serve a studiare come varia l'andamento della funzione di trasferimento di un [[Sistema]] a ciclo chiuso $W(S)$ al variare di un parametro.

Presa una funzione $Q$ ed un parametro $\alpha$, la funzione di trasferimento $S^Q_\alpha$ indica quanto la funzione $Q$ sia sensibile alle variazioni del parametro $\alpha$.
$$
S^Q_\alpha \coloneqq \frac{dQ/Q}{d\alpha/\alpha} = \frac{\alpha}{Q}\frac{dQ}{d\alpha}
$$
## Dimostrazione
Posto un sistema dove la funzione in catena diretta $G(S) = \dfrac{1}{ps+3}$ dipende da un parametro $p$ 

![[Pasted image 20250521163119.png]]

La funzione di sensibilità di $W(S)=\dfrac{G(S)}{1+G(S)H(S)}$ sarà uguale a
$$
S^W_p = \frac{p}{W}\frac{dW}{dp}
$$
Con una manipolazione algebrica si può ottenere la funzione di sensibilità rispetto a $G(S)$.
$$
S^W_p = \frac{p}{W}\frac{dW}{dp} \ \to\ S^W_p = \frac{p}{W}\frac{dW}{dp}\frac{G}{G}\frac{dG}{dG} = \left(\frac{G}{W}\frac{dW}{dG}\right)\left(\frac{p}{G}\frac{dG}{dp}\right) = S^W_G\cdot S^G_p
$$
Quindi la funzione di sensibilità è componibile da due sotto funzioni: una funzione che descrive
la sensibilità di $G(S)$ rispetto al parametro $p$ ed una, più rilevante per le nostre analisi, chiamata
_sensibilità diretta_ $S^W_G$ , la quale descrive la sensibilità di $W(S)$ al variare di $G(S)$.

È utile definire anche la _sensibilità inversa_ $S^W_H$ , la quale descrive la sensibilità di $W(S)$ al variare
di $H(S)$.
$$
\begin{align*}
S^W_G(S) &= \frac{G(S)}{W(S)}\frac{dW(S)}{dG(S)} = \frac{1}{1+G(S)H(S)} \\\\
S^W_H(S) &= \frac{H(S)}{W(S)}\frac{dW(S)}{dH(S)} = -\frac{G(S)H(S)}{1+G(S)H(S)}
\end{align*}
$$
La _sensibilità diretta_ $S^W_G$ tende a $0$ per guadagni di anello elevati.
La _sensibilità inversa_ $S^W_H$ tende a $1$ per guadagni di anello elevati.
Questo significa che queste sono misure proporzionali, è impossibile averle entrambe basse, e all'aumentare di una corrisponde il diminuire dell'altra.