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
Y(S)=\underbrace{\frac{R}{s-p_1}+\frac{R}{s-p_2}+\frac{R}{s-p_3}+\frac{R}{s-p_4}}_{sistema}+\underbrace{\frac{R}{s-j\omega}+\frac{R^{*}}{s+j\omega}}_{ingresso}
$$