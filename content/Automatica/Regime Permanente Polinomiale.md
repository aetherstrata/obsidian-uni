---
tags:
  - sistema
  - proprietà
  - controllo
---
Per comportamento a regime si intende il comportamento del [[Sistema]] a controreazione dopo
un certo tempo dall'applicazione del segnale d’ingresso. L’analisi del comportamento a regime
è importante per comprendere con precisione il funzionamento del sistema, in particolare la sua
risposta. Una volta estinto il transitorio, la differenza che esiste tra il valore desiderato e quello
ottenuto identifica l’indice di fedeltà della risposta, o l’errore.

![[Pasted image 20250515201830.png]]

L'obiettivo è avere l'uscita del sistema $Y(S)$ il più vicino possibile all'uscita desiderata $Y_d(S)$, quindi l'errore di uscita $E(S)$ tra i due deve essere il più vicino possibile a $0$.
$$
y=y_d \quad \longrightarrow \quad \lim_{t\to\infty}e(t)=0
$$
L'errore è esprimibile con la seguente funzione di trasferimento, con $W(S)$ anello a ciclo chiuso.
$$
\begin{align*}
E(S) &= K_d\,U(s) - \frac{G(S)}{1+G(S)\frac{1}{K_d}}U(S) = \\
&= K_d\,U(s) - \frac{K_d\,G(S)}{K_d+G(S)}U(S) = \\
&= U(S)\left[K_d-\frac{K_d\,G(S)}{K_d+G(S)}\right] = \\
&= U(S)\left[\frac{K_d^2}{K_d+G(S)}\right]
\end{align*}
$$
Per ottenere il valore dell'uscita in $t\to\infty$ si può applicare il [[Trasformata di Laplace#Teorema del valor finale|Teorema del valor finale]], applicando un ingresso in forma canonica $\dfrac{t^k}{k!} \longrightarrow \dfrac{1}{s^{k+1}}$.
$$
\begin{align*}
\lim_{t\to\infty}e(t) &= \lim_{s\to0} s\cdot U(S)\left[\frac{K_d^2}{K_d+G(S)}\right] =\\
&= \lim_{s\to0} s \frac{1}{s^{k+1}} \frac{K_d^2}{K_d+G(S)} = \\
&= \lim_{s\to0}  \frac{1}{s^k} \frac{K_d^2}{K_d+G(S)} = 0
\end{align*}
$$
A questo punto si può separare la funzione della catena diretta $G(S)$ nella parte del processo $P(S)$ e nella parte del controllore $C(S)$. Non si può intervenire sul processo ma si può agire sul controllore per mandare l'errore in uscita a $0$. 

>[!note] Ricorda
Per fare questo bisogna comunque considerare la stabilità e la causalità del sistema. Il grado del denominatore deve rimanere uguale o superiore a quello del nominatore.

Quindi si possono separare gli integratori dal resto della catena diretta, che corrispondono al numero dei poli nell'origine.
$$
G(S) = \frac{G'(S)}{s^h}
$$
Ponendo $G'(0) = K_G$, si ottiene la relazione tra l'errore in uscita e i gradi del polinomio.
$$
\begin{align*}
\lim_{t\to\infty}e(t) &= \lim_{s\to0}  \frac{1}{s^k} \frac{K_d^2}{K_d+\frac{G'(S)}{s^h}} =\\
&= \lim_{s\to0}  \frac{K^2_d}{s^h K_d + G'(S)}s^{h-k} = 0
\end{align*}
$$

- $h>k \to e(\infty) = 0$
- $h<k \to e(\infty) = \infty$
- $h=k \to e(\infty)$ dipende dal numero dei poli del controllore nell'origine ($h$):
	- se $h=0 \to \dfrac{K_d^2}{K_d+K_G}$
	- se $h \ge 0 \to \dfrac{K^2_d}{K_G}$

Quindi il valore di $h$ definisce il tipo del sistema di controllo utilizzato, che equivale al numero di integratori in catena diretta.
$$
\begin{array}{r|ccc}
h \\ k\ \ \ \ \ \
& 0 & 1 & 2 \\ 
  \hline
0\ \ & \frac{K^2_d}{K_d+K_G}  &  0 & 0 \\ 
1\ \ & \infty & \frac{K^2_d}{K_G} & 0 \\ 
2\ \ & \infty & \infty  & \frac{K^2_d}{K_G} \\ 
\end{array} 
$$
In definitiva, per annullare l'errore e avere l'uscita desiderata bisogna avere in catena diretta più poli nell'origine rispetto all'ingresso.