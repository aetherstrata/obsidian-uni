---
tags:
  - segnale
  - funzione
---
La funzione finestra è una funzione che vale $0$ al di fuori di un certo intervallo. 
## Finestra rettangolare

La funzione finestra rettangolare ha valore costante per tutto l'intervallo.
$$rect(t) = \Biggl\{ \begin{matrix}\begin{align}
1 & \quad \longrightarrow \quad -\frac{1}{2}< t<\frac{1}{2}\\
0 & \quad \longrightarrow \quad altrove
\end{align}\end{matrix}$$
Quando un altro segnale viene moltiplicato per questa finestra, il suo valore viene annullato in tutto il dominio tranne che nell'intervallo della finestra.
### Cambio di scala
$$
rect\left(\frac{t}{T}\right) = \Biggl\{ \begin{matrix}\begin{align}
1 & \quad \longrightarrow \quad -\frac{1}{2}< \frac{t}{T}<\frac{1}{2}\ =\ -\frac{T}{2}< t <\frac{T}{2} \\
0 & \quad \longrightarrow \quad altrove
\end{align}\end{matrix}
$$
## Finestra triangolare
$$tri(t)=\Biggl\{\begin{matrix}\begin{align}
1-|t|  & \longrightarrow -1<t<1 \\
0 \quad & \longrightarrow altrove
\end{align}\end{matrix}$$
### Cambio di scala
$$
tri\left(\frac{t}{T}\right)=\Biggl\{\begin{matrix}\begin{align}
1-|t|  & \longrightarrow -1<\frac{t}{T}<1\ =\ -T<t<T \\
0 \quad & \longrightarrow altrove
\end{align}\end{matrix}
$$
