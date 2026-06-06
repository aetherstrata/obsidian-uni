---
tags:
  - trasformata
  - analisi
  - segnale
  - operatore
  - matematica
---
La trasformata discreta è un particolare tipo di [[Trasformata di Fourier]] che converte un insieme di campioni equispaziati di una funzione in una combinazione lineare di armoniche
$$
x[n]\overset{\mathcal{F}}{\longrightarrow} X(f)=\underbracket{\sum_{n=-\infty}^{+\infty}x[n]e^{-i2\pi fnT}}_{\text{serie di Fourier}}
$$
La rappresentazione in frequenza di un segnale tempo-discreto è scritta come una [[Serie di Fourier]] di periodo $1/T$.
## Anti-trasformata
$$
X(f)\overset{\mathcal{F^{-1}}}{\longrightarrow}x[n]=T\int_{-1/2T}^{1/2T}X(f)e^{i2\pi nfT}df
$$
## Periodicità
La trasformata $X(f)$ di un segnale tempo-discreto ha periodo $1/T$.
### Dimostrazione
Per essere dimostrato, $X(f)$ deve essere uguale a $X(f-1/T)$.
$$
X\left(f-\frac{1}{T}\right)=\sum_{n=-\infty}^{+\infty}x[n]e^{-i2\pi \left(f-\frac{1}{T}\right)nT}=\sum_{n=-\infty}^{+\infty}x[n]e^{-i2\pi fnT}e^{i2\pi n}
$$
Il termine $e^{i2\pi n}$ vale sempre $1$ e può essere cancellato. La parte rimanente è proprio uguale alla trasformata calcolata in $f$.
$$
X\left(f-\frac{1}{T}\right)=\sum_{n=-\infty}^{+\infty}x[n]e^{-i2\pi nfT}=X(f)
$$
## Proprietà
### Traslazione nel tempo
$$
x[n-n_0]\overset{\mathcal{F}}{\longrightarrow}X(f)e^{-i2\pi n_0fT}
$$
### Traslazione in frequenza
$$
x[n]e^{i2\pi nf_0T}\overset{\mathcal{F}}{\longrightarrow}X(f-f_0)
$$
### Convoluzione
La trasformata di un [[Convoluzione|segnale convoluto]] è il prodotto delle trasformate delle componenti.
$$
y[n]=x[n]*h[n]\overset{\mathcal{F}}{\longrightarrow}Y(f)=X(f)\cdot H(f)
$$
#### Dimostrazione
$$
\begin{gather*}
\begin{alignedat}{4}
y[n]&=x[n]*h[n]&&=\hphantom{\sum_{k=-\infty}^{+\infty}}\sum_{k=-\infty}^{+\infty}x[k]h[n-k]\\
Y(f)&=\sum_{n=-\infty}^{+\infty}y[n]e^{-i2\pi nfT}&&=\sum_{n=-\infty}^{+\infty}\sum_{k=-\infty}^{+\infty}x[k]h[n-k]e^{-i2\pi nfT}
\end{alignedat}\\\\
\boxed{n'\coloneqq n-k}\\\\
\begin{aligned}
Y(f)&=\sum_{k=-\infty}^{+\infty}x[k]\sum_{n'=-\infty}^{+\infty}h[n']e^{-i2\pi n'fT}e^{-i2\pi kfT}=\\
&=\underbrace{\sum_{k=-\infty}^{+\infty}x[k]e^{-i2\pi kfT}}_{X(f)}\underbrace{\sum_{n'=-\infty}^{+\infty}h[n']e^{-i2\pi n'fT}}_{H(f)}=\\
&=X(f)\cdot H(f)
\end{aligned}
\end{gather*}
$$
### Convoluzione circolare
$$
y[n]=x[n]\cdot h[n] \overset{\mathcal{F}}{\longrightarrow} Y(f)=T\int_{-1/2T}^{1/2T}X(\theta)\cdot H(f-\theta)\ d\theta
$$
#### Dimostrazione
$$
\begin{gather*}
y[n]=x[n]\cdot h[n]\\
Y(f)=\sum_{n=-\infty}^{+\infty}y[n]e^{-i2\pi nfT}=\sum_{n=-\infty}^{+\infty}x[n]h[n]e^{-i2\pi nfT}\\\\
\boxed{X(\theta)\overset{\mathcal{F^{-1}}}{\longrightarrow} x[n] = T\int_{-1/2T}^{1/2T}X(\theta)e^{i2\pi n\theta T}d\theta}\\\\
\begin{aligned}
Y(f)&=\sum_{n=-\infty}^{+\infty}h[n]e^{-i2\pi nfT}\cdot T\int_{-1/2T}^{1/2T}X(\theta)e^{i2\pi n\theta T}d\theta =\\
&=T\int_{-1/2T}^{1/2T}X(\theta)\underbrace{\sum_{n=-\infty}^{+\infty}h[n]e^{-i2\pi n(f-\theta)T}}_{H(f-\theta)}d\theta=\\
&=T\int_{-1/2T}^{1/2T}X(\theta)\cdot H(f-\theta)\ d\theta
\end{aligned}
\end{gather*}
$$
#### Periodicità dello spettro
Sapendo che gli spettri $X(f)$ e $H(f)$ di segnali discreti sono periodici, si possono riscrivere come treni di repliche $\overline{\!X}(f)$ e $\overline{\!H}(f)$ degli spettri traslate sul il dominio delle frequenze.
$$
\begin{align*}
X(f)&=\sum_{n=-\infty}^{+\infty}\overline{\!X}\left(f-\frac{n\vphantom{k}}{T}\right)\\
H(f)&=\sum_{k=-\infty}^{+\infty}\overline{\!H}\left(f-\frac{k}{T}\right)
\end{align*}
$$
Queste sommatorie si possono sostituire nella formula ricavata sopra.
$$
\begin{gather*}
Y(f)=T\sum_{n=-\infty}^{+\infty}\sum_{k=-\infty}^{+\infty} \int_{-1/2T}^{1/2T}\overline{\!X}\left(\theta-\frac{n\vphantom{k}}{T}\right)\cdot \overline{\!H}\left(f-\theta-\frac{k}{T}\right)\ d\theta\\\\
\boxed{\ \theta'\coloneqq \theta-\frac{n}{T}\ \vphantom{\int}}\\\\
\begin{aligned}
Y(f)&=T\sum_{n=-\infty}^{+\infty}\sum_{k=-\infty}^{+\infty} \int\limits_{\displaystyle\tiny-\frac{1}{2T}-\frac{n}{T}}^{\displaystyle\tiny\frac{1}{2T}-\frac{n}{T}}\overline{\!X}(\theta')\cdot \overline{\!H}\left(f-\theta'-\frac{k+n}{T}\right)\ d\theta'
\end{aligned}
\end{gather*}
$$
Andando a sommare sui vari periodi $1/T$ traslati di $n$, tutte le somme sono uguali a $0$ tranne che per $n=0$ perché il segnale integrando  $\overline{\!X}(\theta')$ è uguale a $0$ per tutto il dominio tranne che attorno l'origine.
$$
Y(f)=T\sum_{k=-\infty}^{+\infty} \underbrace{\int_{-1/2T}^{1/2T}\overline{\!X}(\theta') \cdot \overline{\!H}\left(f-\theta'-\frac{k}{T}\right)\ d\theta'}_{\overline{Y}\left(f-\frac{k}{T}\right)}
$$
Sia  $\overline{\!Y}(f)$ la convoluzione  $\overline{\!X}(f)*\overline{\!H}(f)$, allora la trasformata $Y(f)$ si può riscrivere come sommatoria delle convoluzioni traslate delle repliche, dato che l'integrale si può riscrivere tra $-\infty$ e $+\infty$, in quanto è nullo per tutti i valori fuori dal periodo.
$$
Y(f)=T\sum_{k=-\infty}^{+\infty}\overline{Y}\left(f-\frac{k}{T}\right)
$$
### Coniugazione
$$
y^*[n] \overset{\mathcal{F}}{\longrightarrow} \sum_{n=-\infty}^{+\infty}y^*[n]e^{-i2\pi nfT}=\left[\sum_{n=-\infty}^{+\infty}y[n]e^{i2\pi nfT}\right]^*=Y^*(-f)
$$
