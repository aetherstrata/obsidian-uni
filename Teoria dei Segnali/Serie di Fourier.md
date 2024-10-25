---
tags:
  - analisi
  - segnale
---
 la **serie di Fourier** è una rappresentazione di una [[Funzione Periodica]] mediante una combinazione lineare di funzioni sinusoidali. 
 $$
 x(t) = \sum_{n=-\infty}^{\infty}c_ne^{\Large\frac{i2\pi nt}{T}}
 \quad\longrightarrow\quad
 \begin{matrix*}[c,l]
 c_n & \text{coefficiente di Fourier}\\
 e^{\Large\frac{i2\pi nt}{T}} & \text{armonica}
 \end{matrix*}
 $$
 Il segnale di partenza viene, quindi, scomposto nelle sue **armoniche**, ognuna di periodo $T/n$.
## Forma trigonometrica

Sfruttando la [[Numeri Complessi#Formula di Eulero|formula di Eulero]], si può trasformare la serie in forma complessa in una serie di polinomi trigonometrici.
$$x(t) = \sum_{n=-\infty}^{\infty}c_n\left[\cos\left(\frac{2\pi n t}{T}\right)+i\sin\left(\frac{2\pi n t}{T}\right)\right]$$
## Definizione

Lo spazio delle funzioni periodiche si può considerare uno spazio vettoriale $V$ con prodotto scalare $\left<\vec{v}_1,\vec{v}_2\right>$ in cui ad ogni funzione periodica corrisponde un vettore $\vec{v}$.
### Prodotto scalare
Per definizione, il prodotto scalare tra due segnali si calcola con l'integrale da $-\infty$ a $+\infty$ del primo segnale per il complesso coniugato del secondo.
$$\left<x(t),y(t)\right> = \int_{-\infty}^{\infty}x(t)y^*(t)dt$$
### Basi
Le armoniche definiscono le basi ortonormali dello spazio.
$$\left\{\begin{matrix}
1 &e^{\Large\frac{i2\pi t}{T}} &e^{\Large\frac{i4\pi t}{T}} &e^{\Large\frac{i6\pi t}{T}}  &e^{\Large\frac{i8\pi t}{T}}  &e^{\Large\frac{i10\pi t}{T}} &\dots &e^{\Large\frac{i2\pi nt}{T}} \\
\hat{v}_1& \hat{v}_2&\hat{v}_3&\hat{v}_4& \hat{v}_5& \hat{v}_6&\dots& \hat{v}_n
\end{matrix}\right\}$$
>[!summary] Dimostrazione
>Questo si può dimostrare facendo il prodotto scalare tra due basi.
>$$\begin{align}
>\left<e^{\Large\frac{i2\pi nt}{T}},e^{\Large\frac{i2\pi mt}{T}}\right> &= \frac{1}{T}\int_{-T/2}^{T/2}e^{\Large\frac{i2\pi nt}{T}}e^{\Large-\frac{i2\pi mt}{T}}dt =\\
>&= \frac{1}{T}\int_{-T/2}^{T/2}e^{i2\pi\Large\frac{(n-m)t}{T}}dt =\\
>&= \frac{1}{T}\left.\frac{T}{i2\pi(n-m)}e^{i2\pi\Large\frac{(n-m)t}{T}}\right|_{-T/2}^{T/2} =\\
>&= \frac{1}{\pi(n-m)}\frac{e^{i\pi(n-m)}-e^{-i\pi(n-m)}}{2i} =\\
>&= \frac{\sin[\pi(n-m)]}{\pi(n-m)} =\\
>&= \operatorname{sinc}(n-m)
>\end{align}$$
>Il risultato è un [[Seno Cardinale]]. Sapendo che $n$ e $m$ sono numeri interi, si può dedurre che il risultato sarà uguale a $1$ se $n=m$, altrimenti sarà uguale a $0$.
>$$\left<e^{\Large\frac{i2\pi nt}{T}},e^{\Large\frac{i2\pi mt}{T}}\right> = \left\{\begin{matrix}
>1&\text{per}&n=m\\
>0&\text{per}&n\ne m
>\end{matrix}\right.\quad\longrightarrow\quad\delta(n-m)$$
>È dimostrato che la base è ortonormale e l'operazione ha lo stesso comportamento di una [[Delta di Dirac]].

Quindi, lo spazio vettoriale $V$ è uno spazio euclideo di **dimensione infinita numerabile**.
### Coefficienti di Fourier
Facendo il prodotto scalare della funzione per un suo versore, si può estrarre il suo coefficiente per l'armonica scelta.
$$c_n=\left<x(t), e^{\Large\frac{i2\pi nt}{T}}\right> = \frac{1}{T}\int_{-T/2}^{T/2}x(t)e^{\Large-\frac{i2\pi nt}{T}}dt$$
## Condizioni di Dirichlet
Un segnale periodico $x(t)$, di periodo $T$, può essere sviluppato in serie se soddisfa le condizioni al contorno di Dirichlet:

- deve essere assolutamente integrabile, ovvero che il seguente integrale deve convergere
$$\displaystyle\int_{T/2}^{-T/2}|x(t)|dt < \infty$$
- presenta un numero finito di discontinuità di prima specie, ovvero continuo a tratti
- contiene un numero finito di massimi e minimi, ovvero è derivabile ovunque tranne, al più, un numero finito di punti
## Proprietà
### Traslazione nel tempo
$$\begin{gather}
\begin{aligned}
x(t)=&\sum_{n=-\infty}^{\infty}c_n\cdot e^{\Large\frac{i2\pi nt}{T}} \\
x(t-t_0)=&\sum_{n=-\infty}^{\infty}d_n\cdot e^{\Large\frac{i2\pi nt}{T}} \\
x(t-t_0)=& \sum_{n=-\infty}^{\infty}c_n\cdot e^{\Large\frac{i2\pi n(t-t_0)}{T}} = \sum_{n=-\infty}^{\infty}c_n\cdot e^{\Large\frac{i2\pi nt}{T}}e^{\Large-\frac{i2\pi nt_0}{T}} \end{aligned} \\ \\
d_n = c_n \cdot e^{\Large-\frac{i2\pi nt_0}{T}}
\end{gather}$$
#### Dimostrazione
Si calcolano i coefficienti di Fourier usando la formula generale.
$$
\begin{align}
d_n=\frac{1}{T}\int_{-T/2}^{T/2}x(t-t_0)\cdot e^{\Large-\frac{i2\pi nt}{T}}dt
\end{align}$$
>[!summary] Cambio di variabile
$$t -t_0 = t' \quad\longrightarrow\quad d_n = \frac{1}{T}\int_{-T/2 - t_0}^{T/2-t_0}x(t')\cdot e^{\Large-\frac{i2\pi nt'}{T}}e^{\Large-\frac{i2\pi nt_0}{T}}dt$$

A questo punto si porta l'esponenziale in $t_0$ fuori dall'integrale.
$$d_n= e^{\Large-\frac{i2\pi nt_0}{T}}\underbrace{\frac{1}{T}\int_{-T/2 - t_0}^{T/2-t_0}x(t')\cdot e^{\Large-\frac{i2\pi nt'}{T}}dt}_{\Large c_n \vphantom{\big|}}$$
Il termine integrale è uguale a quello calcolato per il segnale non traslato.
$$d_n = c_n \cdot e^{\Large-\frac{i2\pi nt_0}{T}}$$
Quindi è dimostrato che i coefficienti di Fourier di un segnale periodico traslato nel tempo sono uguali a quelli del segnale originale ma moltiplicati per un'armonica.
### Traslazione in frequenza
$$\begin{gather}\begin{aligned}
x(t)&=\sum_{n=-\infty}^{\infty}c_n\cdot e^{\Large\frac{i2\pi nt}{T}}\\
y(t)&= \sum_{n=-\infty}^{\infty}d_n\cdot e^{\Large\frac{i2\pi nt}{T}}=x(t)\cdot e^{\Large\frac{i2\pi kt}{T}}\\\\ \hline
\end{aligned} \\ \\
y(t)=\sum_{n=-\infty}^{\infty}c_n\cdot e^{\Large\frac{i2\pi nt}{T}} e^{\Large\frac{i2\pi kt}{T}}=\sum_{n=-\infty}^{\infty}c_n\cdot e^{\Large\frac{i2\pi (n+k)t}{T}}\xRightarrow{n+k = n'}\sum_{n'=-\infty}^{\infty}c_{n'-k}\cdot e^{\Large\frac{i2\pi n't}{T}}
\end{gather}$$

I coefficienti di Fourier del segnale traslato in frequenza $y(t)$ sono uguali a quelli del segnale originale $x(t)$ calcolati in $n-k$.
$$d_n=c_{n-k}$$
### Modulazione
$$\begin{gather}\begin{aligned}
x(t)&=\sum_{n=-\infty}^{\infty}c_n\cdot e^{\Large\frac{i2\pi nt}{T}}\\
y(t)= \cos\left(\frac{2\pi t}{T}\right)x(t)&=\frac{x(t)}{2}\left[e^{\Large\frac{i2\pi t}{T}}+e^{\Large-\frac{i2\pi t}{T}}\right] \\\\ \hline
\end{aligned} \\\\ \begin{aligned}
y(t) &= \frac{1}{2}\sum_{n=-\infty}^{\infty}c_n\cdot e^{\Large\frac{i2\pi nt}{T}}\left[e^{\Large\frac{i2\pi t}{T}}+e^{\Large-\frac{i2\pi t}{T}}\right] =\\
&= \frac{1}{2}\sum_{n'=-\infty}^{\infty}c_n\cdot e^{\Large\frac{i2\pi (n+1)t}{T}}+\frac{1}{2}\sum_{n''=-\infty}^{\infty}c_n\cdot e^{\Large\frac{i2\pi (n-1)t}{T}} =\\
&= \frac{1}{2}\sum_{n'=-\infty}^{\infty}c_{n'-1}\cdot e^{\Large\frac{i2\pi n't}{T}} + \frac{1}{2}\sum_{n'=-\infty}^{\infty}c_{n''+1}\cdot e^{\Large\frac{i2\pi n''t}{T}}
\\\\ \hline \end{aligned} \\\\
d_n = \frac{1}{2}c_{n-1}+\frac{1}{2}c_{n+1}
\end{gather}$$
 I coefficienti del segnale modulato sono uguali a quelli del segnale modulante doppiati e dimezzati, uno traslato di un unità verso destra, e l'altro verso sinistra.
### Derivazione
$$\begin{gather}\begin{aligned}
x(t)&=\sum_{n=-\infty}^{\infty}c_n\cdot e^{\Large\frac{i2\pi nt}{T}}\\
y(t)=\frac{d}{dt}x(t)&=\frac{d}{dt}\sum_{n=-\infty}^{\infty}c_n\cdot e^{\Large\frac{i2\pi nt}{T}} = \sum_{n=-\infty}^{\infty}c_n\frac{i2\pi n}{T}\cdot e^{\Large\frac{i2\pi nt}{T}} 
\\ \\ \hline \end{aligned} \\ \\
d_n = c_n\frac{i2\pi n}{T}
\end{gather}$$
Dato un segnale, si conoscono anche i coefficienti di Fourier della sua derivata prima, che si ottengono moltiplicando per $\dfrac{i2\pi n}{T}$.
## Teorema di Parseval

Il Teorema di Parseval permette di calcolare velocemente la potenza di un segnale rappresentato come serie di Fourier.

>[!info] Integrale di potenza
>$$P_x=\frac{1}{T}\int_{-T/2}^{T/2}x(t)x^*(t)dt = \frac{1}{T}\int_{-T/2}^{T/2}\underbrace{\sum_{n=-\infty}^{\infty}c_n\cdot e^{\Large\frac{i2\pi nt}{T}}}_{\Large x(t)}\underbrace{\sum_{m=-\infty}^{\infty}c^*_m\cdot e^{\Large-\frac{i2\pi mt}{T}}}_{\Large x^*(t)}dt$$

Invertendo l'ordine tra integrale e sommatorie, rimane solo il prodotto tra due armoniche dentro l'integrale.
$$P_x = \sum_{n=-\infty}^{\infty}\sum_{m=-\infty}^{\infty}c_n\cdot c^*_m\ \underbrace{\frac{1}{T}\int_{-T/2}^{T/2}e^{\Large\frac{i2\pi (n-m)t}{T}}dt}_{\Large\delta(n-m)}$$
Questo è uguale al prodotto scalare tra due [[#Basi|basi]] e il suo risultato è $\delta(n-m)$. Quindi si possono scartare tutti i termini tranne quelli in cui $n=m$.
$$P_x = \sum_{n=-\infty}^{\infty}c_n\cdot c^*_n = \sum_{n=-\infty}^{\infty}|c_n|^2$$
## Esempi
### Segnale coseno
Dato il segnale $x(t) = A\cos\left(\dfrac{2\pi t}{T}\right)$, si possono ricavare i suoi coefficienti di Fourier senza calcolare l'integrale usando la [[Numeri Complessi#Formula di Eulero|formula di Eulero]].
$$x(t)= A\cos\left(\frac{2\pi t}{T}\right) = \frac{A}{2}\left(\boxed{e^{\Large\frac{i2\pi t}{T}}}+\boxed{e^{\Large-\frac{i2\pi t}{T}}}\right)$$
Si può notare intuitivamente che questo segnale è composto da due armoniche e che il loro coefficiente è proprio $\dfrac{A}{2}$.
$$\begin{gather}
c_1=\frac{A}{2} &;& c_{-1}=\frac{A}{2}
\end{gather}$$
### Onda quadra
Dato un segnale a onda quadra di periodo $T$, si vuole ottenere i suoi coefficienti di Fourier.
$$x(t)=\sum_{n=-\infty}^{+\infty}\operatorname{rect}\left(\frac{t-nT}{\tau}\right)$$
La sua rappresentazione in serie di Fourier richiede infiniti coefficienti.
$$\begin{align}
c_n&= \frac{1}{T}\int_{-T/2}^{T/2}\operatorname{rect}\left(\frac{t}{\tau}\right)e^{\Large-\frac{i2\pi nt}{T}}dt =\\
&= \frac{1}{T}\int_{-\tau/2}^{\tau/2}e^{\Large-\frac{i2\pi nt}{T}}dt =\\
&=\frac{1}{T}\left.\frac{T}{-i2\pi nt}e^{\Large-\frac{i2\pi nt}{T}}\right|_{-\tau/2}^{\tau/2} =\\
&= \frac{1}{\pi n}\frac{e^{\Large\frac{i2\pi n\tau}{T}}-e^{\Large-\frac{i2\pi n\tau}{T}}}{2i} =\\
&= \frac{1}{\pi n}\sin\left(\frac{\pi n\tau}{T}\right)=\\
&= \frac{\tau}{T}\operatorname{sinc}\left(\frac{n\tau}{T}\right)
\end{align}$$
Il coefficiente $c_0$ avrà valore $\dfrac{\tau}{T}$ e il resto dei coefficienti seguirà l'andamento del seno cardinale. 
Di conseguenza, il peso dei coefficienti diminuisce all'aumentare del valore assoluto di $n$.

>[!info] Parametri $\tau$ e $T$ divisibili tra loro
>Nel caso in cui $\tau$ e $T$ siano divisibili tra loro, alcuni coefficienti saranno nulli.
>
>---
>Ad esempio, se l'onda quadra ha un duty cycle del 50% allora $\tau$ sarà la metà di $T$ e la funzione per ricavare i coefficienti può essere semplificata.
>$$c_n = \frac{\tau}{T}\operatorname{sinc}\left(\frac{n\tau}{T}\right) = \frac{1}{2}\operatorname{sinc}\left(\frac{n}{2}\right)$$
>Si può vedere come, per valori di $n$ divisibili per $2$, quindi per numeri pari, si ricavano coefficienti nulli, visto che per qualsiasi intero diverso da $0$ il seno cardinale vale $0$.
>---
>In generale, nei casi in cui $T$ sia un multiplo di $\tau$ e il rapporto tra loro valga $m$, i coefficienti di Fourier ottenuti da valori di $n$ divisibili per $m$ saranno nulli.
>$$c_n = \frac{\tau}{T}\operatorname{sinc}\left(\frac{n\tau}{T}\right) = \frac{1}{m}\operatorname{sinc}\left(\frac{n}{m}\right)$$
