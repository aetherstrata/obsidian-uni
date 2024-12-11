---
tags:
  - segnale
  - sistema
  - digitale
---
Il [[Segnale]] analogico in entrata in un convertitore analogico-digitale può assumere infiniti valori nel campo dei reali. Per poterlo elaborare digitalmente, al segnale viene applicata una **quantizzazione**, ovvero una mappatura dei valori in ingresso ad un insieme numerabile di valori di uscita.
## Valori
Il valore che può assumere il segnale in un dato istante di tempo è una [[Variabile Aleatoria]] (per semplicità si assume [[Distribuzione di Probabilità#Distribuzione uniforme|uniformemente distribuita]]) sull'intervallo $2V$ centrato nell'origine.
$$
p_X(x)=\frac{1}{2V}\operatorname{rect}\left(\frac{x}{V}\right)
$$
Essendo una variabile aleatoria, e non un processo dipendente dal tempo, anche le sue statistiche sono indipendenti dal tempo.
$$
P_X=\frac{1}{2V}\int_{-V}^{V}x^2\,dx=\left.\frac{x^3}{6V}\right|_{-V}^{+V}=\frac{V^2}{3}
$$
## Mappatura
Solitamente si usano insiemi con cardinalità uguale a una potenza del $2$.
$$
|M|=2^k\quad k:\text{numero di bit}
$$
Quindi, per poter trasmettere almeno $M$ valori diversi, bisogna usare una quantizzazione ad almeno $\log_2M$ bit.

Nella maggior parte dei sistemi, questi livelli $M$ sono equamente spaziati tra loro di un intervallo $\Delta$, con valore di picco $V$.
$$
\Delta = \frac{2V}{M}
$$
## Errore
A differenza del [[Teorema del Campionamento|campionamento]], nella quantizzazione c'è perdita di informazioni del segnale (anche se limitato in banda).
$$
e_q[n]=x[n]-x_q[n]
$$
Questo errore è una [[Variabile Aleatoria]] (per semplicità si assume [[Distribuzione di Probabilità#Distribuzione uniforme|uniformemente distribuita]]) sull'intervallo di quantizzazione $\Delta$.
$$
p_E(e)=\frac{1}{\Delta}\operatorname{rect}\left(\frac{e}{\Delta}\right)
$$
Essendo una variabile aleatoria, e non un processo dipendente dal tempo, anche le sue statistiche sono indipendenti dal tempo.
$$
P_E=\frac{1}{\Delta}\int_{-\Delta/2}^{\Delta/2}e^2\,de=\left.\frac{e^3}{3\Delta}\right|_{-\Delta/2}^{+\Delta/2}=\frac{\Delta^2}{12}
$$
Questa formula si può riscrivere sostituendo $\Delta$.
$$
P_E=\frac{\Delta^2}{12}=\frac{V^2}{3M^2}
$$
È comune studiare la potenza in scala logaritmica.
$$
\begin{align*}
(P_E)_{dB}&=10\log_{10}P_E=\\
&=10\log_{10}\frac{V^2}{3M^2}=\\
&=10\log_{10}\frac{V^2}{3}-10\log_{10}M^2=\\
&=10\log_{10}P_X-10\log_{10}2^{2k}
\end{align*}
$$
Tenendo conto che $10\log_{10}2$ si può approssimare a $3$, il secondo termine può essere semplificato.
$$
(P_E)_{dB}=10\log_{10}P_X-6k
$$
Il primo termine è invece uguale alla potenza di $X$ calcolata in decibel.
$$
(P_E)_{dB}=(P_X)_{dB}-6k
$$
