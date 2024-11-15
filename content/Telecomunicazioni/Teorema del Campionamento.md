---
tags:
  - segnale
  - teorema
---
Il teorema del campionamento definisce la frequenza di campionamento minimo affinché il [[Segnale]] analogico possa essere ricostruito senza perdita di informazioni.
$$
x_c[n]\longrightarrow\boxed{\vphantom{\int}\text{ DAC }}\longrightarrow x(t)\\
$$
## Dimostrazione 
### Lo spettro del segnale
Il [[Segnale Campionato]] ha lo stesso spettro del segnale analogico, ma ripetuto per il tutto il dominio.
$$
X_c(f)=X(f)*\Pi(f)=X(f)*\frac{1}{T}\sum_{n=-\infty}^{+\infty}\delta\left(f-\frac{n}{T}\right)=\frac{1}{T}\sum_{n=-\infty}^{+\infty}X\left(f-\frac{n}{T}\right)
$$
Quindi per riottenere lo spettro del segnale analogico basta avere un [[Filtro]] passa-basso che faccia tornare lo spettro ad ampiezza normale.
$$
H(f)=T\operatorname{rect}(Tf)
$$
>[!info] Grafico
>![[segnale-campionato-filtro.png]]
### La sua uscita
Conoscendo la [[Filtro#Funzione di trasferimento|funzione di trasferimento]] del filtro, si può ricavarne l'uscita. 

>[!info] Risposta impulsiva
>In questo caso è più evidente se prima si anti-trasforma la funzione di trasferimento, si ottiene la risposta impulsiva e si lavora nel dominio del tempo.
>$$
>H(f)=T\operatorname{rect}(Tf)\overset{\mathcal{F}}{\longrightarrow}\operatorname{sinc}\left(\frac{t}{T}\right)
>$$

Facendo la convoluzione tra la [[Filtro#Risposta impulsiva|risposta impulsiva]] e il segnale campionato, si ottiene l'uscita.
$$
\begin{align*}
y(t)&=\sum_{n=-\infty}^{+\infty}x[n]\delta(t-nT)*\operatorname{sinc}\left(\frac{t}{T}\right)=\\
&=\sum_{n=-\infty}^{+\infty}x[n]\operatorname{sinc}\left(\frac{t-nT}{T}\right)
\end{align*}
$$
L'uscita sarà uguale al segnale analogico originale ricostruito.
### Condizioni
Per poter ricostruire un segnale bisogna però tener conto di alcune condizioni necessarie affinché questo avvenga con successo.
#### La banda
La banda del segnale analogico deve essere finita e limitata ed avere una **frequenza massima**
#### Il campionamento
La **frequenza di campionamento** deve essere scelta accuratamente per ottenere un'uscita priva di **aliasing**.

>[!info] Criterio di Nyquist
>$$
>\frac{1}{T}=f_c>2f_{max}
>$$

