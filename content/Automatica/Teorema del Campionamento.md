---
tags:
  - matematica
  - digitale
  - segnale
  - teorema
---
Come visto anche nel [[Telecomunicazioni/Teorema del Campionamento|dominio di Fourier]],  il teorema del campionamento si applica anche al dominio di Laplace, secondo cui la frequenza di campionamento necessaria per evitare aliasing deve essere maggiore del doppio della frequenza massima del segnale analogico.
## Dimostrazione
Il [[Segnale Campionato]] presenta nel vari istanti di campionamento $kT_c$ un [[Delta di Dirac|impulso]] di ampiezza pari al valore del segnale nel punto.
$$
x_c(t)=x(t)\cdot\pi(t)=\sum_{k=0}^{+\infty}x(t)\delta_0(t-kT_c)\quad \overset{\mathcal{L}}{\longrightarrow}\quad X_c(S) = \sum_{k=0}^{+\infty}x(t)e^{-skT_c}
$$
Ricordando che la pulsazione di campionamento $\omega_c$ è uguale a $2\pi f_c$, e che il periodo di campionamento $T_c$ è uguale a $1/f_c$, allora $T_c=2\pi/\omega_c$. Esplicitando $s$ come $\sigma +j\omega$ si ottiene
$$
X_c(S) = \sum_{k=0}^{+\infty}x(t)\exp({-skT_c}) = \sum_{k=0}^{+\infty}x(t)\exp\left({-\sigma k\frac{2\pi}{\omega_c}}\right)\exp\left({-j\omega k\frac{2\pi}{\omega_c}}\right)
$$
L'esponenziale è periodico nella parte immaginaria, quindi il piano complesso viene diviso in fasce orizzontali, dato che la fase si ripete ogni $\omega_c$. Tutte le fasce riproducono il contenuto della fascia principale, passante per l'origine.

Per completare la dimostrazione si trasforma il segnale in una [[Serie di Fourier]].
$$
x(t) =  \sum_{k=-\infty}^{+\infty} c_ke^{jk\omega_ct}
$$
Da questa rappresentazione si possono ricavare i coefficienti applicando il prodotto scalare.
$$
c_k=\left<x(t), e^{\large jk\omega_ct}\right>=\frac{1}{T_c}\int_{-T_c/2}^{T_c/2} x(t)e^{-jk\omega_ct}\,dt
$$
Si comincia applicando la serie solamente alla parte periodica del segnale campionato, ovvero il treno campionatore.
$$
c_k =\frac{1}{T_c}\int_{-T_c/2}^{T_c/2} \sum_{k=0}^{+\infty}\delta_0(t-kT_c)\, e^{-jk\omega_ct}\,dt
$$
Tra $-\dfrac{T_c}{2}$ e  $\dfrac{T_c}{2}$ c'è sempre solo un impulso, quindi $c_k$ è sempre uguale a  $\dfrac{1}{T_c}$. La parte periodica del segnale si può quindi riscrivere come serie di esponenziali.
$$
x_c(t)=x(t)\sum_{k=-\infty}^{+\infty} \frac{1}{T_c}e^{jk\omega_ct} = \frac{1}{T_c}\sum_{k=-\infty}^{+\infty} x(t)\,e^{jk\omega_ct}
$$
Questa rappresentazione si può riportare nel dominio di Laplace calcolandone la trasformata.
$$
\frac{1}{T_c}\sum_{k=-\infty}^{+\infty} x(t)\,e^{jk\omega_ct}\quad \overset{\mathcal{L}}{\longrightarrow}\quad \frac{1}{T_c}\sum_{k=-\infty}^{+\infty} \mathcal{L}\left\{x(t)\,e^{jk\omega_ct}\right\} = \frac{1}{T_c}\sum_{k=-\infty}^{+\infty} X(s-jk\omega_c)
$$
Facendo uno studio sull'asse immaginario, ponendo $s=j\omega$, si ottengono i diagrammi della risposta armonica. Il risultato sono ripetizioni lungo tutto l'asse del contenuto spettrale del segnale ce centrate in $k\omega_c$. 

Per ricostruire il segnale originale bisogna filtrare le repliche derivanti dal campionamento.
### No aliasing
Se le repliche sono distanziate tra loro di $\omega_c >\omega_{MAX}$, allora non è presente aliasing e si può prendere lo spettro compreso tra $-\omega_c/2$ e $\omega_c/2$. Per riottenere il segnale iniziale si calcola l'antitrasformata e si interpolano dei [[Seno Cardinale|seni cardinali]] centrati sugli impulsi.
$$
\operatorname{sinc}\left(\frac{\omega_c}{2}\right) = \frac{\sin \frac{\omega_c}{2}t}{\frac{\omega_c}{2}t} = h(t)
$$
Interpolando gli impulsi con $h(t)$ si ottiene il segnale originale. Questa metodologia è il
ricostruttore ideale di Shannon.
$$
y(t) = x_c(t)*h(t)
$$
Bisogna evidenziare che se lavoriamo su sistemi real-time, dove i campioni vengono prodotti nello stesso momento dell'elaborazione, questo tipo di ricostruttore non è applicabile, poiché il [[Seno Cardinale]] è un filtro non causale.
### Aliasing
Se le repliche sono distanziate tra loro di $\omega_c < \omega_{MAX}$, allora le parti a frequenza maggiore saranno sovrapposte.