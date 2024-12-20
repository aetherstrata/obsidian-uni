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
## Soglia
Quando si trasmette un segnale quantizzato su un canale, c'è la possibilità che il valore di un simbolo cambi in modo significativo a causa del [[Rumore]] immesso. 

Di solito si sceglie una soglia a metà tra due simboli adiacenti per definire quali valori siano associati ad un simbolo e quali all'altro.
### Sopra la soglia

>[!info] Grafico
>![[errore-sopra-soglia.png]]

L'area in verde rappresenta la probabilità di trasmettere il simbolo $C_1$ e ricevere il simbolo $C_2$.
$$
P_e\set{y>\overline{C}\,|\,C_1}=\int_{\overline{C}}^{\infty}\frac{1}{\sqrt{2\pi}\sigma_n}\cdot e^{-\frac{(y-c_1)^2}{2\sigma_n^2}}dy=\frac{1}{2}\operatorname{erfc}\left(\frac{\overline{C}-C_1}{\sqrt{2}\sigma_n}\right)
$$
### Sotto la soglia

>[!info] Grafico
>![[sovrapposizione-simboli.png]]

L'area in rosso rappresenta la probabilità di trasmettere il simbolo $C_2$ e ricevere il simbolo $C_1$.
$$
P_e\set{y<\overline{C}\,|\,C_2}=\int_{-\infty}^{\overline{C}}\frac{1}{\sqrt{2\pi}\sigma_n}\cdot e^{-\frac{(y-c_2)^2}{2\sigma_n^2}}dy=\frac{1}{2}\operatorname{erfc}\left(\frac{C_2-\overline{C}}{\sqrt{2}\sigma_n}\right)
$$
### Bit Error Rate
Sommando la metà di queste due probabilità (visto che la probabilità di inviare uno $0$ o un $1$ è equamente distribuita), si ottiene il bit error rate (**BER**).
$$
\text{BER}=\frac{1}{4}\operatorname{erfc}\left(\frac{\overline{C}-C_1}{\sqrt{2}\sigma_n}\right)+\frac{1}{4}\operatorname{erfc}\left(\frac{C_2-\overline{C}}{\sqrt{2}\sigma_n}\right)
$$
Il BER è minimo per valori di $\overline{C}$ tale che $\overline{C}-C_1=C_2-\overline{C}$, quindi $\overline{C} = \dfrac{C_1+C_2}{2}$.
$$
\text{BER}=\frac{1}{2}\operatorname{erfc}\left(\frac{C_2-C_1}{2\sqrt{2}\sigma_n}\right)
$$
In questo ambito è solito definire una nuova funzione $Q(x)$ sulla funzione di errore complementare.
$$
Q(x)\coloneqq\frac{1}{2}\operatorname{erfc}\left(\frac{x}{\sqrt{2}}\right)
$$
Quindi, il bit error rate del segnale trasmesso si può riscrivere usando questa funzione.
$$
\text{BER}=Q\left(\frac{C_2-C_1}{2\sigma_n}\right)=Q\left(\frac{d}{2\sqrt{N_0B}}\right)
$$
