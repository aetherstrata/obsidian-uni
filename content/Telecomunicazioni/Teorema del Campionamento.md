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

## Esercizi
### Esercizio 1
Dato il seguente segnale analogico $x(t)$, verificare se è possibile ricostruirlo correttamente usando una frequenza di campionamento $f_c=4f_0$ e un filtro $H(f)=\operatorname{rect}\left(\frac{f}{4f_0}\right)$.
$$
x(t)=2f_0 \operatorname{sinc}^2(f_0t)\cdot\cos(6\pi f_0t)
$$
A questo punto si calcola il suo spettro $X(f)$.
$$
\begin{align*}
X(f)&=2\operatorname{tri}\left(\frac{f}{f_0}\right)*\left[\frac{1}{2}\delta(f-3f_0)+\frac{1}{2}\delta(f+3f_0)\right]=\\
&=\operatorname{tri}\left(\frac{f-3f_0}{f_0}\right)+\operatorname{tri}\left(\frac{f+3f_0}{f_0}\right)
\end{align*}
$$
>[!info] Grafico dello spettro
>![[1731856141.png]]
>L'asse delle frequenze è in scala di $f_0$.

Calcolando lo spettro del segnale campionato si può vedere se le componenti si intersecano tra loro.
$$
\begin{align*}
X_c(f)&=4f_0\sum_{n=-\infty}^{+\infty}\operatorname{tri}\left(\frac{f-3f_0-n4f_0}{f_0}\right)+\operatorname{tri}\left(\frac{f+3f_0-n4f_0}{f_0}\right)=\\
&=4f_0\sum_{n=-\infty}^{+\infty}\operatorname{tri}\left(\frac{f-(4n+3)f_0}{f_0}\right)+\operatorname{tri}\left(\frac{f-(4n-3)f_0}{f_0}\right)
\end{align*}
$$
>[!info] Grafico dello spettro periodicizzato
>![[1731857178.png]]
>Le repliche invadono lo spettro di altre repliche. 
>Nel grafico le repliche per $n=1$ (blu) e $n=-1$ (verde) invadono lo spettro del segnale originale $n=0$ (rosso).

Conoscendo lo spettro del segnale campionato si può procedere con la ricostruzione del segnale.
$$
Y(f)=X_c(f)\cdot H(f)=4f_0\operatorname{tri}\left(\frac{f-f_0}{f_0}\right)+4f_0\operatorname{tri}\left(\frac{f+f_0}{f_0}\right)
$$
L'ultimo passo è calcolare l'anti-trasformata del segnale di uscita.
$$
y(t)=8f_0\operatorname{sinc}^2(f_0t)\cdot\cos(2\pi f_0t)
$$
Il segnale non può essere ricostruito correttamente.
### Esercizio 2
Un segnale $x(t)$ ha uno spettro $X(f)$ con frequenza massima di $50$ Hz che viene campionato a $100$ Hz.

Dopo il campionamento, i campioni estratti sono i seguenti
$$
x(nT)=\begin{cases}
-1 & n\in\{-2,-1\} \\
\hphantom{-}1 & n\in\{1,2\} \\
\hphantom{-}0 & \text{altrove}
\end{cases}
$$
In ricostruzione si usa un filtro con la seguente funzione di trasferimento
$$
H(f)=T\operatorname{rect}(Tf)
$$
Calcolare il valore del segnale ricostruito all'istante $t=5ms$.

>[!info] Campioni del segnale
>Sapendo che il segnale è non nullo solo per alcuni valori di $n$, si può riscrivere la funzione come somma di [[Delta di Dirac]].
>$$
>\begin{align*}
>x_c(t)&=x(t)\cdot\Pi(t)=\sum_{n=-\infty}^{+\infty}x(nT)\delta(t-nT)= \\
>&=x(2T)\delta(t-2T)+x(T)\delta(t-T)+{} \\
>&\hphantom{=}+x(-T)\delta(t+T)+x(-2T)\delta(t+2T)= \\
>&=\delta(t-2T)+\delta(t-T)-\delta(t+T)-\delta(t+2T)
>\end{align*}
>$$

Sapendo che l'uscita del filtro vale $y(t)=x_c(t)*h(t)$, il segnale può essere ricostruito facilmente.

>[!info] Risposta impulsiva
>Data la funzione di trasferimento $H(f)=T\operatorname{rect}(Tf)$, si vede chiaramente che la risposta impulsiva associata è un [[Seno Cardinale]].
>$$
>H(f)=T\operatorname{rect}(Tf)\overset{\mathcal{F^{-1}}}{\longrightarrow}\operatorname{sinc}\left(\frac{t}{T}\right)=h(t)
>$$

L'uscita $y(t)$ del filtro è una somma di seni cardinali.
$$
\begin{align*}
y(t)&=x_c(t)*h(t)=\\
&=\left[\delta(t-2T)+\delta(t-T)-\delta(t+T)-\delta(t+2T)\right]*\operatorname{sinc}\left(\frac{t}{T}\right)=\\
&=\operatorname{sinc}\left(\frac{t-2T}{T}\right)+\operatorname{sinc}\left(\frac{t-T}{T}\right)-\operatorname{sinc}\left(\frac{t+T}{T}\right)-\operatorname{sinc}\left(\frac{t+2T}{T}\right)
\end{align*}
$$
Per valutare l'uscita in $t=5ms$ basta sostituire i valori, tenendo conto che il tempo di campionamento $T$ vale $\dfrac{1}{f_c}=10ms$.
$$
\begin{align*}
y(5ms)&=\cancel{\operatorname{sinc}\left(\frac{-15}{10}\right)}+\operatorname{sinc}\left(\frac{-5}{10}\right) -
\cancel{\operatorname{sinc}\left(\frac{15}{10}\right)}-\operatorname{sinc}\left(\frac{25}{10}\right)=\\
&=\operatorname{sinc}\left(\frac{1}{2}\right)-\operatorname{sinc}\left(\frac{5}{2}\right)=\\
&=\frac{\sin\left(\frac{\pi}{2}\right)}{\frac{\pi}{2}}-\frac{\sin\left(\frac{5\pi}{2}\right)}{\frac{5\pi}{2}}=\\
&=\frac{2}{\pi}-\frac{2}{5\pi}=\\
&=\frac{8}{5\pi}
\end{align*}
$$