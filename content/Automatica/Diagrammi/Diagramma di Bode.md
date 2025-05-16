---
tags:
  - sistema
  - analisi
  - controllo
  - proprietà
---
Il diagramma di Bode è usato per studiare il comportamento di modulo e fase della funzione di trasferimento al variare della frequenza dell'ingresso sinusoidale. Questo diagramma è quindi usato nell'analisi dei sistemi nel [[Regime Permanente Sinusoidale]].

Per poter essere utilizzato, la funzione di trasferimento deve essere prima normalizzata per poter ricavare il guadagno generalizzato $K_G$.
$$
G(S) = \frac{K_G\,s^h\,(s\tau_1+1)(s\tau_2+1)\dots\left(\frac{\omega^2}{\omega^2_n}+\frac{2\zeta}{\omega_n}s+1\right)\dots}{(s\tau_3+1)(s\tau_4+1)\dots\left(\frac{\omega^2}{\omega^2_n}+\frac{2\zeta}{\omega_n}s+1\right)\dots}
$$
In questa espressione tutti i termini sono moltiplicati tra loro e questo semplifica il calcolo del modulo e fase di $G(j\omega)$. 

La fase di una divisione diventa una sottrazione di fasi e la fase di un prodotto diventa una somma di fasi.
$$
\begin{align*}
\angle G(j\omega) &= \angle K_G+\angle (j\omega)^h +\angle(j\omega\tau_1+1)+\angle(j\omega\tau_2+1)+\dots+\\
&-\angle(j\omega\tau_3+1)+\angle(j\omega\tau_4+1)-\dots
\end{align*}
$$
L'andamento complessivo della fase è pari alla somma dell'andamento dei singoli termini.

Il modulo di un prodotto è il prodotto dei moduli, il che è comunque un'operazione complicata. Per questo si usa una scala logaritmica per trasformare questi prodotti in delle somme.
$$
20\log_{10}|a\cdot b| = 20\log_{10}|a| + 20\log_{10}|b|
$$
### Guadagno generalizzato
#### Modulo
$$
|Kg| = 20\log_{10}|K_G|
$$
#### Fase
$$
\angle Kg =
\begin{cases}
\hphantom{-18}0 & \text{per } K_G > 0\\
-180 & \text{per } K_G < 0\\
\end{cases}
$$
