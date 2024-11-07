---
tags:
  - segnale
  - sistema
---
Un filtro è un [[Sistema|sistema]] [[Linearità|lineare]] [[Invarianza temporale|tempo-invariante]] ed è univocamente caratterizzato da una [[#Risposta Impulsiva|risposta impulsiva]] $h(t)$.
$$
x(t) \to\boxed{\quad h(t)\quad}\to y(t)
$$
La sua uscita si può sempre ricavare dalla [[Convoluzione|convoluzione]] tra il suo ingresso e la risposta impulsiva.
$$
y(t) = x(t)*h(t)
$$
## Risposta impulsiva
 La risposta impulsiva è la funzione caratterizzante di un filtro e descrive il suo comportamento in risposta ad un [[Delta di Dirac|impulso]]. 
$$
\delta(t) \to\boxed{\text{ sistema }}\to h(t)
$$
## Funzione di trasferimento
La funzione di trasferimento è la [[Trasformata di Fourier]] della risposta impulsiva.
$$
h(t)\overset{\mathcal{F}}{\longrightarrow}H(f)
$$
## Categorie
### Filtri passa-basso
I filtri, o segnali, passa-basso si usano per trasmettere segnali la cui [[Trasformata di Fourier]] è limitata attorno l'origine. La banda del segnale è legata alla frequenza massima del segnale.
#### Esempio
Il segnale vocale è un segnale passa-basso poiché tutte le informazioni sono contenute tra pochi Hz e 20 kHz. Il segnale delle chiamate telefoniche è tagliato a circa 4 kHz.
### Filtri passa-banda
I filtri passa-banda, o segnali modulati, sono formati da due componenti, uno centrato in $f_0$ e l'altro in $-f_0$. Questa frequenza si chiama **portante**.
#### Esempio
Questo tipo di segnali si usa per le trasmissioni wireless, come WiFi e Bluetooth.
## Esempi
### Esercizio 1
Data la riposta impulsiva $h(t) = 2\delta(t-2) + u(t-2)$, calcolare l'uscita del filtro quando in ingresso è applicato un gradino $u(t)$.
$$
y(t) = u(t)*[2\delta(t-2) + u(t-2)] = [u(t)*2\delta(t-2)] + [u(t)*u(t-2)]
$$

Applicando la proprietà distributiva, si ottengono due convoluzioni elementari da risolvere:

- Per la convoluzione con la [[Delta di Dirac]] basta traslare la funzione.
$$
u(t)*2\delta(t-2) = 2u(t-2)
$$

- Per la convoluzione con il [[Segnale Gradino|gradino]] il risultato sarà una [[Segnale Rampa|rampa]].
$$
\begin{align*}
u(t)*u(t-2) &= \int_{-\infty}^{\infty}u(\tau)u(t-\tau-2)\ d\tau = \int_{0}^{\infty}u(t-\tau-2)\ d\tau=\\
&= \int_{0}^{t-2}\ d\tau = \tau\Big|_{0}^{t-2}=(t-2)u(t-2)
\end{align*}
$$

L'uscita seguirà quindi la funzione $y(t) = 2u(t-2) + (t-2)u(t-2) = t\cdot u(t-2)$.
### Esercizio 2
Data la risposta impulsiva $h(t)=\dfrac{1}{T}\operatorname{sinc}\left(\dfrac{t}{T}\right)$, calcolare l'uscita del filtro quando in ingresso è applicato un coseno $x(t)=\cos(2\pi f_0t)$.
$$
y(t)=\cos(2\pi f_0t)*\dfrac{1}{T}\operatorname{sinc}\left(\dfrac{t}{T}\right)
$$

Questa convoluzione è difficile da calcolare quindi si ricavano gli spettri dei segnali.
$$
\begin{align*}
H(f)&=\frac{1}{T}T\operatorname{rect}(Tf)=\operatorname{rect}(Tf)\\
X(f)&=\frac{1}{2}\delta\left(f-f_0\right)+\frac{1}{2}\delta\left(f+f_0\right)
\end{align*}
$$

Applicando la proprietà della [[Trasformata di Fourier#Convoluzione|convoluzione]] si può ricavare il segnale di uscita.
$$
Y(f)=X(f)H(f)=\frac{1}{2}\operatorname{rect}(Tf)\delta(f-f_0)+\frac{1}{2}\operatorname{rect}(Tf)\delta(f+f_0)
$$
#### Considerazioni
La funzione di trasferimento è una [[Segnale Finestra#Finestra rettangolare|finestra rettangolare]], quindi questo filtro è un filtro passa-basso. Il segnale in uscita sarà uguale a quello in ingresso se $\large f_0< {1}/{2T}$, altrimenti sarà annullato.
### Esercizio 3
Data la risposta impulsiva $h(t)=\delta(t-2T)+\delta(t)-\delta(t-T)$, calcolare la funzione di trasferimento del filtro e la sua risposta al [[Segnale Gradino]].

Si può ottenere la funzione di trasferimento calcolando le trasformate di Fourier delle delta.
$$
H(f)=e^{-i2\pi f2T}+1-e^{-i2\pi fT}
$$
Si può ottenere la  calcolando le trasformate di Fourier delle delta.
$$
\begin{align*}
y(t)=h(t)*u(t)&=[\delta(t-2T)+\delta(t)-\delta(t-T)]*u(t)=\\
&=u(t-2T)+u(t)-u(t-T)
\end{align*}
$$
### Esercizio 4
Data la risposta impulsiva $h[n]=\dfrac{1}{4}\delta[n+1]+\dfrac{1}{2}\delta[n]+\dfrac{1}{2}\delta[n-1]$ e il segnale in ingresso $x[n]= cos(2\pi\phi n)$, calcolare l'uscita al variare del parametro $\phi$.

>[!info] Grafico di $h[n]$
>![[filtro-es-1.png]]

#### Caso $\phi=0$
Se $\phi=0$ allora $x[n]=1$ per tutto il dominio, visto che $\cos(0)=1$. Per ottenere l'uscita si calcola la convoluzione tra $1$ e la risposta impulsiva.
$$
y[n]=\sum_{k=-\infty}^{+\infty}\underbrace{x[k]}_{1}h[n-k]=\frac{1}{4}\delta[n+1]+\frac{1}{2}\delta[n]+\frac{1}{2}\delta[n-1]=1
$$
#### Caso $\phi=1/2$
Se $\phi=1/2$ allora $x[n]=\cos\left(\pi n\right)$.

>[!info] Grafico di $\cos\left(\pi n\right)$
>![[filtro-es-2.png]]

Il segnale $x[t]$ è una [[Funzione Periodica]] di periodo $2$.

L'uscita del filtro si può calcolare attraverso la convoluzione con la risposta impulsiva.
$$
y[n]=0
$$
#### Caso $\phi=1/4$
Se $\phi=1/4$ allora $x[n]=\cos\left(\frac{\pi n}{2}\right)$.

>[!info] Grafico di $\cos\left(\frac{\pi n}{2}\right)$
>![[filtro-es-3.png]]

Il segnale $x[t]$ è una [[Funzione Periodica]] di periodo $4$.

L'uscita del filtro si può calcolare attraverso la convoluzione con la risposta impulsiva.
$$
y[n]=\left\{\begin{matrix*}
\hphantom{-}1/2 & n=0\\
\hphantom{-}0 & n=1\\
-1/2& n=2\\
\hphantom{-}0 & n=3
\end{matrix*}\right.
$$