---
tags:
  - trasformata
  - analisi
  - segnale
  - operatore
  - matematica
---
La trasformata di Fourier è una generalizzazione della [[Serie di Fourier]], capace di operare anche su [[Filtro#Risposta Impulsiva|segnali impulsivi]], che ha uno spazio delle basi infinito **non** numerabile, al contrario della serie.
$$
x(t)=\int_{-\infty}^{\infty}X(f)\cdot e^{i2\pi ft}df \quad\longrightarrow\quad
 \begin{cases}
 \mathrel{}X(f) & \text{trasformata di Fourier}\\
 e^{i2\pi ft} & \text{nucleo di trasformazione}
 \end{cases}
$$
L'insieme dei coefficienti definisce la trasformata di Fourier del segnale.
$$
X(f)=\int_{-\infty}^{\infty}x(t)\cdot e^{-i2\pi ft}dt
$$
## Definizione
A differenza della serie di Fourier, nella trasformata è necessario definire il prodotto scalare per qualsiasi funzione, non solo quelle [[Funzione Periodica|periodiche]].
$$
\left<x_1(t),x_2(t)\right>=\int_{-\infty}^{\infty}x_1(t)x_2^*(t)dt
$$
>[!hint] Questo integrale è uguale alla [[Correlazione]] a ritardo nullo tra i due segnali
### Basi
Le armoniche definiscono le basi ortonormali dello spazio vettoriale.

Questo si può dimostrare facendo il prodotto scalare tra due basi.
$$
\begin{align*}
\left<e^{i2\pi f_1t},e^{i2\pi f_2t}\right> &=\lim_{\Delta T\to\infty}\int_{-\Delta T/2}^{\Delta T/2}e^{i2\pi (f_1-f_2)t}dt =\\
&= \lim_{\Delta T\to\infty} \left.\frac{e^{i2\pi(f_1-f_2)t}}{i2\pi(f_1-f_2)}\right|_{-\Delta T/2}^{\Delta T/2} =\\
&= \lim_{\Delta T\to\infty}\Delta T \frac{\sin(\pi(f_1-f_2)\Delta T)}{\pi(f_1-f_2)\Delta T} =\\
&=\lim_{\Delta T\to\infty}\Delta T \operatorname{sinc}(\Delta T(f_1-f_2))=\\
&= \delta(f_1-f_2)
\end{align*}
$$
Come per la serie di Fourier, è dimostrato che le armoniche sono ortogonali tra loro.
### Trasformata
Facendo il prodotto scalare della funzione per un suo versore, si può estrarre il suo coefficiente per l'armonica scelta.
$$
X(f)=\left<x(t),e^{i2\pi ft}\right> =\int_{-\infty}^{\infty}x(t)e^{-i2\pi ft}dt
$$
## Proprietà
### Valore atteso
Il valore in $0$ della trasformata è il valor medio del segnale.
$$
X(0)=\int_{-\infty}^{\infty}x(t)e^{-i2\pi ft}dt=\int_{-\infty}^{\infty}x(t)dt = \mathbb{E}(x)
$$
### Linearità
Ad una combinazione lineare di segnali nel tempo corrisponde una combinazione lineare di spettri di frequenze.
$$
\begin{gather*}
\begin{aligned}
x_1(t)&\overset{\mathcal{F}}{\longrightarrow}X_1(f)\\
x_2(t)&\overset{\mathcal{F}}{\longrightarrow}X_2(f)\\\\ \hline
\end{aligned}\\\\\begin{aligned}
ax_1(t)+bx_2(t)\overset{\mathcal{F}}{\longrightarrow}&\int_{-\infty}^{\infty}\left[ax_1(t)+bx_2(t)\right]e^{-i2\pi ft}dt=\\
={}&a\int_{-\infty}^{\infty}x_1(t)e^{-i2\pi ft}dt+b\int_{-\infty}^{\infty}x_2(t)e^{-i2\pi ft}dt =\\
={}&aX_1(f)+bX_2(f)
\end{aligned}
\end{gather*}
$$
### Dualità
$$
x(t)\overset{\mathcal{F}}{\longrightarrow}X(f)\overset{\mathcal{F}}{\longrightarrow}x(-t)
$$
#### Dimostrazione
$$
\int_{-\infty}^{\infty}X(t)e^{-i2\pi ft}dt = \int_{-\infty}^{\infty} X(f')e^{-i2\pi t'f'}df' = x(-t')
$$
#### Esempio
$$
\begin{align*}
\operatorname{rect}(t)\overset{\mathcal{F}}{\longrightarrow}{}&\operatorname{sinc}(f)\\
\operatorname{sinc}(t)\overset{\mathcal{F}}{\longrightarrow}{}&\operatorname{rect}(-f)
\end{align*}
$$
### Scala
$$
\begin{align*}
x(at)\overset{\mathcal{F}}{\longrightarrow}{}&\int_{-\infty}^{\infty}x(at)e^{-i2\pi ft}dt =\\
{}\xlongequal{at=t'}{}&\int_{-\infty}^{\infty}x(t')e^{-i2\pi f\large\frac{t'}{a}}\frac{dt'}{|a|}=\\
={}&\frac{1}{|a|}\int_{-\infty}^{\infty}x(t')e^{-i2\pi \large\frac{f}{a}t'}dt'=\\
={}&\frac{1}{|a|}X\left(\frac{f}{a}\right)
\end{align*}
$$
#### Esempio
Conoscendo la trasformata di una [[Segnale Finestra#Finestra rettangolare|finestra rettangolare]] unitaria, si può calcolare la trasformata di una finestra rettangolare di base arbitraria senza l'integrale, usando questa proprietà.
$$
\operatorname{rect}\left(\frac{t}{\tau}\right)\overset{\mathcal{F}}{\longrightarrow}\tau\operatorname{sinc}(f\tau)
$$
### Traslazione nel tempo
$$
\begin{align*}
x(t-t_0)\overset{\mathcal{F}}{\longrightarrow}&\int_{-\infty}^{\infty}x(t-t_0)e^{-i2\pi ft}dt =\\
t'\coloneqq t-t_o\quad={}&\int_{-\infty}^{\infty}x(t')e^{-i2\pi f(t'+t_0)}dt' =\\
={}&e^{-i2\pi ft_0}\int_{-\infty}^{\infty}x(t')e^{-i2\pi ft'}dt'=\\
={}&e^{-i2\pi ft_0}X(f)
\end{align*}
$$
### Derivazione
$$
\begin{align*}
\dot{x}(t)=\frac{d}{dt}x(t)&=\int_{-\infty}^{\infty}i2\pi f\cdot X(f)e^{i2\pi ft}df\\
\dot{x}(t)=\frac{d}{dt}x(t)&\overset{\mathcal{F}}{\longrightarrow}\boxed{i2\pi f\cdot X(f)}
\end{align*}
$$
#### Duale
$$
\begin{align*}
\dot{X}(f)=\frac{d}{df}X(f)&=\int_{-\infty}^{\infty}-i2\pi t\cdot x(t)e^{-i2\pi ft}dt\\
\boxed{i2\pi t\cdot x(t)}&\overset{\mathcal{F}}{\longrightarrow}-\frac{d}{df}X(f)
\end{align*}
$$
### [[Convoluzione]]
$$
z(t)=x(t)*y(t)\overset{\mathcal{F}}{\longrightarrow}X(f)\cdot Y(f)=Z(f)
$$
#### Duale
$$
z(t)=x(t)\cdot y(t)\overset{\mathcal{F}}{\longrightarrow}X(f)*Y(f)=Z(f)
$$
#### Dimostrazione
$$
\begin{align*}
z(t)=x(t)*y(t)&=\int_{-\infty}^{\infty}x(t-\tau)\underbrace{y(\tau)}_{\mathcal{F^{-1}}}d\tau=\\
&=\int_{-\infty}^{\infty}x(t-\tau)\int_{-\infty}^{\infty}Y(f)e^{i2\pi f\tau}df\ d\tau=\\
&=\int_{-\infty}^{\infty}Y(f)df\int_{-\infty}^{\infty}x(t-\tau)e^{i2\pi f\tau} d\tau=\\
t'\coloneqq t-\tau\quad&=\int_{-\infty}^{\infty}Y(f)df\int_{-\infty}^{\infty}x(t')e^ {i2\pi f(t-t')} dt'=\\
&=\int_{-\infty}^{\infty}Y(f)e^ {i2\pi ft}df\underbrace{\int_{-\infty}^{\infty}x(t')e^ {-i2\pi ft'} dt'}_{X(f)}=\\
&=\int_{-\infty}^{\infty}\boxed{Y(f)X(f)}e^ {i2\pi ft}df
\end{align*}
$$
### [[Correlazione]]
$$
z(t)=x(t)\otimes y(t)=\int_{-\infty}^{\infty}x(t+\tau)y^*(\tau)d\tau
$$
Ricordando il legame tra correlazione e convoluzione, si può applicare la proprietà di convoluzione.
$$
z(t)= x(t)*y^*(-t)=X(f)Y^*(f)
$$
### Integrazione
$$
\begin{align*}
x(t)&\overset{\mathcal{F}}{\longrightarrow}X(f)\\\\
x'(t)&=\int_{-\infty}^{\tau}x(\tau)d\tau=\int_{-\infty}^{\infty}u(t-\tau)x(\tau)d\tau\\
x'(t)&\overset{\mathcal{F}}{\longrightarrow}X(f)\left[\frac{1}{2}\delta(f)+\frac{1}{2\pi if}\right]=\\
&=\frac{1}{2}X(0)\delta(f)+\frac{X(f)}{2\pi if}
\end{align*}
$$
### Coniugazione
$$
\begin{align*}
x(t)&\overset{\mathcal{F}}{\longrightarrow}X(f)\\\\
x^*(t)&\overset{\mathcal{F}}{\longrightarrow}\int_{-\infty}^{\infty}x^*(t)e^{-i2\pi ft}dt=\left(\int_{-\infty}^{\infty}x(t)e^{i2\pi ft}dt\right)^*=X^*(-f)\\
\end{align*}
$$
## Coefficienti di un segnale periodico

La trasformata di Fourier può anche essere usata per ottenere i coefficienti di Fourier di un segnale [[Funzione Periodica|periodico]].
$$
x(t)=\sum_{n=-\infty}^{+\infty}c_ne^{i2\pi\frac{n}{T}t}\overset{\mathcal{F}}{\longrightarrow}X(f)=\sum_{n=-\infty}^{+\infty}c_n\delta(f-\frac{n}{T})
$$
Prendendo un campione $\dot{x}(t)$ del segnale periodico. si possono quindi ottenere i coefficienti attraverso la trasformata di Fourier.
$$
c_n=\int_{-\infty}^{\infty}\dot{x}(t)e^{-i2\pi\frac{n}{T}t}dt = \frac{1}{T}X\left(\frac{n}{T}\right)
$$
## Esempi
### [[Segnale Finestra#Finestra rettangolare|Finestra rettangolare]]
Dato un impulso rettangolare $x(t)=\operatorname{rect}\left(\dfrac{t}{\tau}\right)$, calcolare la sua trasformata di Fourier.
$$
\begin{align*}
X(f)&=\int_{-\infty}^{\infty}\operatorname{rect}\left(\frac{t}{\tau}\right) e^{-i2\pi ft}dt = \int_{-\tau/2}^{\tau/2}e^{-i2\pi ft}dt =\\
&= \left.\frac{e^{-i2\pi ft}}{-i2\pi f}\right|_{-\tau/2}^{\tau/2}=\frac{e^{-i2\pi f\frac{\Large\tau}{2}}-e^{i2\pi f\frac{\Large\tau}{2}}}{ -i2\pi f}=\\
&=\frac{\sin\left(\pi f\tau\right)}{\pi f} =\tau\operatorname{sinc}(f\tau)
\end{align*}
$$
### [[Delta di Dirac]]
Dato un impulso matematico $x(t)=\delta(t)$, calcolare la sua trasformata di Fourier.
$$
\begin{align*}
X(f)&=\int_{-\infty}^{\infty}\delta(t)e^{-i2\pi ft}dt =\\
&=\int_{-\infty}^{\infty}\delta(t)dt=\\
&= 1
\end{align*}
$$

Qui si nota una particolare relazione tra la rappresentazione nel tempo e in frequenza del segnale.

>[!important] Tanto più è limitato nel tempo, tanto più è largo il contenuto spettrale del segnale
### [[Delta di Dirac]] traslata
Dato un impulso matematico traslato $x(t)=\delta(t-t_0)$, calcolare la sua trasformata di Fourier.
$$
\begin{align*}
X(f)&=\int_{-\infty}^{\infty}\delta(t-t_0)e^{-i2\pi ft}dt =\\
&=e^{i2\pi ft_0}\int_{-\infty}^{\infty}\delta(t-t_0)dt=\\
&=e^{i2\pi ft_0}
\end{align*}
$$
### Armonica
Dato un segnale armonico $x(t)=e^{\large i2\pi f_0t}$, calcolare la sua trasformata di Fourier.
$$
\begin{align*}
X(f)&=\int_{-\infty}^{\infty}e^{i2\pi f_0t}e^{-i2\pi ft}dt =\\
&=\int_{-\infty}^{\infty}e^{i2\pi (f_0-f)t}dt=\\
&= \lim_{\Delta T\to\infty}\left.\frac{e^{i2\pi (f_0-f)t}}{i2\pi(f_0-f)}\right|_{-\Delta T/2}^{\Delta T/2}=\\
&=\lim_{\Delta T\to\infty}\frac{e^{ i\pi(f_0-f)\Delta T}-e^{-i\pi(f_0-f)\Delta T}}{i2\pi(f_0-f)} =\\
&= \lim_{\Delta T\to\infty}\frac{\sin\left(\pi(f_0-f)\Delta T\right)}{\pi(f_0-f)}= \\
&=\lim_{\Delta T\to\infty}\Delta T \operatorname{sinc}((f_0-f)\Delta T)=\\
&= \delta(f_0-f)=\delta(f-f_0)
\end{align*}
$$
### Coseno
Dato un segnale coseno $x(t)=\cos(2\pi f_0t)$, calcolare la sua trasformata di Fourier.
$$
\begin{align*}
X(f)&=\int_{-\infty}^{\infty}\cos(2\pi f_0t)e^{-i2\pi ft}dt =\\
&=\int_{-\infty}^{\infty}\frac{e^{i2\pi f_0t}+e^{-i2\pi f_0t}}{2}e^{-i2\pi ft}dt=\\
&=\int_{-\infty}^{\infty}\frac{e^{-i2\pi (f-f_0)t}}{2}dt+\int_{-\infty}^{\infty}\frac{e^{-i2\pi (f+f_0)t}}{2}dt =\\
&=\frac{1}{2}\delta(f-f_0)+\frac{1}{2}\delta(f+f_0)
\end{align*}
$$
### Seno
Dato un segnale seno $x(t)=\sin(2\pi f_0t)$, calcolare la sua trasformata di Fourier.
$$
\begin{align*}
X(f)&=\int_{-\infty}^{\infty}\sin(2\pi f_0t)e^{-i2\pi ft}dt =\\
&=\int_{-\infty}^{\infty}\frac{e^{i2\pi f_0t}-e^{-i2\pi f_0t}}{2i}e^{-i2\pi ft}dt=\\
&=\int_{-\infty}^{\infty}\frac{e^{-i2\pi (f-f_0)t}}{2i}dt-\int_{-\infty}^{\infty}\frac{e^{-i2\pi (f+f_0)t}}{2i}dt =\\
&=\frac{1}{2i}\delta(f-f_0)-\frac{1}{2i}\delta(f+f_0)=\\
&=-\frac{i}{2}\delta(f-f_0)+\frac{i}{2}\delta(f+f_0)
\end{align*}
$$

### Una [[Segnale di Gauss|gaussiana]]
Dato un segnale gaussiano $x(t) = e^{\large-\alpha t^2}$, calcolare la sua trasformata di Fourier.
$$
\begin{align*}
X(f)&=\int_{-\infty}^{\infty}e^{-\alpha t^2}e^{\large -i2\pi ft}dt =\\
&=\int_{-\infty}^{\infty}e^{-\alpha\left[t^2+\frac{i2\pi ft}{\alpha}+\left(\frac{i\pi f}{\alpha}\right)^2-\left(\frac{i\pi f}{\alpha}\right)^2\right]}dt =\\
&=e^{-\frac{\pi^2f^2}{\alpha^2}\alpha} \int_{-\infty}^{\infty}e^{-\alpha\left(t+\frac{i\pi f}{\alpha}\right)^2}dt=\\
t'\coloneqq\sqrt{a}\left(t+\frac{i\pi f}{\alpha}\right)\quad&=e^{-\frac{\pi^2f^2}{\alpha}} \underbrace{\int_{-\infty}^{\infty}e^{-t'^2}\frac{dt'}{\sqrt{a}}}_{\mathclap{\text{integrale di Gauss}}}=\\
&=e^{\large-\frac{\pi^2f^2}{\alpha}}\sqrt{\frac{\pi}{a}}
\end{align*}
$$
### [[Segnale Esponenziale|Esponenziale unilatero]]
Dato il segnale esponenziale $x(t)=e^{\large-\alpha t}u(t)$, calcolare la sua trasformata di Fourier.
$$
\begin{align*}
X(f)&=\int_{-\infty}^{\infty}u(t)e^{-\alpha t}e^{-i2\pi ft}dt =\\
&=\int_{0}^{\infty}e^{-\alpha t}e^{-i2\pi ft}dt =\\
&=\int_{0}^{\infty}e^{-(\alpha +i2\pi f)t}dt =\\
&=\left.\frac{e^{-(\alpha +i2\pi f)t}}{-\alpha -i2\pi f}\right|_{0}^{\infty}=\\
&=\frac{1}{\alpha+2\pi if}
\end{align*}
$$
### [[Segnale Esponenziale|Esponenziale unilatero]] ribaltato
Dato il segnale $x(t)=e^{3t}u(-t)$, calcolare la sua trasformata di Fourier.

Si noti come questo segnale sia un esponenziale unilatero ribaltato.
$$
x(t) = e^{3t}u(-t) \quad\triangle\quad x'(t)=e^{-3t}u(t)
$$
In questo caso si vuole calcolare l'energia usando il [[Trasformata di Fourier#Teorema di Parseval|Teorema di Parseval]]. 

Conoscendo la trasformata elementare per un segnale esponenziale unilatero $x_1(t)$, si può applicare la proprietà di [[Trasformata di Fourier#Scala|scala]] per ottenere la trasformata del segnale $x(t)$.
$$
X'(f)=\frac{1}{3+2\pi if}\quad\longrightarrow\quad X(f)=\frac{1}{3-2\pi if}
$$
### [[Segnale Esponenziale|Esponenziale unilatero]] traslato
Dato il segnale $x(t)=e^{-2t+4}u(t-2)$, calcolare la sua trasformata di Fourier.

Si noti come questo segnale sia un esponenziale unilatero traslato.
$$
\begin{align*}
x(t)&=e^{-2t+4}u(t-2)\\
x'(t)&=e^{-2t}u(t)
\end{align*}
\quad\longrightarrow\quad
x(t)=x'(t-2)
$$
Quindi, conoscendo la trasformata elementare dell'esponenziale unilatero, si può applicare la proprietà della [[Trasformata di Fourier#Traslazione nel tempo|traslazione nel tempo]] per ottenere la trasformata del segnale $x(t)$.
$$
X'(f)=\frac{1}{2+2\pi if}\quad\longrightarrow\quad X(f)=\frac{1}{2+2\pi if}\cdot e^{-i4\pi f}
$$
### Esponenziale simmetrico
Dato il segnale $x(t) = e^{\large -\alpha|t|}$, calcolare la sua trasformata di Fourier.
$$
X(f)=\int_{-\infty}^{\infty}e^{-\alpha |t|}e^{-i2\pi ft}dt
$$
Il segnale può essere diviso in due esponenziali unilateri. $X_1$ è la loro trasformata calcolata nell'esempio sopra.
$$
\begin{align*}
X(f)&= e^{-\alpha t}u(t)+e^{\alpha t}u(-t) =\\
&= X_1(f)+X_1(-f)=\\
&= \frac{1}{\alpha+2\pi if}+ \frac{1}{\alpha-2\pi if} =\\
&=\underbrace{\frac{2\alpha}{\alpha^2+4\pi^2f^2}}_{\mathclap{\text{distribuzione Lorentziana}}}
\end{align*}
$$
### [[Segnale Gradino]]
Dato il segnale $x(t)=u(t)$, calcolare la sua trasformata di Fourier.
$$
X(f)=\int_{-\infty}^{\infty}u(t )e^{-i2\pi ft}dt
$$

Questo segnale si può riscrivere come il limite di un esponenziale equilatero, di cui la trasformata è già nota. 
$$
\begin{align*}
x(t)&=\lim_{\alpha\to 0}e^{-\alpha t}u(t)\\
X(f)&=\lim_{\alpha\to 0}\frac{1}{\alpha+2\pi if} =\frac{1}{2\pi if}
\end{align*}
$$
>[!fail] Procedimento fallace
>Se $f=0$, e di conseguenza $X(f)\to\infty$, allora dovrebbe apparire una $\delta(f)$. 

A questo punto si compie una razionalizzazione.
$$
X(f)=\lim_{\alpha\to 0}\frac{1}{\alpha+2\pi if} = \lim_{\alpha\to 0}\frac{\alpha}{\alpha^2+4\pi^2 f^2}-\frac{2\pi if}{\alpha^2+4\pi^2 f^2}
$$

Per tener conto del caso $f=0$, si aggiunge il termine $\delta(f)\cdot A$. 
$$
X(f)= \lim_{\alpha\to 0}\frac{\alpha}{\alpha^2+4\pi^2 f^2}-\frac{2\pi if}{\alpha^2+4\pi^2 f^2}+A\cdot\delta(f)
$$

Per ottenere l'area $A$ della delta, si calcola l'integrale del valor medio. Il termine immaginario non dà contributo all'integrale e può essere omesso.
$$
\begin{align*}
A&=\int_{-\infty}^{\infty}\frac{\alpha}{\alpha^2+4\pi^2f^2}\ df=\\
&=\int_{-\infty}^{\infty}\frac{1}{\alpha\left(1+\frac{4\pi^2f^2}{\alpha^2}\right)}\ df=\\
f'\coloneqq\frac{2\pi f}{\alpha}\quad&=\frac{1}{\alpha}\int_{-\infty}^{\infty}\frac{1}{1+f'^2}\frac{\alpha}{2\pi}\ df=\\
&=\frac{1}{2\pi}\Big|\arctan(f)\Big|_{-\infty}^{+\infty}=\\
&=\frac{1}{2\pi}\left[\frac{\pi}{2}+\frac{\pi}{2}\right]=\\
&=\frac{1}{2}
\end{align*}
$$

Adesso si può scrivere la corretta trasformata del gradino.
$$
X(f)=\frac{1}{2}\delta(f)+\frac{1}{2\pi if}
$$
### [[Funzione Segno]]
Data la funzione segno $x(t)=\operatorname{sgn}(r)$, calcolare la sua trasformata di Fourier.
$$
X(f)=\int_{-\infty}^{\infty}\operatorname{sgn}(t)e^{-i2\pi ft}dt
$$

La funzione segno può essere riscritta come somma di due gradini.
$$
x(t)=\operatorname{sgn}(t)=u(t)-u(-t)
$$

Conoscendo la trasformata di un gradino, si può calcolare questa trasformata senza svolgere integrali.
$$
\begin{align*}
X(f)&=\frac{1}{2}\delta(f)+\frac{1}{2\pi if}-\frac{1}{2}\delta(f)+\frac{1}{2\pi if}=\\
&=\frac{1}{i\pi f}
\end{align*}
$$
### Derivata di una [[Segnale Finestra#Finestra rettangolare|finestra rettangolare]]
Dato il segnale $x(t)=\operatorname{rect}\left(\dfrac{t}{T}\right)$, di cui si conosce la corrispondente trasformata di Fourier $X(f)=T\operatorname{sinc}(Tf)$, calcolare la trasformata della sua derivata.

>[!info] Calcolo analitico della derivata
>Se si volesse calcolare a mano la derivata di una funzione discontinua bisogna tener conto dei punti di discontinuità con degli impulsi $\delta(t)$.
>$$
>\dot{x}(t)=\delta(t+T/2)-\delta(t-T/2)
>$$

Sfruttando la proprietà della derivazione, non è necessario calcolare la derivata del segnale nel dominio del tempo.
$$
\begin{align*}
X'(f)&=i2\pi f \cdot T\operatorname{sinc}(fT)=\\
&=i2\pi fT\cdot\frac{\sin(\pi fT)}{\pi fT}=\\
&=2i\cdot\sin(\pi fT)=\\
&=2i\cdot\frac{e^{i\pi fT}-e^{-i\pi fT}}{2i}=\\
&=e^{i\pi fT}-e^{-i\pi fT}
\end{align*}
$$

La trasformata ottenuta è uguale alla trasformata due [[Delta di Dirac]] traslate quindi è corretto.
### Derivata di una [[Segnale Finestra#Finestra triangolare|finestra triangolare]]
Dato il segnale $x(t)=\operatorname{tri}\left(\dfrac{t}{T}\right)$, calcolare la trasformata della sua derivata.
>[!info] Calcolo analitico della derivata
>$$
>\dot{x}(t)=\begin{cases}
>\hphantom{-}1/T&\text{ se }-T\le t\le0\\
>-1/T&\text{ se }\hphantom{-T}0\le t\le T\\
>\hphantom{\dfrac{1}{T}}{0}&\text{ altrove}
>\end{cases}
>$$
>Questa derivata può essere riscritta come somma di due finestre rettangolari.
>$$
>\dot{x}(t)=\frac{1}{T}\operatorname{rect}\left(\frac{t+\frac{T}{2}}{T}\right)-\frac{1}{T}\operatorname{rect}\left(\frac{t-\frac{T}{2}}{T}\right)
>$$

Per calcolare questa trasformata si applicano le proprietà di [[#Traslazione nel tempo]] e [[#Scala]].
$$
\begin{align*}
X'(f)&=\frac{1}{T}T\operatorname{sinc}(fT)e^{i2\pi f\frac{T}{2}}-\frac{1}{T}T\operatorname{sinc}(fT)e^{-i2\pi f\frac{T}{2}}=\\
&=\operatorname{sinc}(fT)\left(e^{i2\pi f\frac{T}{2}}-e^{-i2\pi f\frac{T}{2}}\right)=\\
&=\operatorname{sinc}(fT)\sin(\pi fT)\mathrel{2i}{=}\\
&=\frac{2i\sin^2(\pi fT)}{\pi fT}
\end{align*}
$$
Da qui si può anche calcolare la trasformata della funzione originale $x(t)=\operatorname{tri}\left(\dfrac{t}{T}\right)$.
$$
\begin{align*}
X(f)&=\frac{X'(f)}{2\pi if}=\\
&=\frac{2i\sin^2(\pi fT)}{\pi fT}\frac{1}{2\pi if}=\\
&=\frac{\sin^2(\pi fT)}{\pi^2 f^2 T}\frac{T}{T}=\\
&=T\operatorname{sinc}^2(fT)
\end{align*}
$$

## Esercizi vari
### Esercizio 1
Dato il segnale $x(t)=e^{-t/2}\cos(100\pi t)u(t)$, calcolare la sua trasformata di Fourier.

Il segnale può essere scomposto in due segnali ed elaborato separatamente.
$$
x'(t)=e^{-t/2}u(t) \quad\longrightarrow\quad x(t)=x'(t)\cos(50\cdot 2\pi t)
$$
A questo punto si può calcolare facilmente la trasformata del coseno e dell'esponenziale unilatero.
$$
x'(t)=e^{-t/2}u(t)\quad\overset{\mathcal{F}}{\longrightarrow}\quad X'(f)=\frac{1}{\frac{1}{2}+2\pi if}
$$
Applicando la proprietà della [[#Convoluzione]], si può ricavare la trasformata del segnale composto.
$$
\begin{align*}
X(f)&=X'(f)*\left[\frac{1}{2}\delta(f-50)+\frac{1}{2}\delta(f+50)\right]=\\
&=\left[\frac{1}{\frac{1}{2}+2\pi if}*\frac{1}{2}\delta(f-50)\right] + \left[\frac{1}{\frac{1}{2}+2\pi if}*\frac{1}{2}\delta(f+50)\right]=\\
&=\frac{1}{1+4\pi i(f-50)}+\frac{1}{1+4\pi i(f+50)}
\end{align*}
$$
### Esercizio 2
Dato il segnale $x(t)$, calcolare la sua trasformata di Fourier.
$$
x(t)=-\operatorname{rect}\left[4\left(t+\frac{3}{8}\right)\right] +\operatorname{rect}\left(2t\right) -\operatorname{rect}\left[4\left(t-\frac{3}{8}\right)\right]
$$
>[!tip] Forma alternativa
>Questo segnale si può riscrivere in modo più semplice per facilitare i calcoli.
>$$
>x(t)=2\operatorname{rect}\left(2t\right)-\operatorname{rect}\left(t\right)
>$$
>A beneficio dell´esercizio, si terrà conto della prima forma.

Si procede calcolando la trasformata delle varie componenti:
- il rettangolo centrato nell'origine
$$
\operatorname{rect}\left(2t\right)\overset{\mathcal{F}}{\longrightarrow}\frac{1}{2}\operatorname{sinc}\left(\frac{f}{2}\right)
$$
- il rettangolo anticipato
$$
\operatorname{rect}\left[4\left(t+\frac{3}{8}\right)\right]\overset{\mathcal{F}}{\longrightarrow}\frac{1}{4}\operatorname{sinc}\left(\frac{f}{4}\right)e^{i\pi f\frac{3}{4}}
$$
- il rettangolo ritardato
$$
\operatorname{rect}\left[4\left(t-\frac{3}{8}\right)\right]\overset{\mathcal{F}}{\longrightarrow}\frac{1}{4}\operatorname{sinc}\left(\frac{f}{4}\right)e^{-i\pi f\frac{3}{4}}
$$
Quindi si possono ricomporre insieme le varie trasformate ed ottenere la trasformata del segnale completo.
$$
\begin{align*}
X(f)&=\frac{1}{2}\operatorname{sinc}\left(\frac{f}{2}\right)-\frac{1}{4}\operatorname{sinc}\left(\frac{f}{4}\right)\left(e^{i\pi f\frac{3}{4}}+e^{-i\pi f\frac{3}{4}}\right)=\\
&=\frac{1}{2}\operatorname{sinc}\left(\frac{f}{2}\right)-\frac{1}{2}\operatorname{sinc}\left(\frac{f}{4}\right)\cos\left(\frac{3}{4}\pi f\right)=\\
&=\frac{1}{2}\left[\operatorname{sinc}\left(\frac{f}{2}\right)-\operatorname{sinc}\left(\frac{f}{4}\right)\cos\left(\frac{3}{4}\pi f\right)\right]
\end{align*}
$$
### Esercizio 3
Semplificare il seguente segnale.
$$
x(t)=\cos(2\pi f_0t)\cdot\left[\sin(2\pi f_0t)*\frac{\sin(3\pi f_0t)}{\pi t}\right]
$$
Facendo una manipolazione algebrica si può riscrivere l'ultimo seno come un [[Seno Cardinale]].
$$
x(t)=\cos(2\pi f_0t)\cdot\underbrace{\left[\sin(2\pi f_0t)*3f_0\operatorname{sinc}(3f_0t)\right]}_{z(t)}
$$
I due termini del prodotto si possono trattare separatamente per semplificare i calcoli.
$$
x(t)=\cos(2\pi f_0t)\cdot z(t)
$$
Fare la [[Convoluzione]] tra un seno e un seno cardinale è complicato quindi si passa al dominio della frequenza.
$$
\begin{align*}
Z(f)&=\frac{1}{2i}[\delta(f-f_0)-\delta(f+f_0)]\cdot \frac{\cancel{3f_0}}{\cancel{3f_0}}\operatorname{rect}\left(\frac{f}{3f_0}\right)=\\
&=\frac{1}{2i}\operatorname{rect}\left(\frac{f_0}{3f_0}\right)\delta(f-f_0)-\frac{1}{2i}\operatorname{rect}\left(\frac{-f_0}{3f_0}\right)\delta(f+f_0)=\\
&=\frac{1}{2i}\operatorname{rect}\left(\frac{1}{3}\right)\delta(f-f_0)-\frac{1}{2i}\operatorname{rect}\left(\frac{-1}{3}\right)\delta(f+f_0)
\end{align*}
$$
$1/3$ e $-1/3$ sono compresi tra $1/2$ e $-1/2$, quindi le finestre valgono $1$.
$$
Z(f)=\frac{1}{2i}\delta(f-f_0)-\frac{1}{2i}\delta(f+f_0)
$$
Anti-trasformando $Z(f)$ si ottiene il risultato della convoluzione.
$$
z(t)=\sin(2\pi f_0t)
$$
Il segnale semplificato vale $x(t)=\cos(2\pi f_0t)\sin(2\pi f_0t)=\dfrac{1}{2}\sin(4\pi f_0t)$.
### Esercizio 4
Dato il seguente segnale, calcolare la sua trasformata di Fourier.
$$
x(t)=\cos\left[\frac{\pi(t-T)}{2T}\right]\cdot\cos\left(\frac{2\pi t}{T}\right)=\cos\left(\frac{2\pi t}{4T}-\frac{\pi}{2}\right)\cdot\cos\left(\frac{2\pi t}{T}\right)
$$
Il primo coseno può essere riscritto come seno in quanto $\cos(\alpha-\pi/2)=\sin(\alpha)$.
$$
x(t)=\sin\left(\frac{2\pi t}{4T}\right)\cdot\cos\left(\frac{2\pi t}{T}\right)
$$
Applicando le [[Formule Trigonometriche#Formule di Werner|formule di Werner]], il segnale si può riscrivere come somma di seni. 
$$
\begin{align*}
x(t)&=\frac{1}{2}\sin\left(\frac{2\pi t}{4T}+\frac{2\pi t}{T}\right)+\frac{1}{2}\sin\left(\frac{2\pi t}{4T}-\frac{2\pi t}{T}\right)=\\
&=\frac{1}{2}\sin\left(\frac{5\pi t}{2T}\right)-\frac{1}{2}\sin\left(\frac{3 \pi t}{2T}\right)
\end{align*}
$$
Ora si può ricavare la trasformata usando la trasformata elementare del seno.
$$
\begin{align*}
X(f)&=\frac{1}{2}\left[\frac{i}{2}\delta\left(f+\frac{5}{4T}\right)-\frac{i}{2}\delta\left(f-\frac{5}{4T}\right)\right]-\\
&-\frac{1}{2}\left[\frac{i}{2}\delta\left(f+\frac{3}{4T}\right)-\frac{i}{2}\delta\left(f-\frac{3}{4T}\right)\right]=\\
&=\frac{i}{4}\delta\left(f+\frac{5}{4T}\right)-\frac{i}{4}\delta\left(f-\frac{5}{4T}\right)-\frac{i}{4}\delta\left(f+\frac{3}{4T}\right)+\frac{i}{4}\delta\left(f-\frac{3}{4T}\right)
\end{align*}
$$