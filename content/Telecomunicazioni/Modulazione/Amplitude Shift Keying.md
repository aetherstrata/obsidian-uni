---
tags:
  - modulazione
  - segnale
  - digitale
---
La modulazione a cambiamenti di ampiezza, detta anche **ASK**, è un tipo di modulazione passa-banda in cui i simboli del segnale sono codificati nell'ampiezza che assume l'onda portante.
$$
z(t)=x(t)\cos(2\pi f_ot)=c_ng(t)\cos(2\pi f_ot)
$$
Il segnale in ingresso è [[Trasformata di Fourier#Traslazione in frequenza|traslato in frequenza]] sulla portante del coseno.
$$
Z(f)=\frac{c_n}{2}\left[G(f-f_0)+G(f+f_0)\right]
$$
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


