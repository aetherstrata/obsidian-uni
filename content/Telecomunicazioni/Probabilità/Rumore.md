---
tags:
  - sistema
  - statistica
---
Il rumore è un segnale indesiderato di disturbo delle informazioni in un [[Sistema]]. Il rumore viene definito come una somma di oscillazioni irregolari, intermittenti e/o [[Processo aleatorio|statisticamente casuali]].
## Proprietà
### Additivo
Il rumore è *additivo* se si somma al processo interessato.
$$
z(t)=x(t)+y(t)
$$
### Gaussiano
Il rumore è *gaussiano* se rispetta le caratteristiche di una [[Distribuzione di Probabilità#Distribuzione normale|distribuzione normale]].
### Bianco
Il rumore è *bianco* se ha valore atteso nullo e spettro densità di potenza costante.
$$
\mathbb{E}[w(t)]=0\quad;\quad G_W(f)=K
$$
## AWGN
Il rumore gaussiano bianco additivo rappresenta il **rumore termico** generato dall'agitazione dei portatori di carica.
### Proprietà di un segnale disturbato
Le variabili aleatorie sono incorrelate e possono essere fattorizzate.
#### Valore atteso
$$
\begin{align*}
\mathbb{E}[z(t)]&=\int_{-\infty}^{\infty}z(t)P_Z(z)\,dz =\\
&=\iint_{-\infty}^{\infty}[x(t)+y(t)]P_{X|Y}(x,y)\,dxdy=\\
&=\int_{-\infty}^{\infty}x(t)P_X(x)\,dx+\int_{-\infty}^{\infty}y(t)P_Y(y)\,dy=\\
&=\mu_X+\mu_Y
\end{align*}
$$
#### Varianza
$$
\sigma^2_Z=\sigma^2_X+\sigma^2_Y
$$
#### Correlazione
$$
\begin{align*}
R_Z(\tau)&=\mathbb{E}[z(t+\tau)z(t)]=\\
&=\mathbb{E}[(x(t+\tau)+y(t+\tau))(x(t)+y(t))]=\\
&=
\end{align*}
$$