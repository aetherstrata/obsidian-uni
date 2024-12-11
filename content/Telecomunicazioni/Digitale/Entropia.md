---
tags:
  - segnale
  - digitale
  - proprietà
---
L'entropia di un messaggio misura la quantità di informazioni trasmesse.
## Informazione
L'entropia associata ad un singolo evento, con probabilità $p_i$, è definita secondo la seguente formula.
$$
I=\log_2\frac{1}{p_i}=-\log_2p_i
$$
Significa quindi che l'informazione di un evento può essere vista come la quantità di cifre (solitamente in base $2$) da utilizzare per distinguere l'evento accaduto da tutti gli altri eventi possibili. 

Il logaritmo diventa indispensabile se, considerando due eventi indipendenti la cui probabilità è data dal prodotto delle singole probabilità, si vuole che l'entropia totale sia la somma delle entropie dei singoli eventi.
$$
I_{ab}=\log_2\frac{1}{p_ap_b}=\log_2\frac{1}{p_a}+\log_2\frac{1}{p_b}=I_a+I_b
$$
## Sorgente
L'entropia associata ad una sorgente di informazioni è la somma pesata delle informazioni dei simboli.
$$
H=\sum_{i}^{M}p_iI_i=-\sum_{i}^{M}p_i\log_2p_i
$$
Se tutti gli eventi sono equamente probabili allora si può semplificare.
$$
H=\sum_{i}^{M}I_i=-\sum_{i}^{M}\log_2p_i
$$