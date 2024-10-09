---
tags:
  - proprietà
  - segnale
---
Tutti i processi fisici sono mediati attraverso **trasferimenti di energia**. Un sistema fisico reale risponde all'energia fornita da un'eccitazione o sollecitazione applicata ad esso. I segnali rappresentano, appunto, tali eccitazioni o sollecitazioni ed hanno, pertanto, un **contenuto energetico**.

Il contenuto energetico di un segnale dipende dalla sua natura fisica. Ma per poter trattare un segnale in maniera generica ed astratta si definiscono le quantità di **Potenza ed Energia di un segnale**. Tali quantità sono differenti dalla Potenza ed Energia fisiche ma sono ad esse **proporzionali**.
## Segnali analogici
### Energia di un segnale analogico
$$E_x = \lim\limits_{\Delta T \to\infty} \int_{-\Delta T/2}^{\Delta T/2} |x(t)|^2\ dt = \int_{-\infty}^{\infty}|x(t)|^2\ dt$$
> [!summary] Considerazioni
> - Può valere al minimo $0$ in quanto l'integrando è sempre positivo
> - Può valere $0$ solo quando il segnale è nullo
> - Non esistono nella realtà segnali a energia infinita
### Potenza di un segnale analogico
$$P_x = \lim\limits_{\Delta T \to\infty}\frac{1}{\Delta T} \int_{-\Delta T/2}^{\Delta T/2} |x(t)|^2\ dt$$
>[!info] Potenza di un segnale [[Funzione Periodica|periodico]]
>Sia $T$ il periodo del segnale, allora il limite potrà essere riscritto come
>$$P_x = \lim\limits_{n \to\infty}\frac{1}{n T} \int_{-nT/2}^{nT/2} |x(t)|^2\ dt = \lim\limits_{n \to\infty}\frac{1}{n T}\ n\int_{-T_0/2}^{T/2} |x(t)|^2\ dt = $$
>$$= \frac{1}{T}\ \int_{-T/2}^{T/2} |x(t)|^2\ dt$$
## Segnali tempo-discreti
### Energia di un segnale discreto

$$\sum_{n=-\infty}^{\infty}\Big|x[n]\Big|^2$$
### Potenza di un segnale discreto
$$\lim\limits_{N\to\infty} \frac{1}{2N+1}\sum_{n=-N}^{N}\Big|x[n]\Big|^2$$
>[!info] Potenza di un segnale [[Funzione Periodica|periodico]]
>$$\frac{1}{T}\sum_{n=0}^{T-1}\Big|x[n]\Big|^2$$
## Esercizi

>[!example] Una [[Segnale di Gauss|gaussiana]]
>Si definisce $t'=\sqrt{2a}t$ e si calcola l'integrale di Gauss.
>$$E_x = \int_{-\infty}^{\infty} |Ae^{-at^2}|^2\ dt = A^2 \int_{-\infty}^{\infty} \dfrac{e^{-t'^2}}{\sqrt{2a}}\ dt' = A^2 \frac{\pi}{\sqrt{2a}}$$
>Dato che ha un valore non nullo, allora è un segnale di energia e ha potenza nulla.

> [!example] Una [[Segnale Rampa|rampa]]
> Dato un segnale rampa $x(t) = t\cdot u(t)$ si calcola l'integrale di potenza.
> $$P_x= \lim\limits_{\Delta T \to\infty} \frac{1}{\Delta T} \int_{-\Delta T/2}^{\Delta T/2} |t\cdot u(t)|^2\ dt = \lim\limits_{\Delta T \to\infty}\frac{1}{\Delta T} \int_{0}^{\Delta T/2} t^2\ dt =$$
> $$= \lim\limits_{\Delta T \to\infty} \frac{1}{\Delta T} \frac{t^3}{3} \Big|_0^{\Delta T/2} = \lim\limits_{\Delta T \to\infty} \frac{\Delta T^2}{24} = \infty $$
> Il risultato non è un valore finito, quindi si passa a calcolare l'integrale di energia.
> $$E_x = \int_{0}^{\infty}t^2\ dt = \infty$$
> Anche l'energia del segnale è infinita, quindi non è un segnale di potenza né di energia.

>[!example] Esponenziale complesso
>Dato un segnale esponenziale complesso $x(t) = -Ae^{i2\pi f_0 t}$ si calcola l'integrale di energia.
>
>Per la [[Numeri Complessi#Formula di Eulero|formula di Eulero]], l'esponenziale può essere trasformato in $cos(2\pi f_0 t) + i\ sin(2\pi f_0 t)$.
>$$E_x = \int_{-\infty}^{\infty} A^2|cos(2\pi f_0 t) + i\ sin(2\pi f_0 t)|^2\ dt$$
>La somma di seno e coseno al quadrato è sempre uguale a $1$ quindi si può eliminare.
>$$E_x = \int_{-\infty}^{\infty} A^2\ dt = \infty$$
> Il segnale non è un segnale di energia. Si calcola quindi l'integrale di potenza.
> $$P_x= \lim\limits_{\Delta T \to\infty} \frac{1}{\Delta T} \int_{-\Delta T/2}^{\Delta T/2} A^2\ dt = \lim\limits_{\Delta T \to\infty} \frac{1}{\Delta T}\ A^2\ t \Big|_{-\Delta T/2}^{\Delta T/2} = \lim\limits_{\Delta T \to\infty} \frac{1}{\Delta T}\ \frac{\Delta T}{2}\ A^2 = \frac{A^2}{2}$$
> Il risultato è un valore finito, quindi questo segnale è un segnale di potenza.

>[!example] Esponenziale unilatero
>$$E_x = \int_{-\infty}^{\infty}|e^{-\alpha t}u(t)|^2\ dt = \int_{0}^{\infty} e^{-2\alpha t}\ dt = \frac{e^{-2\alpha t}}{-2\alpha t}\Big|_0^{\infty} = \frac{1}{2\alpha}$$
>Il risultato è un valore finito, quindi l'esponenziale unilatero è un segnale di energia.

>[!example] Coseno
>La funzione coseno è una funzione periodica quindi si può usare la forma alternativa dell'integrale di potenza, sapendo che il periodo vale $\frac{1}{f_0}$.
>$$P_x = \frac{1}{T}\ \int_{-T/2}^{T/2} |A\ cos(2\pi f_0t)|^2\ dt = f_0A^2\int_{-1/2f_0}^{1/2f_0} cos^2(2\pi f_0 t)\ dt =$$
>Usando la formula di duplicazione si ottengono questi due integrali. Visto che l'intervallo di integrazione è simmetrico, l'integrale del coseno si annulla e rimane solo quello di $1/2$.
>$$= f_0A^2\int_{-1/2f_0}^{1/2f_0} \frac{cos(4\pi f_0 t)+1}{2}\ dt = f_0 \frac{1}{2} \frac{1}{f_0}A^2 = \frac{A^2}{2} $$
>Il risultato è un valore finito, quindi la funzione coseno è un segnale di potenza.
