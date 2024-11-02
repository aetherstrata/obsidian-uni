---
tags:
  - trasformata
  - analisi
  - segnale
---
La trasformata di Fourier è una generalizzazione della [[Serie di Fourier]], capace di operare anche su [[Filtro#Risposta Impulsiva|segnali impulsivi]], che ha uno spazio delle basi infinito **non** numerabile, al contrario della serie.
$$
x(t)=\int_{-\infty}^{\infty}X(f)\cdot e^{\large i2\pi ft}df \quad\longrightarrow\quad
 \begin{cases}
 \mathrel{}X(f) & \text{trasformata di Fourier}\\
 e^{\large i2\pi ft} & \text{nucleo di trasformazione}
 \end{cases}
$$
L'insieme dei coefficienti definisce la trasformata di Fourier del segnale.
$$
X(f)=\int_{-\infty}^{\infty}x(t)\cdot e^{\large -i2\pi ft}dt
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
\left<e^{\large i2\pi f_1t},e^{\large i2\pi f_2t}\right> &=\lim_{\Delta T\to\infty}\int_{-\Delta T/2}^{\Delta T/2}e^{\large i2\pi (f_1-f_2)t}dt =\\
&= \lim_{\Delta T\to\infty} \left.\frac{e^{\large i2\pi(f_1-f_2)t}}{i2\pi(f_1-f_2)}\right|_{-\Delta T/2}^{\Delta T/2} =\\
&= \lim_{\Delta T\to\infty}\Delta T \frac{\sin(\pi(f_1-f_2)\Delta T)}{\pi(f_1-f_2)\Delta T} =\\
&=\lim_{\Delta T\to\infty}\Delta T \operatorname{sinc}(\Delta T(f_1-f_2))=\\
&= \delta(f_1-f_2)
\end{align*}
$$
Come per la serie di Fourier, è dimostrato che le armoniche sono ortogonali tra loro.
### Trasformata
Facendo il prodotto scalare della funzione per un suo versore, si può estrarre il suo coefficiente per l'armonica scelta.
$$
X(f)=\left<x(t),e^{\large i2\pi ft}\right> =\int_{-\infty}^{\infty}x(t)e^{\large -i2\pi ft}dt
$$
## Proprietà
### Valore atteso
Il valore in $0$ della trasformata è il valor medio del segnale.
$$
X(0)=\int_{-\infty}^{\infty}x(t)e^{\large -i2\pi ft}dt=\int_{-\infty}^{\infty}x(t)dt = \mathbb{E}(x)
$$
### Linearità
Ad una combinazione lineare di segnali nel tempo corrisponde una combinazione lineare di spettri di frequenze.
$$
\begin{gather*}
\begin{aligned}
x_1(t)&\overset{\mathcal{F}}{\longrightarrow}X_1(f)\\
x_2(t)&\overset{\mathcal{F}}{\longrightarrow}X_2(f)\\\\ \hline
\end{aligned}\\\\\begin{aligned}
ax_1(t)+bx_2(t)\overset{\mathcal{F}}{\longrightarrow}&\int_{-\infty}^{\infty}\left[ax_1(t)+bx_2(t)\right]e^{\large -i2\pi ft}dt=\\
={}&a\int_{-\infty}^{\infty}x_1(t)e^{\large -i2\pi ft}dt+b\int_{-\infty}^{\infty}x_2(t)e^{\large -i2\pi ft}dt =\\
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
\int_{-\infty}^{\infty}X(t)e^{\large -i2\pi ft}dt = \int_{-\infty}^{\infty} X(f')e^{\large -i2\pi t'f'}df' = x(-t')
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
x(at)\overset{\mathcal{F}}{\longrightarrow}{}&\int_{-\infty}^{\infty}x(at)e^{\large -i2\pi ft}dt =\\
{}\xlongequal{at=t'}{}&\int_{-\infty}^{\infty}x(t')e^{\large -i2\pi f\Large\frac{t'}{a}}\frac{dt'}{|a|}=\\
={}&\frac{1}{|a|}\int_{-\infty}^{\infty}x(t')e^{\large -i2\pi \Large\frac{f}{a}t'}dt'=\\
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
x(t-t_0)\overset{\mathcal{F}}{\longrightarrow}{}&\int_{-\infty}^{\infty}x(t-t_0)e^{\large -i2\pi ft}dt =\\
t'\coloneqq t-t_o\quad={}&\int_{-\infty}^{\infty}x(t')e^{\large -i2\pi f(t'+t_0)}dt' =\\
={}&e^{\large -i2\pi ft_0}\int_{-\infty}^{\infty}x(t')e^{\large -i2\pi ft'}dt'=\\
={}&e^{\large -i2\pi ft_0}X(f)
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
\dot{X}(f)=\frac{d}{df}X(f)&=\int_{-\infty}^{\infty}-i2\pi t\cdot x(t)e^{\large -i2\pi ft}dt\\
\boxed{i2\pi t\cdot x(t)}&\overset{\mathcal{F}}{\longrightarrow}-\frac{d}{df}X(f)
\end{align*}
$$
### [[Convoluzione]]
$$
z(t)=x(t)*y(t)\overset{\mathcal{F}}{\longrightarrow}X(f)Y(f)=Z(f)
$$
#### Dimostrazione
$$
\begin{align*}
z(t)=x(t)*y(t)&=\int_{-\infty}^{\infty}x(t-\tau)\underbrace{y(\tau)}_{\mathcal{F^{-1}}}d\tau=\\
&=\int_{-\infty}^{\infty}x(t-\tau)\int_{-\infty}^{\infty}Y(f)e^{i2\pi f\tau}df\ d\tau=\\
&=\int_{-\infty}^{\infty}Y(f)df\int_{-\infty}^{\infty}x(t-\tau)e^{i2\pi f\tau} d\tau=\\
t'\coloneqq t-\tau\quad&=\int_{-\infty}^{\infty}Y(f)df\int_{-\infty}^{\infty}x(t')e^ {\large i2\pi f(t-t')} dt'=\\
&=\int_{-\infty}^{\infty}Y(f)e^ {\large i2\pi ft}df\underbrace{\int_{-\infty}^{\infty}x(t')e^ {\large -i2\pi ft'} dt'}_{\large X(f)}=\\
&=\int_{-\infty}^{\infty}\boxed{Y(f)X(f)}e^ {\large i2\pi ft}df
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
x^*(t)&\overset{\mathcal{F}}{\longrightarrow}\int_{-\infty}^{\infty}x^*(t)e^{\large -i2\pi ft}dt=\left(\int_{-\infty}^{\infty}x(t)e^{\large i2\pi ft}dt\right)^*=X^*(-f)\\
\end{align*}
$$
## Teorema di Parseval

Il Teorema di Parseval permette di calcolare velocemente la potenza di un segnale rappresentato come trasformata di Fourier.
$$
z(t)=x(t)*y^*(-t)=\int_{-\infty}^{\infty}x(\tau)y^*(t+\tau)d\tau
$$
Sfruttando la proprietà della [[#Convoluzione]], si può riscrivere come prodotto tra trasformate.
$$
z(t)=\int_{-\infty}^{\infty}X(f)Y^*(f)e^{\large i2\pi ft}df
$$
Valutando l'espressione in $t=0$, è evidente l'uguaglianza con l'operazione di prodotto scalare.
$$
z(0)=\underbrace{\int_{-\infty}^{\infty}x(\tau)y^*(\tau)d\tau}_{\large\left<x(t),y(t)\right>}=\underbrace{\int_{-\infty}^{\infty}X(f)Y^*(f)df}_{{\large\left<X(f),Y(f)\right>}}
$$
Se si pone $x(t) = y(t)$, quindi si fa la sua [[Correlazione#Autocorrelazione|autocorrelazione]], il risultato sarà l'[[Energia e potenza#Energia di un segnale analogico|energia]] del segnale.
$$
E_x=\int_{-\infty}^{\infty}\left|x(t)\right|^2dt=\int_{-\infty}^{\infty}x(\tau)x^*(\tau)d\tau=\int_{-\infty}^{\infty}X(f)X^*(f)df=\int_{-\infty}^{\infty}\left|X(f)\right|^2df
$$
## Esempi
### [[Segnale Finestra#Finestra rettangolare|Finestra rettangolare]]
Dato un impulso rettangolare $x(t)=\operatorname{rect}\left(\dfrac{t}{\tau}\right)$, calcolare la sua trasformata di Fourier.
$$
\begin{align*}
X(f)&=\int_{-\infty}^{\infty}\operatorname{rect}\left(\dfrac{t}{\tau}\right) e^{\large -i2\pi ft}dt =
 \int_{-\tau/2}^{\tau/2}e^{\large -i2\pi ft}dt =\\
&= \left.\frac{e^{\large -i2\pi ft}}{-i2\pi f}\right|_{-\tau/2}^{\tau/2}=\frac{e^{\large -i2\pi f\frac{\Large\tau}{2}}-e^{\large i2\pi f\frac{\Large\tau
}{2}}}{ -i2\pi f}=\\
&=\frac{\sin\left(\pi f\tau\right)}{\pi f} =\tau\operatorname{sinc}(f\tau)
\end{align*}
$$
### [[Delta di Dirac]]
Dato un impulso matematico $x(t)=\delta(t)$, calcolare la sua trasformata di Fourier.
$$
\begin{align*}
X(f)&=\int_{-\infty}^{\infty}\delta(t)e^{\large -i2\pi ft}dt =\\
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
X(f)&=\int_{-\infty}^{\infty}\delta(t-t_0)e^{\large -i2\pi ft}dt =\\
&=e^{\large i2\pi ft_0}\int_{-\infty}^{\infty}\delta(t-t_0)dt=\\
&=e^{\large i2\pi ft_0}
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
X(f)&=\int_{-\infty}^{\infty}\cos(2\pi f_0t)e^{\large -i2\pi ft}dt =\\
&=\int_{-\infty}^{\infty}\frac{e^{\large i2\pi f_0t}+e^{\large -i2\pi f_0t}}{2}e^{\large -i2\pi ft}dt=\\
&=\int_{-\infty}^{\infty}\frac{e^{\large -i2\pi (f-f_0)t}}{2}dt+\int_{-\infty}^{\infty}\frac{e^{\large -i2\pi (f+f_0)t}}{2}dt =\\
&=\frac{1}{2}\delta(f-f_0)+\frac{1}{2}\delta(f+f_0)
\end{align*}
$$
### Seno
Dato un segnale seno $x(t)=\sin(2\pi f_0t)$, calcolare la sua trasformata di Fourier.
$$
\begin{align*}
X(f)&=\int_{-\infty}^{\infty}\sin(2\pi f_0t)e^{\large -i2\pi ft}dt =\\
&=\int_{-\infty}^{\infty}\frac{e^{\large i2\pi f_0t}-e^{\large -i2\pi f_0t}}{2i}e^{\large -i2\pi ft}dt=\\
&=\int_{-\infty}^{\infty}\frac{e^{\large -i2\pi (f-f_0)t}}{2i}dt-\int_{-\infty}^{\infty}\frac{e^{\large -i2\pi (f+f_0)t}}{2i}dt =\\
&=\frac{1}{2i}\delta(f-f_0)-\frac{1}{2i}\delta(f+f_0)=\\
&=-\frac{i}{2}\delta(f-f_0)+\frac{i}{2}\delta(f+f_0)
\end{align*}
$$

### Una [[Segnale di Gauss|gaussiana]]
Dato un segnale gaussiano $x(t) = e^{\large-\alpha t^2}$, calcolare la sua trasformata di Fourier.
$$
\begin{align*}
X(f)&=\int_{-\infty}^{\infty}e^{\large-\alpha t^2}e^{\large -i2\pi ft}dt =\\
&=\int_{-\infty}^{\infty}e^{\large-\alpha\left[t^2+\frac{i2\pi ft}{\alpha}+\left(\frac{i\pi f}{\alpha}\right)^2-\left(\frac{i\pi f}{\alpha}\right)^2\right]}dt =\\
&=e^{\large-\frac{\pi^2f^2}{\alpha^2}\alpha} \int_{-\infty}^{\infty}e^{\large-\alpha\left(t+\frac{i\pi f}{\alpha}\right)^2}dt=\\
t'\coloneqq\sqrt{a}\left(t+\frac{i\pi f}{\alpha}\right)\quad&=e^{\large-\frac{\pi^2f^2}{\alpha}} \underbrace{\int_{-\infty}^{\infty}e^{\large -t'^2}\frac{dt'}{\sqrt{a}}}_{\mathclap{\text{integrale di Gauss}}}=\\
&=e^{\large-\frac{\pi^2f^2}{\alpha}}\sqrt{\frac{\pi}{a}}
\end{align*}
$$
### [[Segnale Esponenziale|Esponenziale unilatero]]
Dato il segnale esponenziale $x(t)=e^{\large-\alpha t}u(t)$, calcolare la sua trasformata di Fourier.
$$
\begin{align*}
X(f)&=\int_{-\infty}^{\infty}u(t)e^{\large-\alpha t}e^{\large -i2\pi ft}dt =\\
&=\int_{0}^{\infty}e^{\large-\alpha t}e^{\large -i2\pi ft}dt =\\
&=\int_{0}^{\infty}e^{\large -(\alpha +i2\pi f)t}dt =\\
&=\left.\frac{e^{\large -(\alpha +i2\pi f)t}}{-\alpha -i2\pi f}\right|_{0}^{\infty}=\\
&= \frac{1}{\alpha+2\pi if}
\end{align*}
$$
### Esponenziale simmetrico
Dato il segnale $x(t) = e^{\large -\alpha|t|}$, calcolare la sua trasformata di Fourier.
$$
X(f)=\int_{-\infty}^{\infty}e^{\large-\alpha |t|}e^{\large -i2\pi ft}dt
$$
Il segnale può essere diviso in due esponenziali unilateri. $X_1$ è la loro trasformata calcolata nell'esempio sopra.
$$
\begin{align*}
X(f)&= e^{\large -\alpha t}u(t)+e^{\large \alpha t}u(-t) =\\
&= X_1(f)+X_1(-f)=\\
&= \frac{1}{\alpha+2\pi if}+ \frac{1}{\alpha-2\pi if} =\\
&=\underbrace{\frac{2\alpha}{\alpha^2+4\pi^2f^2}}_{\mathclap{\text{distribuzione Lorentziana}}}
\end{align*}
$$
### [[Segnale Gradino]]
Dato il segnale $x(t)=u(t)$, calcolare la sua trasformata di Fourier.
$$
X(f)=\int_{-\infty}^{\infty}u(t )e^{\large -i2\pi ft}dt
$$

Questo segnale si può riscrivere come il limite di un esponenziale equilatero, di cui la trasformata è già nota. 
$$
\begin{align*}
x(t)&=\lim_{\alpha\to 0}e^{\large-\alpha t}u(t)\\
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
X(f)=\int_{-\infty}^{\infty}\operatorname{sgn}(t)e^{\large -i2\pi ft}dt
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
&=2i\cdot\frac{e^{\large i\pi fT}-e^{\large -i\pi fT}}{2i}=\\
&=e^{\large i\pi fT}-e^{\large -i\pi fT}
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
X'(f)&=\frac{1}{T}T\operatorname{sinc}(fT)e^{\large i2\pi f\frac{T}{2}}-\frac{1}{T}T\operatorname{sinc}(fT)e^{\large -i2\pi f\frac{T}{2}}=\\
&=\operatorname{sinc}(fT)\left(e^{\large i2\pi f\frac{T}{2}}-e^{\large -i2\pi f\frac{T}{2}}\right)=\\
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
