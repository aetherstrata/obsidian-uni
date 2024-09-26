---
tags:
  - segnale
---
Il segnale esponenziale è una funzione esponenziale con esponente negativo.
$$\begin{align}
\text{versione unilatera}& \quad x(t) =\ \Biggl\{\begin{matrix*}
e^{-\alpha t} & \longrightarrow & t \geq 0\\
0 & \longrightarrow & t<0 \\
\end{matrix*} \\ \\
\text{versione bilatera}& \quad x(t) =\ \Biggl\{\begin{matrix*}
e^{-\alpha t} & \longrightarrow & t \geq 0\\
e^{\alpha t} & \longrightarrow & t<0 \\
\end{matrix*}
\end{align}$$
>[!summary] Definizione dell'esponenziale unilatero
> Il segnale esponenziale unilatero può anche essere definito come la moltiplicazione tra una funzione esponenziale e un [[Segnale Gradino|gradino]].
> $$x(t) = e^{-\alpha t} \cdot u(t)$$

Il segnale esponenziale è un segnale a durata praticamente limitata, in quanto gli esponenziali decadono asintoticamente a $0$, per cui si possono ritenere trascurabili ad di fuori di un certo intervallo.

>[!info] Grafico
>![[segnale_esponenziale.png]]

