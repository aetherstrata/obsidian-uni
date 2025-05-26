---
tags:
  - sistema
  - segnale
  - digitale
---
Un [[Segnale Campionato]] ideale è formato da una serie di [[Delta di Dirac|impulsi]], ma questo non è realizzabile. Serve un sistema che mantiene il valore fino al campione successivo. 

![[Pasted image 20250526213857.png]]

Quando il valore rimane costante si parla di organo di tenuta _di ordine zero_ (ZOH). Il suo comportamento può essere descritto come dei [[Segnale Finestra#Finestra rettangolare|segnali rettangolari]] che hanno ampiezza uguale al valore del campione e lunghezza pari al tempo di campionamento. Questo può essere anche scritto come la somma di due [[Segnale Gradino|gradini]].
$$
\begin{align*}
x_{H}(t) &= x_c(t) * \left[\delta_1(t)-\delta_1(t-T_c)\right] =\\
&= \sum_{k=0}^{+\infty}\left[x_k\delta_1(t-kT_c)-x_k\delta_1(t-(k+1)T_c)\right] \\
x_H(S) &= \sum_{k=0}^{+\infty}\left[x_k\frac{1}{s}e^{-skT_c}-x_k\frac{1}{s}e^{-s(k+1)T_c}\right] =\\
&= \underbrace{\vphantom{\sum_{k=0}^{+\infty}}\frac{1-e^{-sT_C}}{s}}_{T(S)}\underbrace{\sum_{k=0}^{+\infty}x_ke^{-skT_C}}_{X_c(S)}
\end{align*}
$$
Quindi il segnale a gradino si ottiene moltiplicando il segnale campionato $X_c(S)$ per la funzione
di trasferimento dell’organo di tenuta $T(S)$. 
$$
\begin{align*}
e^x &= 1+x \qquad\qquad\qquad\qquad \text{espansione in serie di Taylor}\\\\
e^{sT_c} &= 1 + sT_C \quad\longrightarrow\quad T(S)=\frac{1-e^{-sT_C}}{s}=\frac{1-\frac{1}{1+sT_C}}{s} = \frac{sT_c}{1+sT_C}
\end{align*}
$$
Si integra per eliminare lo zero.
$$
T(S) = \frac{1}{s}\frac{sT_c}{1+sT_C} = \frac{T_c}{1+sT_C}
$$
## Risposta armonica
Mentre il filtro ricostruttore ideale presenta un taglio netto esattamente in $\dfrac{\omega_c}{2}$, il modulo dell'organo di tenuta di ordine zero comincia a scendere già a $\dfrac{\omega_c}{2\pi}$.

![[Pasted image 20250526221322.png]]

Quindi, se con il filtro ideale si può scegliere una frequenza di campionamento almeno pari al doppio della frequenza massima del segnale, usando questo organo di tenuta bisogna scegliere una frequenza di campionamento di almeno $\approx 6\omega_{MAX}$.
$$
\begin{align*}
\text{ricostruttore ideale} ]\quad\longrightarrow\quad & \omega_{MAX}<\frac{\omega_c}{2} \to \omega_c > 2\omega_{MAX}\\
\text{organo di tenuta} \quad\longrightarrow\quad & \omega_{MAX}<\frac{\omega_c}{2\pi} \to \omega_c > 2\pi\omega_{MAX}\ \approxeq\ 6\omega_{MAX}\\
\end{align*}
$$
