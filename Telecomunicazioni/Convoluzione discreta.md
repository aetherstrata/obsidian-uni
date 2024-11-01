---
tags:
  - operatore
  - matematica
---
La convoluzione discreta è la controparte discreta della [[Convoluzione|convoluzione integrale]] che opera sull'insieme dei numeri interi $\mathbb{Z}$.
$$z[n] = x[n]*y[n] = \sum_{k=-\infty}^{+\infty}x[k]\cdot y[n-k]$$
La variabile dio uscita è $n$ e il risultato sarà un nuovo segnale tempo-discreto $z[n]$.

## Esercizi

### Convoluzione tra due rettangoli discreti

Dati due segnali discreti $x[n]$ e $y[n]$, calcolare la loro convoluzione.
$$\begin{gather}
x[n] = \sum_{k=1}^{5}\ 2\ \delta[n-k] & ; & y[n] = \sum_{k=3}^{6}\ 3\ \delta[n-k]
\end{gather}$$
>[!info] Grafici dei segnali
![[convoluzione-discreta-es-2.png]]
>$$x[n] = \sum_{k=1}^{5}\ 2\ \delta[n-k]$$
>---
![[convoluzione-discreta-es-1.png]]
 $$y[n] = \sum_{k=3}^{6}\ 3\ \delta[n-k]$$

 La convoluzione di questi segnali vale
$$z[n] = \left\{\begin{matrix*}
0 & \longrightarrow & n \le 3 \\
6(n-3) & \longrightarrow & 3 \le n \le 7 \\
24 & \longrightarrow & 7 \le n \le 8 \\
6(12-n) & \longrightarrow & 8 \le n \le 12 \\
0 & \longrightarrow & n \ge 12
\end{matrix*}\right.$$

### Convoluzione tra coseno unilatero e due [[Delta di Dirac#Delta di Kroneker|impulsi]]

Dati due segnali $x[n] = cos(\pi n)u[n]$ e $y[n] = \delta[n]+\delta[n-1]$, calcolare la loro convoluzione.

>[!info] Grafici dei segnali
>![[convoluzione-discreta-es-3.png]]
>$$x[n] = cos(\pi n)u[n]$$
>---
>![[convoluzione-discreta-es-4.png]]
>$$y[n] = \delta[n]+\delta[n-1]$$

>[!summary] Scorrimento della finestra
>Lo scorrimento della finestra $y[n]$ sui campioni del coseno ha sempre risultato nullo (tranne  nell'origine) perché i campioni nella finestra hanno uguale valore e segno opposto.

La convoluzione di questi segnali vale
$$z[n]=\left\{\begin{matrix*}
0 & \longrightarrow & n < 0\\
1 & \longrightarrow & n = 0\\
0 & \longrightarrow & n > 0
\end{matrix*}\right.\quad=\ \delta[n]$$
### Convoluzione tra [[Segnale Gradino#Gradino tempo-discreto|gradino]] e [[Segnale Esponenziale#Esponenziale tempo-discreto|esponenziale]]

Dati i segnali $x[n] = a^nu[n]$ e $y[n]=u[n]$, con $a\in\mathbb{R} < 1$ , calcolare la loro convoluzione.
$$z[n] = x[n]*y[n] = \sum_{k=-\infty}^{+\infty}a^ku[k]\cdot u[n-k]$$
>[!info] Grafici dei segnali
>![[convoluzione-discreta-es-5.png]]
>$$x[n]=a^nu[n]$$
>---
>![[convoluzione-discreta-es-6.png]]
>$$y[n]=u[n]$$

Per $n<0$ entrambi i segnali sono nulli, quindi si può riscrivere la convoluzione con un intervallo minore.
$$z[n] = \sum_{k=-\infty}^{+\infty}a^ku[k]\cdot u[n-k] = \sum_{k=0}^{+\infty}a^k\cdot u[n-k]$$
Il gradino $u[n-k]$ ha valore sono quando $k\le n$, quindi si può ulteriormente restringere l'intervallo della sommatoria da $\infty$ a $n$.
$$z[n] = \sum_{k=0}^{+\infty}a^k\cdot u[n-k] = \sum_{k=0}^{n}a^k = \frac{1-a^n}{1-a}$$
La convoluzione dei due segnali vale
$$z[n]=\left\{\begin{matrix*}
0 & \longrightarrow & n < 0 \\
\dfrac{1-a^n}{1-a}  & \longrightarrow & n \ge 0 \\
\end{matrix*}\right.$$
