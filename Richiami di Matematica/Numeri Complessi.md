---
tags:
  - matematica
---
### Formula di Eulero

> [!summary] Per ogni numero reale $\large x$ si ha $\large e^{ix}=\cos x + i \sin x$ .

Si tratta di una relazione usata per rappresentare i numeri complessi in coordinate polari, e che permette la definizione del logaritmo per argomenti complessi. La rappresentazione della funzione $e^{ix}$ nel piano complesso è un cerchio unitario, e $x$ è l'angolo che un segmento che collega l'origine a un punto del cerchio unitario forma con l'asse reale positivo, misurato in senso antiorario e in radianti.

![[formula_eulero.svg|center|500]]

>[!example] Quadrato del coseno
>Data la formula di Eulero per il coseno,
>$$\large x(t) = A\cos(2\pi f_0 t) = A\left(\dfrac{e^{i2\pi f_0t}+e^{-i2\pi f_0t}}{2}\right)$$
>e ricavando quindi che
>$$\large 2\cos(2\pi f_0 t) = e^{i2\pi f_0t}+e^{-i2\pi f_0t}$$
>si può ottenere la formula per il quadrato del coseno.
>$$\large x^2(t) = \dfrac{A^2}{4}\left(e^{i2\pi f_0t}+e^{-i2\pi f_0t}\right)^2 = \dfrac{A^2}{4}(2+e^{i4\pi f_0t}+e^{-i4\pi f_0t}) =$$
>$$\large = \dfrac{A^2}{4}\left(2+2\cos(4\pi f_0t)\right) = \dfrac{A^2}{2}\left(1+\cos(4\pi f_0t)\right)$$
