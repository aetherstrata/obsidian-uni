---
tags:
  - statistica
---
## Definizione
### Definizione frequentistica
Definito un evento $E$ infinitamente ripetibile, il rapporto tra il numero delle prove che corrispondono a un risultato favorevole $N_E$ (ad esempio che esca testa su una moneta) e il totale delle prove $N$, è uguale alla probabilità dell'evento $E$.
$$
f_E = \lim_{N\to\infty}\frac{N_E}{N}
$$
In ogni caso, il rapporto $N_E/N$ sarà sempre minore o uguale a $1$.
$$
P_r\{\mathcal{E}\}\le1
$$
>[!tip] Caso $P_r=1$
>La probabilità sarà uguale a $1$ solo quando l'insieme degli eventi favorevoli è uguale allo spazio degli eventi.
## Probabilità condizionata
### Teorema di Bayes
Il teorema di Bayes lega due probabilità con condizione opposta
$$
P(A|B)=\frac{P(A\cap B)}{P(B)} \quad;\quad P(B|A)=\frac{P(A\cap B)}{P(A)} 
$$
Il numeratore dell'espressione è lo stesso e le due probabilità possono essere riscritte nella seguente formula
$$
P(A|B)P(B)=P(B|A)P(A)
$$
### Probabilità composta
La probabilità che si verifichino entrambi gli eventi $A$ e $B$ è pari alla probabilità di uno dei due eventi moltiplicato con la probabilità dell'altro evento condizionato al verificarsi del primo.
$$
P(A \cap B) = P(A)P(B|A) = P(B)P(A|B) 
$$
Nel caso in cui i due eventi siano reciprocamente disgiunti, la probabilità coniugata è pari al prodotto delle probabilità
$$
P(A \cap B) =P(A)P(B)
$$
### Probabilità assoluta
La probabilità che si verifichi un evento $B$, dipendente da un insieme di eventi $A_i$, è la somma di tutte le possibilità composte tra $B$ e uno degli eventi in $A_i$.
$$
P(B)=\sum_{i=1}^{n}P(A_i \cap B) = \sum_{i=1}^{n}P(A_i)P(B|A_i)
$$
## Esempi
### Lancio del dado
Per prima cosa si definiscono gli eventi elementari.
$$
\mathcal{E}=\{1,2,3,4,5,6\}
$$
A ogni evento è associato un vettore nello spazio $\mathbb{R}^n$.
$$
x=(x_1,\dots,x_n)\in\mathbb{R}^n
$$
Con le corrispondenze create ora si può far a meno di utilizzare delle locuzioni per descrivere gli eventi.