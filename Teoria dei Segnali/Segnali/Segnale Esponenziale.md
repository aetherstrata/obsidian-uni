---
tags:
  - segnale
  - funzione
---
Il segnale esponenziale è una funzione esponenziale con esponente negativo.
$$\begin{gather}
\text{versione unilatera}& \quad x(t) =\ \left\{\begin{matrix*}
e^{-\alpha t} & \longrightarrow & t \geq 0\\
0 & \longrightarrow & t<0 \\
\end{matrix*}\right. \\ \\
\text{versione bilatera}& \quad x(t) =\ \left\{\begin{matrix*}
e^{-\alpha t} & \longrightarrow & t \geq 0\\
e^{\alpha t} & \longrightarrow & t<0 \\
\end{matrix*}\right.
\end{gather}$$
>[!summary] Definizione dell'esponenziale unilatero
> Il segnale esponenziale unilatero può anche essere definito come la moltiplicazione tra una funzione esponenziale e un [[Segnale Gradino|gradino]].
> $$x(t) = e^{-\alpha t} \cdot u(t)$$

Il segnale esponenziale è un segnale a durata praticamente limitata, in quanto gli esponenziali decadono asintoticamente a $0$, per cui si possono ritenere trascurabili ad di fuori di un certo intervallo.

>[!info] Grafico
>![[segnale_esponenziale.png]]

## Esponenziale tempo-discreto

La variante tempo-discreta del segnale gradino è definita come una funzione su numeri interi che vale $a^n$ per tutti i numeri interi maggiori o uguali a $0$, altrimenti vale $0$. 
$$x[n]=a^n\cdot u[n]$$
$u[n]$ è il [[Segnale Gradino#Gradino tempo-discreto|segnale gradino tempo-discreto]].

>[!info] Grafico per $\ a = 0.6$
>![[esponenziale_discreto.png]]
