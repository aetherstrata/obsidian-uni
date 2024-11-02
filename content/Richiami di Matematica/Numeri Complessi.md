---
tags:
  - matematica
---
## Formula di Eulero

> [!summary] Per ogni numero reale $x$ si ha $e^{ix}=\cos x + i \sin x$ .

Si tratta di una relazione usata per rappresentare i numeri complessi in coordinate polari, e che permette la definizione del logaritmo per argomenti complessi. La rappresentazione della funzione $e^{ix}$ nel piano complesso è un cerchio unitario, e $x$ è l'angolo che un segmento che collega l'origine a un punto del cerchio unitario forma con l'asse reale positivo, misurato in senso antiorario e in radianti.

![[formula_eulero.svg|center|500]]
### Rappresentazione esponenziale di seno e coseno
La formula di Eulero permette di interpretare le funzioni seno e coseno come varianti della funzione esponenziale.
$$
\begin{align*}
\cos(x)&=\frac{e^{ix}+e^{-ix}}{2}\\
\sin(x)&=\frac{e^{ix}-e^{-ix}}{2i}
\end{align*}
$$
Queste formule possono anche essere usate come definizione delle funzioni trigonometriche per argomenti complessi $x$.

>[!info] Quadrato del coseno
>Data la formula di Eulero per il coseno,
>$$
>x(t) = A\cos(2\pi f_0 t) = A\left(\dfrac{e^{i2\pi f_0t}+e^{-i2\pi f_0t}}{2}\right)
>$$
>e ricavando quindi che
>$$
>2\cos(2\pi f_0 t) = e^{i2\pi f_0t}+e^{-i2\pi f_0t}
>$$
>si può ottenere la formula per il quadrato del coseno.
>$$
>\begin{align*}
>x^2(t) &= \frac{A^2}{4}\left(e^{i2\pi f_0t}+e^{-i2\pi f_0t}\right)^2 =\\
>&= \frac{A^2}{4}(2+e^{i4\pi f_0t}+e^{-i4\pi f_0t}) =\\
>&= \frac{A^2}{4}\left(2+2\cos(4\pi f_0t)\right) =\\
>&= \frac{A^2}{2}\left(1+\cos(4\pi f_0t)\right)
>\end{align*}
>$$
>Nel caso in cui $A=1$, i termini si possono semplificare e si ottiene la [[Formule Trigonometriche#Formule di duplicazione|formula di duplicazione]] del coseno inversa.
>$$
>\cos^2(t)=\frac{1}{2}+\frac{1}{2}\cos(4\pi f_0t)
>$$
