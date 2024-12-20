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
### Esercizio 3
Dato il segnale $x(t)=8\operatorname{sinc}(4t)\cos(6\pi t)$ campionato con tempo $T_c$ pari al massimo valore per evitare l'aliasing. 
Il segnale è poi ricostruito con un filtro passa-basso ideale $H(f)$ ad ampiezza unitaria e spettro di frequenza compreso tra $[-3,3]$. 
Calcolare il valore di $T_c$, il segnale ricostruito e la sua trasformata.
$$
H(f)=\operatorname{rect}\left(\frac{f}{6}\right)\quad\longrightarrow\quad h(t)=6\operatorname{sinc}(-6t)=6\operatorname{sinc}(6t)
$$
Si comincia studiando la rappresentazione in frequenza del segnale $x(t)$.
$$
\begin{align*}
x(t)\overset{\mathcal{F}}{\longrightarrow}X(f)&=8\cdot\mathcal{F}\set{\operatorname{sinc}(4t)}*\mathcal{F}\set{\cos(6\pi t)}=\\
&=8\cdot\frac{1}{4}\operatorname{rect}\left(\frac{f}{4}\right)*\left[\frac{1}{2}\delta(f-3)+\frac{1}{2}\delta(f+3)\right]=\\
&=\operatorname{rect}\left(\frac{f-3}{4}\right)+\operatorname{rect}\left(\frac{f+3}{4}\right)
\end{align*}
$$
>[!info] Grafico della trasformata
>![[campionamento-es-3.png]]

Da questa si può ricavare la trasformata del segnale campionato moltiplicando lo spettro del segnale per il treno campionatore.
$$
\begin{align*}
X_c(f)&=X(f)\cdot\frac{1}{T_c}\sum_{n}\delta\left(f-\frac{n}{T_c}\right)=\\
&=\frac{1}{T_c}\sum_{n}X\left(f-\frac{n}{T_c}\right)
\end{align*}
$$
La frequenza di campionamento deve essere almeno il doppio della massima frequenza del segnale, quindi $f_c=10$. 
$$
T_c=\frac{1}{f_c}=\frac{1}{10}
$$
Sostituendo il valore nella trasformata si ottiene la forma finale.
$$
\begin{align*}
X_c(f)&=10\sum_{n}X(f-10n)=\\
&=10\sum_{n}\left[\operatorname{rect}\left(\frac{f-10n-3}{4}\right)+\operatorname{rect}\left(\frac{f-10n+3}{4}\right)\right]
\end{align*}
$$
L'aliasing non è un problema in quanto le repliche non si sovrappongono.

Per ricostruire il segnale si moltiplica la trasformata del segnale campionato per la funzione di trasferimento del [[Filtro]].
$$
X'(f)=X_c(F)H(f)
$$
>[!info] Funzione di trasferimento
>![[1734706613.png]]
>$$
>\downarrow
>$$
>![[1734707026.png]]

$$
\begin{align*}
X'(f)&=10\left[\operatorname{rect}\left(\frac{f-2}{2}\right)+\operatorname{rect}\left(\frac{f+2}{2}\right)\right]=\\
&=20\cdot\operatorname{rect}\left(\frac{f}{2}\right)*\left[\frac{1}{2}\delta(f-2)+\frac{1}{2}\delta(f+2)\right]
\end{align*}
$$
Per ottenere il segnale ricostruito si applica l'anti-trasformata allo spettro ricostruito.
$$
\begin{align*}
x'(t)&=20\cdot\mathcal{F}^{-1}\left\{\operatorname{rect}\left(\frac{f}{2}\right)*\left[\frac{1}{2}\delta(f-2)+\frac{1}{2}\delta(f+2)\right]\right\}=\\
&=20\cdot2\operatorname{sinc}(2t)\cos(4\pi t)
\end{align*}
$$
### Esercizio 4
Dato un segnale $x(t)$, campionato con il massimo intervallo $T_c$ necessario per evitare l'aliasing, determinare il segnale ricostruito $x'(t)$ e la sua trasformata $X'(f)$. 

Il segnale campionato è ricostruito usando un filtro passa-basso ideale con ampiezza $T$ e intervallo di frequenze $\left[-\dfrac{1}{2T};\dfrac{1}{2T}\right]$ .
$$
H(f)=T\operatorname{rect}(fT)\quad\longrightarrow\quad h(t)=\operatorname{sinc}\left(\frac{t}{T}\right)
$$
Il segnale campionato ha solo due campioni non nulli.
$$
x[-1]=x[1]=1
$$

