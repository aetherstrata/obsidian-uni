---
tags:
  - matematica
---
La dinamica di un [[Sistema]] descrive le sue mutazioni nel tempo. Per ottenere una situazione di equilibrio, tutte le derivate devono essere uguali a 0.

Se si ottengono autovalori complessi e coniugati dal polinomio caratteristico, allora il sistema presenterà oscillazioni.

> [!example]+ Esempio - Carrello legato a una molla e uno smorzatore
> ![[massa_molla_smorzatore.svg]]
> #### Analisi statica
> Si vuole trovare i paramtri per arrivare ad una situazione di equilibrio tra la forza che spinge verso destra e le reazioni di molla e smorzatore. Per la seconda legge di Newton:
> $$F_{tot} = ma = m \frac{d^2x}{dt^2}$$
> $$F_{tot} = -Kx(t)-c\frac{dx}{dt}+F(t)$$
> $$m \ddot x(t) + c \dot x(t) + Kx(t) = F(t)$$
> All'equilibrio tutte le derivate devono essere uguali a 0 quindi quello che rimane è il termine della molla.
> $$Kx_{eq}=F \longrightarrow x_{eq}=\frac{F}{K}$$
> ---
> #### Polinomio caratteristico
> L'equazione differenziale di secondo grado è stata trasformata in un sistema di equazioni differenziali di primo grado. 
> $$\Biggl\{\begin{matrix*}[l]
> x_1=x \\ x_2=\dot x
> \end{matrix*}
> \quad\longrightarrow\quad
> \Biggl\{\begin{matrix*}[l]
> \dot x_1=x_2 \\ \dot x_2=-\frac{c}{m}x_2-\frac{K}{m}x_1+\frac{1}{m}F
> \end{matrix*}$$
> Trasformandolo in forma matriciale si avrà la rappresentazione di spazio di stato:
> $$
> \begin{bmatrix}
> \dot x_1 \\ \dot x_2
> \end{bmatrix} = \begin{bmatrix}
> 0 & 1 \\
> -\frac{K}{m} & -\frac{c}{m}
> \end{bmatrix} \begin{bmatrix}
> x_1 \\ x_2
> \end{bmatrix} + \begin{bmatrix}
> 0 \\ \frac{1}{m}
> \end{bmatrix} F
> $$
> SI calcola l'autovalore con il determinante del sistema
> $$
> det(\lambda I-A)\ =\ \begin{vmatrix}
> \lambda & -1 \\
> \frac{K}{m} & \lambda + \frac{c}{m}
> \end{vmatrix} \  = \ \lambda² + \frac{c}{m}\lambda + \frac{K}{m} = 0
> $$
> $$
>  \lambda² + \frac{c}{m}\lambda + \frac{K}{m}
>  \quad\longrightarrow\quad
>  \lambda^2+c\lambda+k=0
> $$
> Da qui si può risolvere l'equazione e ottenere i due autovalori $\lambda_1$ e $\lambda_2$

> [!example]- Esempio - Curcuito RC
> ![[circuito_rc.svg|500]]
> Analizzando la struttura del circuito si possono ricavare i comportamenti delle uscite.
> $$
> \begin{equation}
> \begin{split}
> \text{Voltaggio} \quad \longrightarrow & \quad V_0sin(\omega t) = R \cdot i(t) + v_c(t) \\
> \text{Corrente} \quad \longrightarrow & \quad i(t) = C \cdot \frac{dv_c(t)}{dt}
> \end{split}
> \end{equation}
> $$
> Manipolando queste equazioni si ottiene la seguente equazione differenziale con $RC=\tau$.
> $$V_0sin(\omega t) = RC\frac{dv_c(t)}{dt}+v_c(t)$$
> $$\frac{dv_c(t)}{dt}+\frac{v_c(t)-V_0sin(\omega t)}{\tau} = 0$$

