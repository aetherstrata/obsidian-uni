---
tags:
  - matematica
  - analisi
  - operatore
---
La divergenza è un operatore vettoriale che opera su un campo vettoriale e restituisce un campo scalare che misura la sua tendenza a divergere o a convergere verso un punto dello spazio. 

In coordinate cartesiane tale quantità è la somma delle derivate parziali delle componenti di F lungo le direzioni degli assi.
$$
\nabla\cdot\mathbf{F}=\frac{\partial F_x}{\partial x}+\frac{\partial F_y}{\partial y}+\frac{\partial F_z}{\partial z}
$$
## Definizione
La divergenza è una quantità scalare che determina la tendenza delle linee di flusso di un campo vettoriale a confluire o divergere da un punto $p$.

Questa quantità può essere misurata considerando una regione di spazio e osservando il flusso del campo vettoriale attraverso la superficie della regione.
- se il flusso è uscente il campo si comporta come se all'interno ci fosse una *sorgente*
- se il flusso è entrante il campo si comporta come se all'interno ci fosse un *pozzo*

La definizione di divergenza di un campo è ottenuta considerando una regione di spazio puntiforme: si tratta del limite, per il volume della regione che tende a zero, del rapporto tra il flusso del campo attraverso la superficie e il volume stesso.
$$
\nabla\cdot\mathbf{F}(p)=\lim_{V\to\{p\}}\frac{1}{V}\int _{S(V)}{\mathbf {F} \cdot \hat{\mathbf{n}}}\;dS
$$
## Teorema della divergenza
Il teorema del gradiente mette in relazione il flusso del campo vettoriale attraverso la superficie della regione e la divergenza sul volume della regione.

Il teorema afferma che l'integrale di superficie di un campo vettoriale su una superficie chiusa (il flusso) è uguale all'integrale di volume della divergenza sulla regione racchiusa dalla superficie.
$$
\int_V \nabla\cdot\mathbf{F}\,dV = \oint_{S(V)}\mathbf{F}\cdot\hat{\mathbf{n}}\, dS = \oint_{S(V)}\mathbf{F}\cdot d\mathbf{S}
$$
