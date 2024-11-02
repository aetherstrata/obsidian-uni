---
tags:
  - segnale
  - funzione
---
La funzione finestra è una funzione che vale $0$ al di fuori di un certo intervallo. 
## Finestra rettangolare

La funzione finestra rettangolare ha valore costante per tutto l'intervallo.
$$
\operatorname{rect}(t) = \left\{\begin{matrix}
1 &   -\frac{1}{2}< t<\frac{1}{2}\\
0 &   altrove \vphantom{\frac{1}{2}}
\end{matrix}\right.
$$
Quando un altro segnale viene moltiplicato per questa finestra, il suo valore viene annullato in tutto il dominio tranne che nell'intervallo della finestra.

>[!info] Grafico
>![[finestra_rettangolo.png|center]]

> [!summary] Composizione di gradini
> Si può creare una finestra rettangolare dalla somma di due [[Segnale Gradino|funzioni gradino]].
> 
> $$
> \operatorname{rect}\left(\frac{t-t_0}{T}\right) = u\left(t_0-\frac{T}{2}\right)-u\left(t_0+\frac{T}{2}\right)
>$$
### Traslazione
$$
\operatorname{rect}\left(t-t_0\right) = \left\{\begin{matrix}
1 & \longrightarrow & -\frac{1}{2} < t-t_0 <\frac{1}{2} &=& t_0 -\frac{1}{2}< t - t_0 +\frac{1}{2}\\
0 & \longrightarrow & altrove
\end{matrix}\right.
$$
### Cambio di scala
$$
\operatorname{rect}\left(\frac{t}{T}\right) = \left\{\begin{matrix}
1 & \longrightarrow & - \frac{1}{2}< \frac{t}{T}< \frac{1}{2} & = & -\frac{T}{2}< t < \frac{T}{2} \\
0 & \longrightarrow & altrove
\end{matrix}\right.
$$

>[!example] Alcuni cambi di scala
>![[finestra_rettangolo_scala.png]]
>$$
>\text{Finestre rettangolari con basi di 1, 2 e 4}
>$$
## Finestra triangolare
La funzione finestra triangolare crea un triangolo isoscele di altezza 1 nel suo intervallo.
$$
\operatorname{tri}(t)=\left\{\begin{matrix}
1-|t| & \longrightarrow & -1<t<1 \\
0 & \longrightarrow & altrove
\end{matrix}\right.
$$

> [!info] Grafico
> ![[finestra_triangolo.png|center]]
### Traslazione
$$
\operatorname{tri}(t-t_0)=\left\{\begin{matrix*}
1-|t - t_0| & \longrightarrow & -1<t-t_0<1 & = & t_0-1<t<t_0+1 \\
0  & \longrightarrow & altrove
\end{matrix*}\right.
$$
### Cambio di scala
$$
\operatorname{tri}\left(\frac{t}{T}\right)=\left\{\begin{matrix*}
1-\left|\dfrac{t}{T}\right| & \longrightarrow & -1<\small\dfrac{t}{T}\normalsize<1 & = & -T<t<T \\
0  & \longrightarrow & altrove
\end{matrix*}\right.
$$
