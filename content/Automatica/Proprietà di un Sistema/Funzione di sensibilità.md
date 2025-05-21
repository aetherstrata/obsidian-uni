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

## Sensibilità ai disturbi
Da queste osservazioni si possono dedurre importanti informazioni sulla reiezione dei disturbi.

![[Pasted image 20250521183935.png]]

Per quanto riguarda il disturbo in catena diretta $z_1$, l'uscita è calcolata come
$$
Y(S)=\frac{G_2(S)}{1+G_1(S)G_2(S)H(S)}Z_1(S) = S^W_G \cdot G_2(S)Z_1(S)
$$
Quindi l'effetto del disturbo $z_1$ segue le variazioni della sensibilità $S^W_G$. 
$$
S^W_G\to0 \quad;\quad S^W_G \cdot G_2(S)Z_1(S) \to0
$$
Allora, più la sensibilità di $W(S)$ rispetto a $G(S)$ tende a $0$, più l'effetto del disturbo sull'uscita sarà basso.

Invece, rispetto al disturbo in controreazione $z_2$, non si riesce quasi mai ad essere attenuato.