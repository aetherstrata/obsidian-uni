---
tags:
  - matematica
  - analisi
  - operatore
---
Il Gradiente è un operatore matematico che si applica ad una funzione reale e restituisce una funzione vettoriale in cui i vettori sono le derivate parziali della funzione.
## Definizione
### Esempio in uno spazio tridimensionale
$$
\nabla f=\frac{\partial f}{\partial x}\hat{\mathbf{x}}+\frac{\partial f}{\partial y}\hat{\mathbf{y}}+\frac{\partial f}{\partial z}\hat{\mathbf{z}}
$$
### Definizione generale per spazi n-dimensionali
$$
\nabla f=\sum_{i=1}^{n}\frac{\partial f}{\partial x_i}\hat{\mathbf{x}}_i
$$
## Teorema del gradiente
Il teorema del gradiente è una generalizzazione del *teorema fondamentale del calcolo degli integrali di linea* ad una curva qualsiasi sul gradiente.

Di conseguenza gli integrali di linea sul gradiente sono indipendenti dal percorso.
$$
\int_{\gamma}\nabla\varphi\cdot dr = \int_{\mathbf{p}}^{\mathbf{q}}\nabla\varphi\cdot dr = \varphi(\mathbf{q})-\varphi(\mathbf{p})
$$


