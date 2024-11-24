---
tags:
  - statistica
---
Gli **assiomi di Kolmogorov** sono una parte fondamentale della [[Teoria della probabilità]] e descrivono le proprietà che una probabilità $P(E)$ di un evento $E$ deve soddisfare.
## Primo assioma
La probabilità di un evento è un numero reale non negativo
$$
P(E)\ge0
$$
## Secondo assioma
La probabilità dell'intero spazio degli eventi $\Omega$ è 1
$$
P(\Omega)=1
$$
## Terzo assioma
Dati due eventi disgiunti $A$ e $B$ , la probabilità dell'unione dei due eventi è uguale alla somma delle possibilità dei due eventi
$$
P(A\cup B)=P(A)+P(B)
$$
### Generalizzazione
Questo assioma può essere generalizzato ad una sequenza numerabile di eventi disgiunti
$$
P\left(\bigcup_{i=1}^{\infty} E_i\right)=\sum_{i=1}^{\infty}P(E_i)
$$
>[!info] Considerazioni sugli eventi non disgiunti
>È importante considerare le intersezioni nei casi in cui gli eventi non siano reciprocamente esclusivi.
>
>Come esempio è riportata la probabilità che venga estratto **almeno** un $2$ da $2$ dadi.
>$$
>P_r\set{2}=\left(\begin{array}{c|c}
>& \begin{matrix*}
>1 & 2 & 3 & 4 & 5 & 6  
>\end{matrix*} \\ 
>\hline
>\begin{matrix*}
>1 \\ 2 \\ 3 \\ 4 \\ 5 \\ 6  
>\end{matrix*} & \begin{matrix*}
>0 & 1 & 0 & 0 & 0 & 0 \\
>1 & 1 & 1 & 1 & 1 & 1 \\
>0 & 1 & 0 & 0 & 0 & 0 \\
>0 & 1 & 0 & 0 & 0 & 0 \\
>0 & 1 & 0 & 0 & 0 & 0 \\
>0 & 1 & 0 & 0 & 0 & 0 \\
>\end{matrix*}
>\end{array}\right) = \frac{11}{36}
>$$
