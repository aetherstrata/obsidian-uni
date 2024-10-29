---
tags:
  - trasformata
  - analisi
  - segnale
---
La trasformata di Fourier è una generalizzazione della [[Serie di Fourier]], capace di operare anche su [[Risposta Impulsiva|segnali impulsivi]], che ha uno spazio delle basi infinito **non** numerabile, al contrario della serie.
$$x(t)=\int_{-\infty}^{\infty}X(f)\cdot e^{\large i2\pi ft}df \quad\longrightarrow\quad
 \begin{matrix*}[c,l]
 X(f) & \text{trasformata di Fourier}\\
 e^{\large i2\pi ft} & \text{nucleo di trasformazione}
 \end{matrix*}$$
L'insieme dei coefficienti definisce la trasformata di Fourier del segnale.
$$X(f)=\int_{-\infty}^{\infty}x(t)\cdot e^{\large -i2\pi ft}dt $$
## Definizione
A differenza della serie di Fourier, nella trasformata è necessario definire il prodotto scalare per qualsiasi funzione, non solo quelle [[Funzione Periodica|periodiche]].
$$\left<x_1(t),x_2(t)\right>=\int_{-\infty}^{\infty}x_1(t)x_2^*(t)dt$$
>[!hint] Questo integrale è uguale alla [[Correlazione]] a ritardo nullo tra i due segnali
### Basi
Le armoniche definiscono le basi ortonormali dello spazio vettoriale.

Questo si può dimostrare facendo il prodotto scalare tra due basi.
$$\begin{align}
\left<e^{\large i2\pi f_1t},e^{\large i2\pi f_2t}\right> &=\lim_{\Delta T\to\infty}\int_{-\Delta T/2}^{\Delta T/2}e^{\large i2\pi (f_1-f_2)t}dt =\\
&= \lim_{\Delta T\to\infty} \left.\frac{e^{\large i2\pi(f_1-f_2)t}}{i2\pi(f_1-f_2)}\right|_{-\Delta T/2}^{\Delta T/2} =\\
&= \lim_{\Delta T\to\infty}\Delta T \frac{\sin(\pi(f_1-f_2)\Delta T)}{\pi(f_1-f_2)\Delta T} =\\
&=\lim_{\Delta T\to\infty}\Delta T \operatorname{sinc}(\Delta T(f_1-f_2))=\\
&= \delta(f_1-f_2)
\end{align}$$
Come per la serie di Fourier, è dimostrato che le armoniche sono ortogonali tra loro.
### Trasformata
Facendo il prodotto scalare della funzione per un suo versore, si può estrarre il suo coefficiente per l'armonica scelta.
$$X(f)=\left<x(t),e^{\large i2\pi ft}\right> =\int_{-\infty}^{\infty}x(t)e^{\large -i2\pi ft}dt$$
## Proprietà
### Valore atteso
Il valore in $0$ della trasformata è il valor medio del segnale.
$$X(0)=\int_{-\infty}^{\infty}x(t)e^{\large -i2\pi ft}dt=\int_{-\infty}^{\infty}x(t)dt = \mathbb{E}(x)$$
### Linearità
Ad una combinazione lineare di segnali nel tempo corrisponde una combinazione lineare di spettri di frequenze.
$$\begin{gather}
\begin{aligned}
x_1(t)&\overset{\mathcal{F}}{\longrightarrow}X_1(f)\\
x_2(t)&\overset{\mathcal{F}}{\longrightarrow}X_2(f)\\\\ \hline
\end{aligned}\\\\\begin{aligned}
ax_1(t)+bx_2(t)\overset{\mathcal{F}}{\longrightarrow}&\int_{-\infty}^{\infty}\left[ax_1(t)+bx_2(t)\right]e^{\large -i2\pi ft}dt=\\
={}&a\int_{-\infty}^{\infty}x_1(t)e^{\large -i2\pi ft}dt+b\int_{-\infty}^{\infty}x_2(t)e^{\large -i2\pi ft}dt =\\
={}&aX_1(f)+bX_2(f)
\end{aligned}
\end{gather}$$
### Dualità
$$x(t)\overset{\mathcal{F}}{\longrightarrow}X(f)\overset{\mathcal{F}}{\longrightarrow}x(-t)$$
#### Dimostrazione
$$\int_{-\infty}^{\infty}X(t)e^{\large -i2\pi ft}dt = \int_{-\infty}^{\infty} X(f')e^{\large -i2\pi t'f'}df' = x(-t')$$
#### Esempio

$$\operatorname{rect}(t)\overset{\mathcal{F}}{\longrightarrow}\operatorname{sinc}(f)\overset{\mathcal{F}}{\longrightarrow}\operatorname{rect}(-t)$$
### Scala
$$\begin{align}
x(at)\overset{\mathcal{F}}{\longrightarrow}{}&\int_{-\infty}^{\infty}x(at)e^{\large -i2\pi ft}dt =\\
{}\xlongequal{at=t'}{}&\int_{-\infty}^{\infty}x(t')e^{\large -i2\pi f\Large\frac{t'}{a}}\frac{dt'}{|a|}=\\
={}&\frac{1}{|a|}\int_{-\infty}^{\infty}x(t')e^{\large -i2\pi \Large\frac{f}{a}t'}dt'=\\
={}&\frac{1}{|a|}X\left(\frac{f}{a}\right)
\end{align}$$
#### Esempio
Conoscendo la trasformata di una [[Segnale Finestra#Finestra rettangolare|finestra rettangolare]] unitaria, si può calcolare la trasformata di una finestra rettangolare di base arbitraria senza l'integrale, usando questa proprietà.
$$\operatorname{rect}\left(\frac{t}{\tau}\right)\overset{\mathcal{F}}{\longrightarrow}\tau\operatorname{sinc}(f\tau)$$
## Esempi
### Trasformata di una [[Segnale Finestra#Finestra rettangolare|finestra rettangolare]]
Dato un impulso rettangolare $x(t)=\operatorname{rect}\left(\dfrac{t}{\tau}\right)$, calcolare la sua trasformata di Fourier.
$$\begin{align}
X(f)&=\int_{-\infty}^{\infty}\operatorname{rect}\left(\dfrac{t}{\tau}\right) e^{\large -i2\pi ft}dt =
 \int_{-\tau/2}^{\tau/2}e^{\large -i2\pi ft}dt =\\
&= \left.\frac{e^{\large -i2\pi ft}}{-i2\pi f}\right|_{-\tau/2}^{\tau/2}=\frac{e^{\large -i2\pi f\frac{\Large\tau}{2}}-e^{\large i2\pi f\frac{\Large\tau
}{2}}}{ -i2\pi f}=\\
&=\frac{\sin\left(\pi f\tau\right)}{\pi f} =\tau\operatorname{sinc}(f\tau)
\end{align}$$
### Trasformata di una [[Delta di Dirac]]
Dato un impulso matematico $x(t)=\delta(t)$, calcolare la sua trasformata di Fourier.
$$\begin{align}
X(f)&=\int_{-\infty}^{\infty}\delta(t)e^{\large -i2\pi ft}dt =\\
&=\int_{-\infty}^{\infty}\delta(t)dt=\\
&= 1
\end{align}$$
Qui si nota una particolare relazione tra la rappresentazione nel tempo e in frequenza del segnale.

>[!important] Tanto più è limitato nel tempo, tanto più è largo il contenuto spettrale del segnale
### Trasformata di una [[Delta di Dirac]] traslata
Dato un impulso matematico traslato $x(t)=\delta(t-t_0)$, calcolare la sua trasformata di Fourier.
$$\begin{align}
X(f)&=\int_{-\infty}^{\infty}\delta(t-t_0)e^{\large -i2\pi ft}dt =\\
&=e^{\large i2\pi ft_0}\int_{-\infty}^{\infty}\delta(t-t_0)dt=\\
&=e^{\large i2\pi ft_0}
\end{align}$$
### Trasformata di un armonica
Dato un segnale armonico $x(t)=e^{\large i2\pi f_0t}$, calcolare la sua trasformata di Fourier.
$$\begin{align}
X(f)&=\int_{-\infty}^{\infty}e^{\large i2\pi f_0t}e^{\large -i2\pi ft}dt =\\
&=\int_{-\infty}^{\infty}e^{\large i2\pi (f_0-f)t}dt=\\
&= \lim_{\Delta T\to\infty}\left.\frac{e^{\large i2\pi (f_0-f)t}}{i2\pi(f_0-f)}\right|_{-\Delta T/2}^{\Delta T/2}=\\
&=\lim_{\Delta T\to\infty}\frac{e^{\large i\pi(f_0-f)\Delta T}-e^{\large-i\pi(f_0-f)\Delta T}}{i2\pi(f_0-f)} =\\
&= \lim_{\Delta T\to\infty}\frac{\sin\left(\pi(f_0-f)\Delta T\right)}{\pi(f_0-f)}= \\
&=\lim_{\Delta T\to\infty}\Delta T \operatorname{sinc}((f_0-f)\Delta T)=\\
&= \delta(f_0-f)=\delta(f-f_0)
\end{align}$$
### Trasformata di un coseno
Dato un segnale coseno $x(t)=\cos(2\pi f_0t)$, calcolare la sua trasformata di Fourier.
$$\begin{align}
X(f)&=\int_{-\infty}^{\infty}\cos(2\pi f_0t)e^{\large -i2\pi ft}dt =\\
&=\int_{-\infty}^{\infty}\frac{e^{\large i2\pi f_0t}+e^{\large -i2\pi f_0t}}{2}e^{\large -i2\pi ft}dt=\\
&=\int_{-\infty}^{\infty}\frac{e^{\large -i2\pi (f-f_0)t}}{2}dt+\int_{-\infty}^{\infty}\frac{e^{\large -i2\pi (f+f_0)t}}{2}dt =\\
&=\frac{1}{2}\delta(f-f_0)+\frac{1}{2}\delta(f+f_0)
\end{align}$$
### Trasformata di un seno
Dato un segnale seno $x(t)=\sin(2\pi f_0t)$, calcolare la sua trasformata di Fourier.
$$\begin{align}
X(f)&=\int_{-\infty}^{\infty}\sin(2\pi f_0t)e^{\large -i2\pi ft}dt =\\
&=\int_{-\infty}^{\infty}\frac{e^{\large i2\pi f_0t}-e^{\large -i2\pi f_0t}}{2i}e^{\large -i2\pi ft}dt=\\
&=\int_{-\infty}^{\infty}\frac{e^{\large -i2\pi (f-f_0)t}}{2i}dt-\int_{-\infty}^{\infty}\frac{e^{\large -i2\pi (f+f_0)t}}{2i}dt =\\
&=\frac{1}{2i}\delta(f-f_0)-\frac{1}{2i}\delta(f+f_0)=\\
&=-\frac{i}{2}\delta(f-f_0)+\frac{i}{2}\delta(f+f_0)
\end{align}$$
