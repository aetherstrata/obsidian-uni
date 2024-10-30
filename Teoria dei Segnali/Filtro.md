---
tags:
  - segnale
  - sistema
---
Un filtro è un [[Sistema|sistema]] [[Linearità|lineare]] [[Invarianza temporale|tempo-invariante]] ed è univocamente caratterizzato da una [[#Risposta Impulsiva|risposta impulsiva]] $h(t)$.
$$x(t) \to\boxed{\quad h(t)\quad}\to y(t)$$
La sua uscita si può sempre ricavare dalla [[Convoluzione|convoluzione]] tra il suo ingresso e la risposta impulsiva.
$$y(t) = x(t)*h(t)$$
## Risposta impulsiva
 La risposta impulsiva è la funzione caratterizzante di un filtro e descrive il suo comportamento in risposta ad un [[Delta di Dirac|impulso]]. 
$$\delta(t) \to\boxed{\text{ sistema }}\to h(t)$$
### Esempi

Data la riposta impulsiva $h(t) = 2\delta(t-2) + u(t-2)$, calcolare l'uscita del filtro quando è applicato in ingresso un gradino $u(t)$.
$$y(t) = u(t)*[2\delta(t-2) + u(t-2)] = [u(t)*2\delta(t-2)] + [u(t)*u(t-2)]$$
Applicando la proprietà distributiva, si ottengono due convoluzioni elementari da risolvere:

- Per la convoluzione con la [[Delta di Dirac]] basta traslare la funzione.
$$u(t)*2\delta(t-2) = 2u(t-2)$$
- Per la convoluzione con il [[Segnale Gradino|gradino]] il risultato sarà una [[Segnale Rampa|rampa]].
$$\begin{align}
u(t)*u(t-2) &= \int_{-\infty}^{\infty}u(\tau)u(t-\tau-2)\ d\tau = \int_{0}^{\infty}u(t-\tau-2)\ d\tau=\\
&= \int_{0}^{t-2}\ d\tau = \tau\Big|_{0}^{t-2}=(t-2)u(t-2)
\end{align}$$
L'uscita seguirà quindi la funzione $y(t) = 2u(t-2) + (t-2)u(t-2) = t\cdot u(t-2)$.

## Funzione di trasferimento

La funzione di trasferimento è la [[Trasformata di Fourier]] della risposta impulsiva.
$$h(t)\overset{\mathcal{F}}{\longrightarrow}H(f)$$
### Filtri passa-basso
I filtri, o segnali, passa-basso si usano per trasmettere segnali la cui [[Trasformata di Fourier]] è limitata attorno l'origine. La banda del segnale è legata alla frequenza massima del segnale.
#### Esempio
Il segnale vocale è un segnale passa-basso poiché tutte le informazioni sono contenute tra pochi Hz e 20 kHz. Il segnale delle chiamate telefoniche è tagliato a circa 4 kHz.
### Filtri passa-banda
I filtri passa-banda, o segnali modulati, sono formati da due componenti, uno centrato in $f_0$ e l'altro in $-f_0$. Questa frequenza si chiama **portante**.
#### Esempio
Questo tipo di segnali si usa per le trasmissioni wireless, come WiFi e Bluetooth.