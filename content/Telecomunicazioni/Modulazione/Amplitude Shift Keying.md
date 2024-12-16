---
tags:
  - modulazione
  - segnale
  - digitale
---
La modulazione a cambiamenti di ampiezza, detta anche **ASK**, è un tipo di modulazione passa-banda in cui i simboli del segnale sono codificati nell'ampiezza che assume l'onda portante.
$$
z(t)=x(t)\cos(2\pi f_0t)=c_ng(t)\cos(2\pi f_0t)
$$
## Descrizione
Il segnale in ingresso è [[Trasformata di Fourier#Traslazione in frequenza|traslato in frequenza]] sulla portante del coseno.
$$
Z(f)=\frac{c_n}{2}\left[G(f-f_0)+G(f+f_0)\right]
$$
Alla ricezione, il segnale modulato è nuovamente moltiplicato per un coseno.
$$
\begin{align*}
y(t)&=2\,z(t)\cos(2\pi f_0t)=\\
&=2\,x(t)\cos^2(2\pi f_0t)=\\
&=2\,x(t)\left[\frac{1}{2}+\frac{1}{2}\cos(4\pi f_0t)\right]=\\
&=x(t)+x(t)\cos(4\pi f_ot)
\end{align*}
$$
Lo spettro del segnale elaborato dal coseno è uguale a quello del segnale inviato più lo spettro modulato.
$$
Y(f)=X(f)+\frac{X(f-2f_0)}{2}+\frac{X(f+2f_0)}{2}
$$
Applicando un [[Filtro]] passa-basso, si rimuovono le parti di spettro traslate e rimangono solo le frequenze del segnale originale.
## Condizioni
Dato un segnale modulante con frequenza massima $f_{max}$ e un segnale portante di banda $B$, la frequenza deve essere minore della metà della banda.
$$
f_{max}\le\frac{B}{2}
$$
Usando degli impulsi di Nyquist per modulare, la loro frequenza massima è metà della frequenza di simbolo.
$$
g(t)=\operatorname{sinc}\left(\frac{t}{T_s}\right)\overset{\mathcal{F}}{\longrightarrow}T_s\operatorname{rect}(T_sf)=G(f)
$$
Sostituendo $f_{max}$ nella disuguaglianza, si ottiene la relazione tra la frequenza di simbolo e la banda.
$$
\frac{1}{2T_s}\le\frac{B}{2} \quad\longrightarrow\quad f_s\le B
$$
Usando un coseno come segnale portante ha la conseguenza di sdoppiare le frequenze nel semiasse negativo, con il conseguente dimezzamento della banda utilizzabile.

