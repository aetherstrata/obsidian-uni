---
tags:
  - segnale
  - sistema
  - digitale
---
Il [[Segnale]] analogico in entrata in un convertitore analogico-digitale può assumere infiniti valori nel campo dei reali. Per poterlo elaborare digitalmente, al segnale viene applicata una **quantizzazione**, ovvero una mappatura dei valori in ingresso ad un insieme numerabile di valori di uscita.
## Valori
Solitamente si usano insiemi con cardinalità uguale a una potenza del $2$.
$$
|M|=2^k\quad k:\text{numero di bit}
$$
Quindi, per poter trasmettere almeno $M$ valori diversi, bisogna usare una quantizzazione ad almeno $\log_2M$ bit.
## Errore
A differenza del [[Teorema del Campionamento|campionamento]], nella quantizzazione c'è perdita di informazioni del segnale (anche se limitato in banda).
$$
e_q[n]=x[n]-x_q[n]
$$
Questo errore è una [[Variabile Aleatoria]] (per semplicità si assume [[Distribuzione di Probabilità#Distribuzione uniforme|uniformemente distribuita]]) sull'intervallo di quantizzazione $\Delta$.
$$
P_E(e)=\frac{1}{\Delta}\operatorname{rect}\left(\frac{e}{\Delta}\right)
$$
Essendo una variabile aleatoria, e non un processo dipendente dal tempo, anche le sue statistiche sono indipendenti dal tempo.
$$
P_E=\frac{1}{\Delta}\int_{-\Delta/2}^{\Delta/2}e^2\,de=\left.\frac{e^3}{3\Delta}\right|_{-\Delta/2}^{\Delta/2}=\frac{\Delta^2}{12}
$$