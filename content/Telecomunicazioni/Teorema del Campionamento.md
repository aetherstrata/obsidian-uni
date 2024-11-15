---
tags:
  - segnale
  - teorema
---
Il teorema del campionamento definisce la frequenza di campionamento minimo affinché il [[Segnale]] analogico possa essere ricostruito senza perdita di informazioni.

Il [[Segnale Campionato]] ha lo stesso spettro del segnale analogico, ma ripetuto per il tutto il dominio, quindi per riottenere lo spettro del segnale analogico basta avere un [[Filtro]] passa-basso.
$$
x_c[n]\longrightarrow\underset{\mathclap{H(f)=T\operatorname{rect}(Tf)}}{\boxed{\vphantom{\int}\text{ DAC }}}\longrightarrow x(t)
$$
