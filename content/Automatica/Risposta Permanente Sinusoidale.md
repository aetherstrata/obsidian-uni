---
tags:
  - controllo
  - sistema
  - proprietà
---
Dato un [[Sistema]] asintoticamente stabile, quindi con tutti i poli a parte reale negativa, si applica in ingresso un segnale sinusoidale.
$$
u(t) = \sin(\omega t) \quad\longrightarrow\quad \mathcal{L}\{u\}=\frac{\omega}{s^2+\omega^2}
$$
Analizzando il comportamento del sistema, si ottiene che, esaurite le dinamiche transitorie, in uscita si avrà un'altro segnale sinusoidale con la stessa frequenza.
$$
\begin{gather*}
u(t) \longrightarrow \boxed{\vphantom{\int}\quad g(t)\quad} \longrightarrow y(t) \\\\
Y(S) = G(S)U(S) = G(S)\,\frac{\omega}{s^2+\omega^2}
\end{gather*}
$$
Questa espressione può essere scomposta in fratti semplici.
$$
Y(S)=\underbrace{\frac{R_1}{s-p_1}+\frac{R_2}{s-p_2}+\frac{R_3}{s-p_3}+\dots+\frac{R_i}{s-p_i}}_{sistema}+\underbrace{\frac{R}{s-j\omega}+\frac{R^{*}}{s+j\omega}}_{ingresso}
$$
Nella prima parte sono evidenziate le dinamiche del sistema che, essendo asintoticamente stabile, tendono a $0$ dopo un certo lasso di tempo. Una volta esaurite, rimarrà soltanto la risposta a regime permanente, composta dalle dinamiche complesse coniugate dell'ingresso, creando un segnale di uscita oscillatorio.

Il valore di $R$ può essere ricavato dal teorema dei residui.
$$
\begin{align*}
Res(Y,j\omega) &= \lim_{s\to j\omega} (s-j\omega)\,Y(S) =\\
&= \lim_{s\to j\omega} (s-j\omega)\,G(S)\,\frac{\omega}{s^2+\omega^2} =\\
&= \lim_{s\to j\omega} (s-j\omega)\,G(S)\,\frac{\omega}{(s-j\omega)(s+j\omega)} =\\
&= \lim_{s\to j\omega} G(S)\,\frac{\omega}{s+j\omega} =\\
&= \frac{G(j\omega)}{2j}
\end{align*}
$$
Essendo il suo coniugato, da questa espressione si può ricavare anche $R^*$.
$$
R^*=\frac{G^*(j\omega)}{-2j}
$$
Da questa scomposizione si può anche calcolare l'antitrasformata.
$$
\mathcal{L^{-1}} \left\{\frac{R}{s-j\omega}\right\} = Re^{j\omega t} \quad;\quad \mathcal{L^{-1}} \left\{\frac{R^*}{s+j\omega}\right\} = R^*e^{-j\omega t} 
$$
Sapendo che un numero complesso è anche esprimibile in forma esponenziale, allora l'uscita a regime permanente si può riscrivere come
$$
\begin{align*}
y_p(t) &= Re^{j\omega t} + R^*e^{-j\omega t} =\\
&= \frac{G(j\omega)}{2j}e^{j\omega t} + \frac{G^*(j\omega)}{-2j}e^{-j\omega t} = \\
&= \frac{|G(j\omega)|e^{j\varphi}}{2j}e^{j\omega t} + \frac{|G(j\omega)|e^{-j\varphi}}{-2j}e^{-j\omega t} = \\
&= |G(j\omega)|\frac{}{2j}
\end{align*}
$$
