---
tags:
  - analisi
  - segnale
---
 la **serie di Fourier** è una rappresentazione di una [[Funzione Periodica]] mediante una combinazione lineare di funzioni sinusoidali.  
 $$
 x(t) = \sum_{n=-\infty}^{\infty}c_ne^{\large\frac{i2\pi nt}{T}}
 \quad\longrightarrow\quad
 \begin{cases}
 \hphantom{11}c_n & \text{coefficiente di Fourier}\\
 e^{\large\frac{i2\pi nt}{T}} & \text{armonica}
 \end{cases}
$$
 Il segnale di partenza viene, quindi, scomposto nelle sue **armoniche**, ognuna di periodo $T/n$.
## Forma trigonometrica
Sfruttando la [[Numeri Complessi#Formula di Eulero|formula di Eulero]], si può trasformare la serie in forma complessa in una serie di polinomi trigonometrici.
$$
x(t) = \sum_{n=-\infty}^{\infty}c_n\left[\cos\left(\frac{2\pi n t}{T}\right)+i\sin\left(\frac{2\pi n t}{T}\right)\right]
$$
## Definizione
Lo spazio delle funzioni periodiche si può considerare uno spazio vettoriale $V$ con prodotto scalare $\left<\vec{v}_1,\vec{v}_2\right>$ in cui ad ogni funzione periodica corrisponde un vettore $\vec{v}$.
### Prodotto scalare
Per definizione, il prodotto scalare tra due segnali si calcola con l'integrale da $-\infty$ a $+\infty$ del primo segnale per il complesso coniugato del secondo.
$$
\left<x(t),y(t)\right> = \int_{-\infty}^{\infty}x(t)y^*(t)dt
$$
### Basi
Le armoniche definiscono le basi ortonormali dello spazio.
$$
\begin{Bmatrix}
1 &e^{\large\frac{i2\pi t}{T}} &e^{\large\frac{i4\pi t}{T}} &e^{\large\frac{i6\pi t}{T}}  &e^{\large\frac{i8\pi t}{T}}  &e^{\large\frac{i10\pi t}{T}} &\dots &e^{\large\frac{i2\pi nt}{T}} \\
\hat{v}_1& \hat{v}_2&\hat{v}_3&\hat{v}_4& \hat{v}_5& \hat{v}_6&\dots& \hat{v}_n
\end{Bmatrix}
$$

Questo si può dimostrare facendo il prodotto scalare tra due basi.
$$
\begin{align*}
\left<e^{\large\frac{i2\pi nt}{T}},e^{\large\frac{i2\pi mt}{T}}\right> &= \frac{1}{T}\int_{-T/2}^{T/2}e^{\large\frac{i2\pi nt}{T}}e^{\large-\frac{i2\pi mt}{T}}dt =\\
&= \frac{1}{T}\int_{-T/2}^{T/2}e^{i2\pi\large\frac{(n-m)t}{T}}dt =\\
&= \frac{1}{T}\left.\frac{T}{i2\pi(n-m)}e^{i2\pi\large\frac{(n-m)t}{T}}\right|_{-T/2}^{T/2} =\\
&= \frac{1}{\pi(n-m)}\frac{e^{i\pi(n-m)}-e^{-i\pi(n-m)}}{2i} =\\
&= \frac{\sin[\pi(n-m)]}{\pi(n-m)} =\\
&= \operatorname{sinc}(n-m)
\end{align*}
$$

Il risultato è un [[Seno Cardinale]]. Sapendo che $n$ e $m$ sono numeri interi, si può dedurre che il risultato sarà uguale a $1$ se $n=m$, altrimenti sarà uguale a $0$.
$$
\left<e^{\large\frac{i2\pi nt}{T}},e^{\large\frac{i2\pi mt}{T}}\right> = \left\{\begin{matrix}
1&\text{per}&n=m\\
0&\text{per}&n\ne m
\end{matrix}\right.\quad\longrightarrow\quad\delta(n-m)
$$

È dimostrato che la base è ortonormale e l'operazione ha lo stesso comportamento di una [[Delta di Dirac]].

Quindi, lo spazio vettoriale $V$ è uno spazio euclideo di **dimensione infinita numerabile**.
### Coefficienti di Fourier
Facendo il prodotto scalare della funzione per un suo versore, si può estrarre il suo coefficiente per l'armonica scelta.
$$
c_n=\left<x(t), e^{\large\frac{i2\pi nt}{T}}\right> = \frac{1}{T}\int_{-T/2}^{T/2}x(t)e^{\large-\frac{i2\pi nt}{T}}dt
$$

## Condizioni di Dirichlet
Un segnale periodico $x(t)$, di periodo $T$, può essere sviluppato in serie se soddisfa le condizioni al contorno di Dirichlet:

- deve essere assolutamente integrabile, ovvero che il seguente integrale deve convergere
$$
\int_{T/2}^{-T/2}|x(t)|dt < \infty
$$
- presenta un numero finito di discontinuità di prima specie, ovvero continuo a tratti
- contiene un numero finito di massimi e minimi, ovvero è derivabile ovunque tranne, al più, un numero finito di punti
## Proprietà
### Traslazione nel tempo
$$
\begin{gather*}
\begin{aligned}
x(t)=&\sum_{n=-\infty}^{\infty}c_n\cdot e^{\large\frac{i2\pi nt}{T}} \\
x(t-t_0)=&\sum_{n=-\infty}^{\infty}d_n\cdot e^{\large\frac{i2\pi nt}{T}} \\
x(t-t_0)=& \sum_{n=-\infty}^{\infty}c_n\cdot e^{\large\frac{i2\pi n(t-t_0)}{T}} = \sum_{n=-\infty}^{\infty}c_n\cdot e^{\large\frac{i2\pi nt}{T}}e^{\large-\frac{i2\pi nt_0}{T}} \end{aligned} \\ \\
d_n = c_n \cdot e^{\large-\frac{i2\pi nt_0}{T}}
\end{gather*}
$$
#### Dimostrazione
Si calcolano i coefficienti di Fourier usando la formula generale.
$$
\begin{align*}
d_n=\frac{1}{T}\int_{-T/2}^{T/2}x(t-t_0)\cdot e^{\large-\frac{i2\pi nt}{T}}dt
\end{align*}
$$

>[!summary] Cambio di variabile
>$$
>t -t_0 = t' \quad\longrightarrow\quad d_n = \frac{1}{T}\int_{-T/2 - t_0}^{T/2-t_0}x(t')\cdot e^{\large-\frac{i2\pi nt'}{T}}e^{\large-\frac{i2\pi nt_0}{T}}dt
>$$

A questo punto si porta l'esponenziale in $t_0$ fuori dall'integrale.
$$
d_n= e^{\large-\frac{i2\pi nt_0}{T}}\underbrace{\frac{1}{T}\int_{-T/2 - t_0}^{T/2-t_0}x(t')\cdot e^{\large-\frac{i2\pi nt'}{T}}dt}_{\large c_n \vphantom{\big|}}
$$
Il termine integrale è uguale a quello calcolato per il segnale non traslato.
$$
d_n = c_n \cdot e^{\large-\frac{i2\pi nt_0}{T}}
$$
Quindi è dimostrato che i coefficienti di Fourier di un segnale periodico traslato nel tempo sono uguali a quelli del segnale originale ma moltiplicati per un'armonica.
### Traslazione in frequenza
$$
\begin{gather*}\begin{aligned}
x(t)&=\sum_{n=-\infty}^{\infty}c_n\cdot e^{\large\frac{i2\pi nt}{T}}\\
y(t)&= \sum_{n=-\infty}^{\infty}d_n\cdot e^{\large\frac{i2\pi nt}{T}}=x(t)\cdot e^{\large\frac{i2\pi kt}{T}}\\\\ \hline
\end{aligned} \\ \\
y(t)=\sum_{n=-\infty}^{\infty}c_n\cdot e^{\large\frac{i2\pi nt}{T}} e^{\large\frac{i2\pi kt}{T}}=\sum_{n=-\infty}^{\infty}c_n\cdot e^{\large\frac{i2\pi (n+k)t}{T}}\xRightarrow{n+k = n'}\sum_{n'=-\infty}^{\infty}c_{n'-k}\cdot e^{\large\frac{i2\pi n't}{T}}
\end{gather*}
$$
I coefficienti di Fourier del segnale traslato in frequenza $y(t)$ sono uguali a quelli del segnale originale $x(t)$ calcolati in $n-k$.
$$
d_n=c_{n-k}
$$
### Modulazione
$$
\begin{gather*}\begin{aligned}
x(t)&=\sum_{n=-\infty}^{\infty}c_n\cdot e^{\large\frac{i2\pi nt}{T}}\\
y(t)= \cos\left(\frac{2\pi t}{T}\right)x(t)&=\frac{x(t)}{2}\left[e^{\large\frac{i2\pi t}{T}}+e^{\large-\frac{i2\pi t}{T}}\right] \\\\ \hline
\end{aligned} \\\\ \begin{aligned}
y(t) &= \frac{1}{2}\sum_{n=-\infty}^{\infty}c_n\cdot e^{\large\frac{i2\pi nt}{T}}\left[e^{\large\frac{i2\pi t}{T}}+e^{\large-\frac{i2\pi t}{T}}\right] =\\
&= \frac{1}{2}\sum_{n'=-\infty}^{\infty}c_n\cdot e^{\large\frac{i2\pi (n+1)t}{T}}+\frac{1}{2}\sum_{n''=-\infty}^{\infty}c_n\cdot e^{\large\frac{i2\pi (n-1)t}{T}} =\\
&= \frac{1}{2}\sum_{n'=-\infty}^{\infty}c_{n'-1}\cdot e^{\large\frac{i2\pi n't}{T}} + \frac{1}{2}\sum_{n'=-\infty}^{\infty}c_{n''+1}\cdot e^{\large\frac{i2\pi n''t}{T}}
\\\\ \hline \end{aligned} \\\\
d_n = \frac{1}{2}c_{n-1}+\frac{1}{2}c_{n+1}
\end{gather*}
$$

 I coefficienti del segnale modulato sono uguali a quelli del segnale modulante doppiati e dimezzati, uno traslato di un unità verso destra, e l'altro verso sinistra.
### Derivazione
$$
\begin{gather*}\begin{aligned}
x(t)&=\sum_{n=-\infty}^{\infty}c_n\cdot e^{\large\frac{i2\pi nt}{T}}\\
y(t)=\frac{d}{dt}x(t)&=\frac{d}{dt}\sum_{n=-\infty}^{\infty}c_n\cdot e^{\large\frac{i2\pi nt}{T}} = \sum_{n=-\infty}^{\infty}c_n\frac{i2\pi n}{T}\cdot e^{\large\frac{i2\pi nt}{T}} 
\\ \\ \hline \end{aligned} \\ \\
d_n = c_n\frac{i2\pi n}{T}
\end{gather*}
$$

Dato un segnale, si conoscono anche i coefficienti di Fourier della sua derivata prima, che si ottengono moltiplicando per $\dfrac{i2\pi n}{T}$.
## Teorema di Parseval

Il Teorema di Parseval permette di calcolare velocemente la potenza di un segnale rappresentato come serie di Fourier.
$$
P_x=\frac{1}{T}\int_{-T/2}^{T/2}x(t)x^*(t)dt = \frac{1}{T}\int_{-T/2}^{T/2}\underbrace{\sum_{n=-\infty}^{\infty}c_n\cdot e^{\large\frac{i2\pi nt}{T}}}_{\large x(t)}\underbrace{\sum_{m=-\infty}^{\infty}c^*_m\cdot e^{\large-\frac{i2\pi mt}{T}}}_{\large x^*(t)}dt
$$
Invertendo l'ordine tra integrale e sommatorie, rimane solo il prodotto tra due armoniche dentro l'integrale.
$$
P_x = \sum_{n=-\infty}^{\infty}\sum_{m=-\infty}^{\infty}c_n\cdot c^*_m\ \underbrace{\frac{1}{T}\int_{-T/2}^{T/2}e^{\large\frac{i2\pi (n-m)t}{T}}dt}_{\large\delta(n-m)}
$$
Questo è uguale al prodotto scalare tra due [[#Basi|basi]] e il suo risultato è $\delta(n-m)$. Quindi si possono scartare tutti i termini tranne quelli in cui $n=m$.
$$
P_x = \sum_{n=-\infty}^{\infty}c_n\cdot c^*_n = \sum_{n=-\infty}^{\infty}|c_n|^2
$$
## Esempi
### Segnale coseno
Dato il segnale $x(t) = A\cos\left(\dfrac{2\pi t}{T}\right)$, si possono ricavare i suoi coefficienti di Fourier senza calcolare l'integrale usando la [[Numeri Complessi#Formula di Eulero|formula di Eulero]].
$$
x(t)= A\cos\left(\frac{2\pi t}{T}\right) = \frac{A}{2}\left(\boxed{e^{\large\frac{i2\pi t}{T}}}+\boxed{e^{\large-\frac{i2\pi t}{T}}}\right)
$$
Si può notare intuitivamente che questo segnale è composto da due armoniche e che il loro coefficiente è proprio $\dfrac{A}{2}$.
$$
c_1=\frac{A}{2} \quad;\quad c_{-1}=\frac{A}{2}
$$
### Onda quadra
Dato un segnale a onda quadra di periodo $T$, si vuole ottenere i suoi coefficienti di Fourier.
$$
x(t)=\sum_{n=-\infty}^{+\infty}\operatorname{rect}\left(\frac{t-nT}{\tau}\right)
$$
La sua rappresentazione in serie di Fourier richiede infiniti coefficienti.
$$
\begin{align*}
c_n&= \frac{1}{T}\int_{-T/2}^{T/2}\operatorname{rect}\left(\frac{t}{\tau}\right)e^{\large-\frac{i2\pi nt}{T}}dt =\\
&= \frac{1}{T}\int_{-\tau/2}^{\tau/2}e^{\large-\frac{i2\pi nt}{T}}dt =\\
&=\frac{1}{T}\left.\frac{T}{-i2\pi nt}e^{\large-\frac{i2\pi nt}{T}}\right|_{-\tau/2}^{\tau/2} =\\
&= \frac{1}{\pi n}\frac{e^{\large\frac{i2\pi n\tau}{T}}-e^{\large-\frac{i2\pi n\tau}{T}}}{2i} =\\
&= \frac{1}{\pi n}\sin\left(\frac{\pi n\tau}{T}\right)=\\
&= \frac{\tau}{T}\operatorname{sinc}\left(\frac{n\tau}{T}\right)
\end{align*}
$$
Il coefficiente $c_0$ avrà valore $\dfrac{\tau}{T}$ e il resto dei coefficienti seguirà l'andamento del seno cardinale. 
Di conseguenza, il peso dei coefficienti diminuisce all'aumentare del valore assoluto di $n$.

>[!info] Parametri $\tau$ e $T$ divisibili tra loro
>Nel caso in cui $\tau$ e $T$ siano divisibili tra loro, alcuni coefficienti saranno nulli.
>
>---
>Ad esempio, se l'onda quadra ha un duty cycle del 50% allora $\tau$ sarà la metà di $T$ e la funzione per ricavare i coefficienti può essere semplificata.
>$$
>c_n = \frac{\tau}{T}\operatorname{sinc}\left(\frac{n\tau}{T}\right) = \frac{1}{2}\operatorname{sinc}\left(\frac{n}{2}\right)
>$$
>Si può vedere come, per valori di $n$ divisibili per $2$, quindi per numeri pari, si ricavano coefficienti nulli, visto che per qualsiasi intero diverso da $0$ il seno cardinale vale $0$.
>
>---
>In generale, nei casi in cui $T$ sia un multiplo di $\tau$ e il rapporto tra loro valga $m$, i coefficienti di Fourier ottenuti da valori di $n$ divisibili per $m$ saranno nulli.
>$$
>c_n = \frac{\tau}{T}\operatorname{sinc}\left(\frac{n\tau}{T}\right) = \frac{1}{m}\operatorname{sinc}\left(\frac{n}{m}\right)
>$$
### Somma di seno e coseno con diversi periodi
#### Esercizio 1
Dato il seguente segnale, calcolare la sua rappresentazione in serie di Fourier.
$$
x(t) = 2\cos\left(\frac{2\pi t}{T}+\frac{\pi}{4}\right)+6\sin\left(\frac{2\pi t}{3T}{}\right)
$$

>[!info] Periodo del segnale
>Dato che il coseno ha periodo $T$ e il seno ha periodo $3T$, allora il periodo del segnale $x(t)$ sarà uguale a $3T$.
>$$
>T'=3T
>$$

Per agevolare i calcoli, si trasformano le funzioni trigonometriche in esponenziali.
$$
\begin{align*}
x(t)&=e^{\large i\left(\frac{2\pi t}{T}+\frac{\pi}{4}\right)}+e^{\large-i\left(\frac{2\pi t}{T}+\frac{\pi}{4}\right)}+\frac{3}{i}e^{\large i\frac{2\pi t}{3T}}-\frac{3}{i}e^{\large-i\frac{2\pi t}{3T}}\\
&=\sum_{n=-\infty}^{+\infty}c_ne^{\large-\frac{i2\pi nt}{3T}}
\end{align*}
$$
Si può notare come tutti i termini di questa espressione siano già espressi in funzione di un'armonica.
$$
x(t)=e^{\large\frac{i\pi}{4}}\boxed{e^{\large i\frac{6\pi t}{3T}}}+
e^{\large-\frac{i\pi}{4}}\boxed{e^{\large-i\frac{6\pi t}{3T}}}+
\frac{3}{i}\boxed{e^{\large i\frac{2\pi t}{3T}}}-\frac{3}{i}\boxed{e^{\large-i\frac{2\pi t}{3T}}}
$$

Di conseguenza si possono ricavare direttamente i coefficienti di Fourier.
$$
\begin{align*}
c_1&=-3i &;&& c_{-1}&=3i \\
c_3&=e^{\large\frac{i\pi}{4}} &;&& c_{-3}&=e^{\large-\frac{i\pi}{4}}
\end{align*}
$$

#### Esercizio 2
Dato il seguente segnale, calcolare la sua rappresentazione in serie di Fourier.
$$
x(t) = \sin\left(2\pi t\right)+\cos\left(4\pi t+\phi\right)
$$
>[!info] Periodo del segnale
>Dato che il coseno ha periodo $1/2$ e il seno ha periodo $1$, allora il periodo del segnale $x(t)$ sarà uguale a $1$.
>$$
>T=1
>$$

Per agevolare i calcoli, si trasformano le funzioni trigonometriche in esponenziali.
$$
x(t)=\frac{e^{\large i2\pi t}-e^{\large-i2\pi t}}{2i}+\frac{e^{\large i(4\pi t+\phi)}+e^{\large -i(4\pi t+\phi)}}{2}
$$
Si può notare come tutti i termini di questa espressione siano già espressi in funzione di un'armonica.
$$
x(t)=\frac{1}{2i}\boxed{e^{\large i2\pi t}}-\frac{1}{2i}\boxed{e^{\large-i2\pi t}}+\frac{e^{\large i\phi}}{2}\boxed{e^{\large i4\pi t}}+\frac{e^{\large-i\phi}}{2}\boxed{e^{\large -i4\pi t}}
$$
Di conseguenza si possono ricavare direttamente i coefficienti di Fourier.
$$
\begin{align*}
c_1&=-\frac{i}{2} &;&& c_{-1}&=\frac{i}{2} \\
c_2&=\frac{e^{\large i\phi}}{2} &;&& c_{-2}&=\frac{e^{\large -i\phi}}{2}
\end{align*}
$$
### Seno quadrato
Dato il seguente segnale, calcolare la sua rappresentazione in serie di Fourier.
$$
x(t)=\sin^2\left(\frac{2\pi t}{T}+\phi\right)
$$
Si applica la [[Formule Trigonometriche#Identità fondamentale della Trigonometria|relazione fondamentale della Trigonometria]] e la [[Formule Trigonometriche#Formule di duplicazione|formula di duplicazione]].
$$
\begin{gather*}
\cos(2\alpha)=\cos^2(\alpha)-\sin^2(\alpha)=1-2\sin^2(\alpha) \\\\
\Downarrow \\\\
x(t)=\frac{1}{2}-\frac{1}{2}\cos\left(\frac{4\pi t}{T}+2\phi\right)
\end{gather*}
$$
Per agevolare i calcoli, si trasforma il coseno con la [[Numeri Complessi#Formula di Eulero|formula di Eulero]].
$$
x(t)=\frac{1}{2}-\frac{1}{4}e^{\large i\left(\frac{4\pi t}{T}+2\phi\right)}-\frac{1}{4}e^{\large-i\left(\frac{4\pi t}{T}+2\phi\right)}
$$
Si può notare come tutti i termini di questa espressione siano già espressi in funzione di un'armonica.
$$
x(t)=\frac{1}{2}-\frac{e^{\large i2\phi}}{4}\boxed{e^{\large i\frac{4\pi t}{T}}} - \frac{e^{\large -i2\phi}}{4}\boxed{e^{\large -i\frac{4\pi t}{T}}}
$$
Di conseguenza si possono ricavare direttamente i coefficienti di Fourier.
$$
c_{-1}=-\frac{e^{\large -i2\phi}}{4}\quad;\quad c_0=\frac{1}{2}\quad;\quad c_1=-\frac{e^{\large i2\phi}}{4}
$$
### Onda a dente di sega
#### Esercizio 1
Dato un segnale $x(t)$ di periodo $T$ che vale $t$ all'interno del periodo, calcolare la sua rappresentazione in serie di Fourier.

>[!info] Grafico del segnale
>![[sawtooth-origine.png]]
>$$
>\text{Gli assi sono in scala di }T
>$$

In questo caso l'unico modo per procedere è con il calcolo dell'integrale.
$$
c_n = \frac{1}{T}\int_{-T/2}^{T/2}t\cdot e^{\large-i\frac{2\pi nt}{T}}dt
$$

>[!summary] Calcolo dell'integrale per parti
>$$
>\begin{align*}
>c_n&=\frac{1}{T}\left|\frac{t\cdot e^{\large-i\frac{2\pi nt}{T}}}{-i\frac{2\pi n}{T}}\right|_{-T/2}^{T/2} -\frac{1}{T}\int_{-T/2}^{T/2}\frac{e^{\large-i\frac{2\pi nt}{T}}}{-i\frac{2\pi n}{T}}dt=\\
>&=\frac{T}{2}\frac{e^{\large i\pi n}+e^{\large -i\pi n}}{-i2\pi n}+\frac{1}{i2\pi n}\left|\frac{e^{\large -i\frac{2\pi nt}{T}}}{-i\frac{2\pi n}{T}}\right|_{-T/2}^{T/2} =\\
>&=\frac{Ti}{2\pi n}\cos(\pi n)-\frac{T}{4\pi^2n^2}\left(e^{\large i\pi n}-e^{\large -i\pi n}\right) =\\
>&=\frac{Ti}{2\pi n}\underbrace{\cos(\pi n)}_{\large(-1)^n}-\frac{2Ti}{4\pi^2n^2}\underbrace{\sin(\pi n)}_{\large0} =\\
>&=\frac{Ti}{2\pi n}(-1)^n\\ \\ \hline \\
>c_0&= \frac{1}{T}\int_{-T/2}^{T/2}t\ dt = 0
>\end{align*}
>$$

>[!summary] Calcolo dell'integrale per scomposizione dell'esponenziale
>TODO
#### Esercizio 2
Dato un segnale $x(t)$ di periodo $T$ che vale $t$ all'interno del periodo, calcolare la sua rappresentazione in serie di Fourier.

>[!info] Grafico del segnale
>![[sawtooth-traslato.png]]
>$$
>\text{Gli assi sono in scala di }T
>$$

Si può calcolare la serie di Fourier di questo segnale riscrivendolo in funzione del segnale dell'esercizio sopra e applicando le proprietà della serie.
$$
y(t)=x\underbrace{\left(t-\frac{T}{2}\right)}_{\mathclap{\text{traslazione nel tempo}}}+\frac{T}{2}
$$
Si applica la proprietà della traslazione nel tempo.
$$
d_n= c_n \cdot e^{\large-\frac{i2\pi nt_0}{T}}=c_n \cdot e^{\large-\frac{i2\pi nT}{2T}}=c_n \cdot e^{\large-i\pi n}
$$
Sostituendo a $c_n$ i reali coefficienti si ottiene il risultato.
$$
\begin{align*}
d_n &= \frac{Ti}{2\pi n}(-1)^ne^{\large-i\pi n}= \frac{Ti}{2\pi n}\underbrace{(-1)^n(-1)^n}_{\large 1} = \frac{Ti}{2\pi n} \\
d_0 &= c_0 + \frac{T}{2} = \frac{T}{2}
\end{align*}
$$
### Onda triangolare
Dato un treno triangolare di periodo $T$ e base $\dfrac{\tau}{2}$, calcolare la sua rappresentazione in serie di Fourier.
$$
c_n=\frac{1}{T}\int_{-T/2}^{T/2}\left(1-\left|\frac{2t}{\tau}\right|\right)e^{\large-i\frac{2\pi nt}{T}}dt
$$
La funzione ha valore solo tra $-\dfrac{\tau}{2}$ e $\dfrac{\tau}{2}$.
$$
\begin{align*}
c_n&=\frac{1}{T}\int_{-\tau/2}^{\tau/2}\left(1-\left|\frac{2t}{\tau}\right|\right)e^{\large-i\frac{2\pi nt}{T}}dt =\\
&= \frac{1}{T}\int_{-\tau/2}^{\tau/2}e^{\large-i\frac{2\pi nt}{T}}dt - \frac{1}{T}\int_{-\tau/2}^{\tau/2}\left|\frac{2t}{\tau}\right|e^{\large-i\frac{2\pi nt}{T}}dt\\
\end{align*}
$$

Il secondo integrale viene scomposto nei due casi del valore assoluto $\left|\dfrac{2t}{\tau}\right|$.
$$
\begin{align*}
c_n &= \frac{1}{T}\int_{-\tau/2}^{\tau/2}e^{\large-i\frac{2\pi nt}{T}}dt + \frac{2}{T\tau}\int_{-\tau/2}^{0}t\cdot e^{\large-i\frac{2\pi nt}{T}}dt - \frac{2}{T\tau}\int_{0}^{\tau/2}t\cdot e^{\large-i\frac{2\pi nt}{T}}dt \\
&= \frac{\tau}{T}\ \operatorname{sinc}\left(\frac{n\tau}{T}\right) - \frac{2}{T\tau}\int_{0}^{\tau/2}t\cdot e^{\large i\frac{2\pi nt}{T}}dt - \frac{2}{T\tau}\int_{0}^{\tau/2}t\cdot e^{\large-i\frac{2\pi nt}{T}}dt
\end{align*}
$$

Unendo gli integrali si possono trasformare gli esponenziali in un coseno.
$$
\begin{align*}
c_n&= \frac{\tau}{T}\ \operatorname{sinc}\left(\frac{n\tau}{T}\right) - \frac{4}{T\tau}\int_{0}^{\tau/2}t\cdot\cos\left(\frac{2\pi nt}{T}\right)dt =\\
&= \frac{\tau}{T}\ \operatorname{sinc}\left(\frac{n\tau}{T}\right) - \frac{4}{T\tau}\left|\frac{t\cdot \sin\left(\frac{2\pi nt}{T}\right)}{\frac{2\pi nt}{T}}\right|_{0}^{\tau/2} + \frac{4}{T\tau}\int_{0}^{\tau/2}\frac{\sin\left(\frac{2\pi nt}{T}\right)}{\frac{2\pi n}{T}}dt =\\
&= \frac{\tau}{T}\ \operatorname{sinc}\left(\frac{n\tau}{T}\right) - \frac{4}{T\tau}\frac{\frac{\tau}{2}\sin\left(\frac{\pi n\tau}{T}\right)}{\frac{\pi n\tau}{T}} - \frac{2}{\pi n\tau}\left|\frac{\cos\left(\frac{2\pi nt}{T}\right)}{\frac{2\pi n}{T}}\right|_{0}^{\tau/2} = \\
&= \frac{\tau}{T}\ \operatorname{sinc}\left(\frac{n\tau}{T}\right) - \frac{\tau}{T}  \frac{\sin\left(\frac{\pi n\tau}{T}\right)}{\frac{\pi n\tau}{T}} + \frac{T}{\pi^2 n^2\tau}\left[\cos\left(\frac{\pi n\tau}{T}\right)-1\right]
\end{align*}
$$

I primi due termini si annullano a vicenda.
$$
c_n= \frac{T}{\pi^2 n^2\tau}\left[\cos\left(\frac{\pi n\tau}{T}\right)-1\right]
$$

Applicando la [[Formule Trigonometriche#Formule di duplicazione|formula di duplicazione]] del coseno, si ottiene una funzione con un singolo termine.
$$
c_n = \frac{2T}{\pi^2 n^2\tau}\sin^2\left(\frac{\pi n\tau}{2T}\right)=\frac{\tau}{2T}\operatorname{sinc}^2\left(\frac{n\tau}{2T}\right)
$$

### Somma di esponenziali
Dato il seguente segnale, calcolare coefficienti di Fourier e potenza del segnale.
$$
x(t)=2e^{i\frac{2\pi t}{T_0}}+e^{-i\frac{2\pi t}{4T_0}}
$$
Il periodo del segnale vale $4T_0$.

>[!summary] Calcolo della potenza con l'integrale
>$$
>\begin{align*}
>P_x&=\frac{1}{4T_0}\int_{-2T_0}^{2T_0}\left[4+1+4\cos\left(\frac{2\pi t}{T_0}+\frac{2\pi t}{4T_0}\right)\right]dt=\\
>&=5 + \frac{1}{T_0}\int_{-2T_0}^{2T_0}\underbrace{\cos\left(\frac{5\pi t}{2T_0}\right)}_{\mathclap{\text{periodo }\frac{4}{5}T_0}}dt
>\end{align*}
>$$
>L'integrale è definito su un multiplo del periodo del coseno, quindi l´integrale sarà nullo.
>$$
>P_x=5
>$$

>[!summary] Calcolo dei coefficienti per confronto diretto
>Il segnale è già una somma di armoniche quindi si possono ricavare facilmente i coefficienti di Fourier.
>$$
x(t)=2e^{i\frac{2\pi t}{T_0}}+e^{-i\frac{2\pi t}{4T_0}}=2\underbrace{e^{i\frac{8\pi t}{4T_0}}}_{n=4}+\underbrace{e^{-i\frac{2\pi t}{4T_0}}}_{n=-1}
>$$
>I coefficienti sono $c_{-1}=1$ e $c_4=2$. Tutti i rimanenti $c_n$ sono nulli.

Questo si può anche ricavate attraverso la trasformata di Fourier.
$$
X(f)=2\delta\left(f-\frac{1}{T_0}\right)+\delta\left(f+\frac{1}{4T_0}\right)
$$

