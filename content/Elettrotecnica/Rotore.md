---
tags:
  - matematica
  - analisi
  - operatore
---
Il rotore = è un operatore che associa a un campo vettoriale $\mathbf{F}$ un altro campo vettoriale $\nabla\times\mathbf{F}$ che esprime una rotazione infinitesima dei vettori.

I vettori del campo sono allineati con l'asse di rotazione, il verso è coerente con quello della regola della mano destra e la lunghezza è il valore della circuitazione infinitesimale.
$$
(\nabla \times \mathbf {F} )(p)\coloneqq\lim _{V\to 0}{\frac {1}{|V|}}\oint _{S}\mathbf {\hat {n}} \times \mathbf {F} \ \mathrm {d} S
$$
### Esempio per spazi tridimensionali
$$
\begin{align*}
\nabla\times\mathbf{F} &=
\begin{vmatrix}
\boldsymbol{\hat{\imath}}&\boldsymbol{\hat{\jmath}}&\boldsymbol{\hat{k}} \\[5mu]
\dfrac{\partial}{\partial x}&\dfrac{\partial}{\partial y}&\dfrac{\partial}{\partial z}\\[5mu]
F_{x}&F_{y}&F_{z}
\end{vmatrix} = \begin{vmatrix}
0 & -\dfrac{\partial}{\partial z} & \hphantom{-}\dfrac{\partial}{\partial y} \\[5mu]
\hphantom{-}\dfrac{\partial}{\partial z}& 0 &-\dfrac{\partial}{\partial x}\\[5mu]
-\dfrac{\partial}{\partial y} & \hphantom{-}\dfrac{\partial}{\partial x}  &0
\end{vmatrix}\ \mathbf{F} = \\[20mu]
&=\boldsymbol{\hat{\imath}}\left(\frac{\partial F_z}{\partial y}-\frac{\partial F_y}{\partial z}\right)+\boldsymbol{\hat{\jmath}}\left(\frac{\partial F_x}{\partial z}-\frac{\partial F_z}{\partial x}\right)+\boldsymbol{\hat{k}} \left(\frac{\partial F_y}{\partial x}-\frac{\partial F_x}{\partial y}\right)
\end{align*}
$$
## Teorema del rotore
Il teorema del rotore, anche detto teorema di Kelvin-Stokes, afferma che il flusso del rotore di un campo vettoriale attraverso una superficie regolare $\mathbf{S}$ è uguale all'integrale di linea del campo lungo il contorno $\Gamma$ della superficie.
$$
\oint_{\Gamma}\mathbf{F}\cdot\mathrm{d}\Gamma = \iint_{S}(\nabla\times\mathbf{F})\cdot\mathbf{\hat{n}}\,dS
$$
