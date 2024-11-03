---
tags:
  - proprietà
  - segnale
---
Tutti i processi fisici sono mediati attraverso **trasferimenti di energia**. Un sistema fisico reale risponde all'energia fornita da un'eccitazione o sollecitazione applicata ad esso. I segnali rappresentano, appunto, tali eccitazioni o sollecitazioni ed hanno, pertanto, un **contenuto energetico**.

Il contenuto energetico di un segnale dipende dalla sua natura fisica. Ma per poter trattare un segnale in maniera generica ed astratta si definiscono le quantità di **Potenza ed Energia di un segnale**. Tali quantità sono differenti dalla Potenza ed Energia fisiche ma sono ad esse **proporzionali**.

> [!summary] Considerazioni
> - Può valere al minimo $0$ in quanto l'integrando è sempre positivo
> - Può valere $0$ solo quando il segnale è nullo
> - Non esistono nella realtà segnali a energia o potenza infinita
## Segnali analogici
### Energia di un segnale analogico
$$
E_x = \lim\limits_{\Delta T \to\infty} \int_{-\Delta T/2}^{\Delta T/2} |x(t)|^2\ dt = \int_{-\infty}^{\infty}|x(t)|^2\ dt
$$
### Potenza di un segnale analogico
$$
P_x = \lim\limits_{\Delta T \to\infty}\frac{1}{\Delta T} \int_{-\Delta T/2}^{\Delta T/2} |x(t)|^2\ dt
$$
>[!info] Potenza di un segnale [[Funzione Periodica|periodico]]
>Sia $T$ il periodo del segnale, allora il limite potrà essere riscritto come
>$$
>\begin{align*}
>P_x &= \lim\limits_{n \to\infty}\frac{1}{n T} \int_{-nT/2}^{nT/2} |x(t)|^2\ dt =\\
>&= \lim\limits_{n \to\infty}\frac{1}{n T}\ n\int_{-T_0/2}^{T/2} |x(t)|^2\ dt = \\
>&= \frac{1}{T}\ \int_{-T/2}^{T/2} |x(t)|^2\ dt
>\end{align*}
>$$
## Segnali tempo-discreti
### Energia di un segnale discreto
$$
\sum_{n=-\infty}^{\infty}\Big|x[n]\Big|^2
$$
### Potenza di un segnale discreto
$$
\lim\limits_{N\to\infty} \frac{1}{2N+1}\sum_{n=-N}^{N}\Big|x[n]\Big|^2
$$
>[!info] Potenza di un segnale [[Funzione Periodica|periodico]]
>$$
>\frac{1}{T}\sum_{n=0}^{T-1}\Big|x[n]\Big|^2
>$$
## Esercizi

### Una [[Segnale di Gauss|gaussiana]]
Si definisce $t'=\sqrt{2a}t$ e si calcola l'integrale di Gauss.
$$
E_x = \int_{-\infty}^{\infty} |Ae^{-at^2}|^2\ dt = A^2 \int_{-\infty}^{\infty} \dfrac{e^{-t'^2}}{\sqrt{2a}}\ dt' = A^2 \frac{\pi}{\sqrt{2a}}
$$
Dato che ha un valore non nullo, allora è un segnale di energia e ha potenza nulla.
### Una [[Segnale Rampa|rampa]]
Dato un segnale rampa $x(t) = t\cdot u(t)$ si calcola l'integrale di potenza.
$$
\begin{align*}
P_x&= \lim\limits_{\Delta T \to\infty} \frac{1}{\Delta T} \int_{-\Delta T/2}^{\Delta T/2} |t\cdot u(t)|^2\ dt = \lim\limits_{\Delta T \to\infty}\frac{1}{\Delta T} \int_{0}^{\Delta T/2} t^2\ dt =\\
&= \lim\limits_{\Delta T \to\infty} \frac{1}{\Delta T} \frac{t^3}{3} \Big|_0^{\Delta T/2} = \lim\limits_{\Delta T \to\infty} \frac{\Delta T^2}{24} =\\
&= \infty 
\end{align*}
$$
Il risultato non è un valore finito, quindi si passa a calcolare l'integrale di energia.
$$
E_x = \int_{0}^{\infty}t^2\ dt = \infty
$$
Anche l'energia del segnale è infinita, quindi non è un segnale di potenza né di energia.
### Esponenziale complesso
Dato un segnale esponenziale complesso $x(t) = -Ae^{i2\pi f_0 t}$ si calcola l'integrale di energia.

Per la [[Numeri Complessi#Formula di Eulero|formula di Eulero]], l'esponenziale può essere trasformato in $cos(2\pi f_0 t) + i\ sin(2\pi f_0 t)$.
$$
E_x = \int_{-\infty}^{\infty} A^2|cos(2\pi f_0 t) + i\ sin(2\pi f_0 t)|^2\ dt
$$
La somma di seno e coseno al quadrato è sempre uguale a $1$ quindi si può eliminare.
$$
E_x = \int_{-\infty}^{\infty} A^2\ dt = \infty
$$
Il segnale non è un segnale di energia. Si calcola quindi l'integrale di potenza.
$$
\begin{align*}
P_x&= \lim\limits_{\Delta T\to\infty}\frac{1}{\Delta T}\int_{-\Delta T/2}^{\Delta T/2}A^2\ dt = \lim\limits_{\Delta T\to\infty}\frac{1}{\Delta T}\ A^2\ t\Big|_{-\Delta T/2}^{\Delta T/2} =\\
&= \lim\limits_{\Delta T \to\infty} \frac{1}{\Delta T}\ \frac{\Delta T}{2}\ A^2 = \frac{A^2}{2}
\end{align*}
$$
Il risultato è un valore finito, quindi questo segnale è un segnale di potenza.
### Esponenziale unilatero
$$
E_x = \int_{-\infty}^{\infty}|e^{-\alpha t}u(t)|^2\ dt = \int_{0}^{\infty} e^{-2\alpha t}\ dt = \frac{e^{-2\alpha t}}{-2\alpha t}\Big|_0^{\infty} = \frac{1}{2\alpha}
$$
Il risultato è un valore finito, quindi l'esponenziale unilatero è un segnale di energia.
### Coseno
La funzione coseno è una funzione periodica quindi si può usare la forma alternativa dell'integrale di potenza, sapendo che il periodo vale $\frac{1}{f_0}$.
$$
P_x = \frac{1}{T}\ \int_{-T/2}^{T/2} |A\ cos(2\pi f_0t)|^2\ dt = f_0A^2\int_{-1/2f_0}^{1/2f_0} cos^2(2\pi f_0 t)\ dt
$$
Usando la [[Formule Trigonometriche#Formule di duplicazione|formula di duplicazione]] si ottengono questi due integrali. Visto che l'intervallo di integrazione è simmetrico, l'integrale del coseno si annulla e rimane solo quello di $1/2$.
$$
P_x = f_0A^2\int_{-1/2f_0}^{1/2f_0} \frac{cos(4\pi f_0 t)+1}{2}\ dt = f_0 \frac{1}{2} \frac{1}{f_0}A^2 = \frac{A^2}{2}
$$
Il risultato è un valore finito, quindi la funzione coseno è un segnale di potenza.
### Somma di un [[Segnale Gradino|gradino]] e una costante
Dato il segnale $x(t) = 1 + u(t)$ si calcola l'integrale di potenza.
$$
P_x=\lim\limits_{\Delta T\to\infty}\frac{1}{\Delta T}\int_{-\Delta T/2}^{\Delta T/2} |1+u(t)|^2 dt
$$
Il segnale è costante a parti e vale $1$ per $t<0$ e $2$ per $t\ge0$. L'integrale può essere diviso in due parti.
$$
\begin{alignat*}{5}
P_x &= \lim\limits_{\Delta T\to\infty}\frac{1}{\Delta T}\int_{-\Delta T/2}^{0} |x(t)|^2 dt &&+ \lim\limits_{\Delta T\to\infty}\frac{1}{\Delta T}\int_{0}^{\Delta T/2} |x(t)|^2 dt &&=\\
&= \lim\limits_{\Delta T\to\infty}\frac{1}{\Delta T}\int_{-\Delta T/2}^{0} dt &&+ \lim\limits_{\Delta T\to\infty}\frac{1}{\Delta T}\int_{0}^{\Delta T/2} 4\ dt &&=\\
&= \lim\limits_{\Delta T\to\infty}\frac{1}{\Delta T}\frac{\Delta T}{2} &&+ \lim\limits_{\Delta T\to\infty}\frac{1}{\Delta T}\frac{4\Delta T}{2} &&= \frac{5}{2}
\end{alignat*}
$$

Il segnale ha una valore di potenza finito, quindi è un segnale di potenza.
### Somma di un [[Segnale Finestra|rettangolo]] e un [[Segnale Finestra|triangolo]]
Dato il segnale $x(t) = \operatorname{rect}\left(\frac{t}{2T}\right) + \operatorname{tri}\left(\frac{t}{T}\right)$ si calcola l'integrale di energia.
$$
E_x=\int_{-\infty}^{\infty}\left|\operatorname{rect}\left(\frac{t}{2T}\right) + \operatorname{tri}\left(\frac{t}{T}\right)\right|^2 dt
$$
Il segnale è nullo per valori di $-T<t<T$, quindi la finestra di integrazione può essere ridotta.
$$
E_x=\int_{-T}^{T}\left|\operatorname{rect}\left(\frac{t}{2T}\right) + \operatorname{tri}\left(\frac{t}{T}\right)\right|^2 dt
$$
Adesso il segnale si può nella sua funzione analitica equivalente, tenendo conto che in questo intervallo il rettangolo è una costante unitaria.
$$
x(t) = 2 - \left|\frac{t}{T}\right|\quad\to\quad -T<t<T
$$
La funzione è pari quindi l'integrale può essere calcolato da un singolo lato e moltiplicato per $2$.
$$
\begin{align*}
E_x&=2\int_{0}^{T}\left(2 - \frac{t}{T}\right)^2dt = 2\int_{0}^{T}\left(4 -\frac{4t}{T}+\frac{t^2}{T^2}\right)dt =\\
&= 2\left[4t -\frac{2t^2}{T}+\frac{t^3}{3T^2}\right]_0^T = \frac{14}{3}T
\end{align*}
$$
Il risultato è un valore finito, quindi è un segnale di energia.
### Prodotto di un [[Segnale Finestra|rettangolo]] e un [[Segnale Finestra|triangolo]]
Dato il segnale $x(t) = \operatorname{rect}\left(\frac{t}{T}\right) \cdot \operatorname{tri}\left(\frac{t}{T}\right)$ si calcola l'integrale di energia.
$$
E_x=\int_{-\infty}^{\infty}\left|\operatorname{rect}\left(\frac{t}{T}\right) \cdot \operatorname{tri}\left(\frac{t}{T}\right)\right|^2 dt
$$
Il segnale è nullo per valori di $-T/2<t<T/2$, quindi la finestra di integrazione può essere ridotta.
$$
E_x=\int_{-T/2}^{T/2}\left|\operatorname{tri}\left(\frac{t}{T}\right)\right|^2 dt = \int_{-T/2}^{T/2}\left(1-\left|\frac{t}{T}\right|\right)^2dt
$$
Sapendo che la funzione è pari, la finestra di integrazione può essere dimezzata.
$$
E_x=2\int_{0}^{T/2}\left(1-\frac{2t}{T}+\frac{t^2}{T^2}\right)dt = 2 \left[t-\frac{t^2}{T}+\frac{t^3}{3T^2}\right]_{0}^{T/2}=\frac{7}{12}T
$$
Il risultato è un valore finito, quindi è un segnale di energia.
### Finestra della somma tra seno e coseno 
Dato il seguente segnale, calcolare la sua energia.
$$
x(t)=\left[\cos\left(\frac{\pi t}{4}\right)+\sin\left(\frac{\pi t}{4}\right)\right]\operatorname{rect}\left(t-\frac{1}{2}\right)
$$
La finestra di integrazione può essere ridotta alla grandezza della finestra rettangolare.
$$
\begin{align*}
E_x &= \int_{-\infty}^{\infty}\left|\left[\cos\left(\frac{\pi t}{4}\right)+\sin\left(\frac{\pi t}{4}\right)\right]\operatorname{rect}\left(t-\frac{1}{2}\right)\right|^2dt = \\
&=\int_{0}^{1}\left[\cos\left(\frac{\pi t}{4}\right)+\sin\left(\frac{\pi t}{4}\right)\right]^2dt = \\
&= \int_{0}^{1}\left[\cos^2\left(\frac{\pi t}{4}\right)+2\cos\left(\frac{\pi t}{4}\right)\sin\left(\frac{\pi t}{4}\right)+\sin^2\left(\frac{\pi t}{4}\right)\right]\ dt
\end{align*}
$$
Si applica la [[Formule Trigonometriche#Identità fondamentale della Trigonometria|relazione fondamentale della trigonometria]].
$$
E_x = \int_{0}^{1}\left[1 + 2\cos\left(\frac{\pi t}{4}\right)\sin\left(\frac{\pi t}{4}\right)\right]\ dt
$$
Si applica la [[Formule Trigonometriche#Formule di duplicazione|formula di duplicazione]] del seno.
$$
E_x= 1 + \int_{0}^{1}\sin\left(\frac{\pi t}{2}\right)\ dt = 1 - \frac{2}{\pi}\cos\left.\left(\frac{\pi t}{2}\right)\right|_{0}^{1} = 1 + \frac{2}{\pi}
$$
Il risultato è un valore finito, quindi è un segnale di energia.
### Combinazione lineare complessa di seno e coseno
Dato il seguente segnale, si calcola l'integrale di potenza.
$$
x(t)=2\cos\left(\frac{\pi t}{\tau}\right)+i\sin\left(\frac{\pi t}{\tau}\right)
$$
Sapendo che il segnale è periodico si può usare la formula per la potenza di un segnale periodico.
$$
\begin{align*}
P_x&=\frac{1}{2\tau}\int_{0}^{2\tau}\left|2\cos\left(\frac{\pi t}{\tau}\right)+i\sin\left(\frac{\pi t}{\tau}\right)\right|^2dt=\\
&=\frac{1}{2\tau}\int_{0}^{2\tau}4\cos^2\left(\frac{\pi t}{\tau}\right)+\sin^2\left(\frac{\pi t}{\tau}\right)\ dt
\end{align*}
$$
Si applica la [[Formule Trigonometriche#Identità fondamentale della Trigonometria|relazione fondamentale della trigonometria]].
$$
P_x = \frac{1}{2\tau}\int_{0}^{2\tau} \left[1 + 3\cos^2\left(\frac{\pi t}{\tau}\right)\right]dt
$$
Si applica la [[Formule Trigonometriche#Formule di duplicazione|formula di duplicazione]] del coseno.
$$
P_x=\frac{1}{2\tau}\int_{0}^{2\tau} 1 + 3\left[\frac{1}{2}+ \frac{1}{2}\cos\left(\frac{2\pi t}{\tau}\right)\right]dt
$$
Ora l'integrale può essere separato in una parte costante e una parte funzione del coseno.
$$
P_x=\frac{1}{2\tau}\int_{0}^{2\tau} \frac{5}{2}\ dt + \frac{1}{2\tau}\int_{0}^{2\tau} \frac{3}{2}\cos\left(\frac{2\pi t}{\tau}\right)\ dt
$$
L'integrale su $k\in\mathbb{Z}$ volte il periodo di una funzione è nullo.
$$
P_x = \frac{5}{2}\frac{1}{2\tau}\int_{0}^{2\tau}dt = \frac{5}{2}
$$
Il risultato è un valore finito, quindi è un segnale di potenza.

### Prodotto tra [[Seno Cardinale]] e coseno
Dato il seguente segnale, calcolare la sua energia.
$$
x(t)=\frac{\sin(\pi t)}{\pi t}\cos(2\pi at)
$$
Calcolare l'integrale nel tempo è complicato quindi si applica il [[Trasformata di Fourier#Teorema di Parseval|Teorema di Parseval]].

>[!summary] Calcolo trasformata con la [[Numeri Complessi#Rappresentazione esponenziale di seno e coseno|scomposizione in esponenziali]]
>$$
>\begin{align*}
>x(t)&=\operatorname{sinc}(t)\cos(2\pi at)=\operatorname{sinc}(t)\frac{e^{i2\pi at}+e^{-i2\pi at}}{2}\\
>X(f)&=\frac{1}{2}\operatorname{rect}(f+a)+\frac{1}{2}\operatorname{rect}(f-a)
>\end{align*}
>$$

>[!summary] Calcolo trasformata con la [[Trasformata di Fourier#Convoluzione|proprietà di convoluzione]]
>$$
>\begin{align*}
>X(f)&=\operatorname{sinc}(t)*\cos(2\pi at) =\\
>&=\operatorname{rect}(f)*\frac{1}{2}[\delta(f-a)+\delta(f+a)]=\\
>&=\frac{1}{2}\operatorname{rect}(f+a)+\frac{1}{2}\operatorname{rect}(f-a)
>\end{align*}
>$$

Si può notare come, per valori di $a<1/2$, i due rettangoli si sovrappongono nell'origine.

>[!info] Grafico per $a<1/2$
>![[aliasing.svg|center|600]]
#### Caso $a>1/2$
Le due finestre non si sovrappongono e si possono calcolare i due integrali separati.
$$
E_x=\int_{a-1/2}^{a+1/2}\frac{1}{4}df +\int_{-a-1/2}^{-a+1/2}\frac{1}{4}df = \frac{1}{2}
$$
#### Caso $a<1/2$
Attorno all'origine le due finestre si sovrappongono quindi bisogna tenerne conto nel calcolo degli integrali.
$$
\begin{align*}
E_x&=\int_{-a+1/2}^{a+1/2}\frac{1}{4}df +\int_{-a-1/2}^{a-1/2}\frac{1}{4}df + \int_{a-1/2}^{-a+1/2} df=\\
&=\frac{2a}{4}+\frac{2a}{4}+1-2a=\\
&=1-a
\end{align*}
$$
