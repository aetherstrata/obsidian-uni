---
tags:
  - sistema
  - analisi
  - controllo
---
Il diagramma di Bode è usato per studiare il comportamento di modulo e fase della funzione di trasferimento al variare della frequenza dell'ingresso sinusoidale. Questo diagramma è quindi usato nell'analisi dei sistemi nel [[Regime Permanente Sinusoidale]].

Per poter essere utilizzato, la funzione di trasferimento deve essere prima normalizzata per poter ricavare il guadagno generalizzato $K_G$.
$$
G(S) = \frac{K_G\,s^h\,(s\tau_1+1)(s\tau_2+1)\dots\left(\frac{s^2}{\omega^2_n}+\frac{2\zeta}{\omega_n}s+1\right)\dots}{(s\tau_3+1)(s\tau_4+1)\dots\left(\frac{s^2}{\omega^2_n}+\frac{2\zeta}{\omega_n}s+1\right)\dots}
$$
In questa espressione tutti i termini sono moltiplicati tra loro e questo semplifica il calcolo del modulo e fase di $G(j\omega)$. 

La fase di una divisione diventa una sottrazione di fasi e la fase di un prodotto diventa una somma di fasi.
$$
\begin{align*}
\angle G(j\omega) &= \angle K_G+\angle (j\omega)^h +\angle(j\omega\tau_1+1)+\angle(j\omega\tau_2+1)+\dots+\\
&-\angle(j\omega\tau_3+1)+\angle(j\omega\tau_4+1)-\dots
\end{align*}
$$
L'andamento complessivo della fase è pari alla somma dell'andamento dei singoli termini.

Il modulo di un prodotto è il prodotto dei moduli, il che è comunque un'operazione complicata. Per questo si usa una scala logaritmica per trasformare questi prodotti in delle somme.
$$
20\log_{10}|a\cdot b| = 20\log_{10}|a| + 20\log_{10}|b|
$$
### Guadagno generalizzato
$$
K_G
$$
#### Modulo
$$
|Kg| = 20\log_{10}|K_G|
$$
#### Fase
$$
\angle Kg =
\begin{cases}
\hphantom{-18}0 & \text{per } K_G > 0\\
-180 & \text{per } K_G < 0\\
\end{cases}
$$
### Radici nell'origine
$$
s^h
$$
#### Modulo
$$
|s^h|\overset{s=j\omega}{\longrightarrow} (|j\omega|)^h \overset{\text{dB}}{\longrightarrow} 20\log_{10}(\omega)^h = 20h\log_{10}\omega
$$
Questo studio è limitato a valori di $\omega > 0$ dato che il caso $\omega < 0$ è lo stesso grafico ma ribaltato.

>[!info] Scala dei grafici
>Graficare questi valori in un piano cartesiano in scala lineare rende difficoltoso applicare le operazioni definite sopra per modulo e fase.
>
>![[1747410415.png]]
>$$
>\text{Grafico in scala lineare}
>$$
>---
>In scala logaritmica queste curve diventano rette e si può operare su di esse con molta più facilità.
>
>![[1747410579.png]]
>$$
>\text{Grafico in scala logaritmica}
>$$
#### Fase
Dato che la fase di $j\omega$ vale $90^{\circ}$ e la fase di un prodotto è la somma delle fasi, allora la fase del termine $(j\omega)^h$ è un multiplo di $90^{\circ}$.
$$
\begin{gather*}
\angle s^h \overset{s=j\omega}{\longrightarrow} \angle(j\omega)^h = (j\omega)(j\omega)(j\omega)\dots(j\omega)\\\\
\angle(j\omega)^h = h\cdot 90^{\circ}
\end{gather*}
$$
### Radici binomiali
$$
s\tau+1
$$
#### Modulo
$$
\begin{gather*}
|s\tau + 1|\overset{s=j\omega}{\longrightarrow} |j\omega\tau+1| \overset{\text{dB}}{\longrightarrow} 20\log_{10}\sqrt{\omega^2\tau^2+1} = 10\log_{10}(\omega^2\tau^2+1) \\\\
10\log_{10}(\omega^2\tau^2+1) \to \left\{
\begin{matrix}
\omega\ll\frac{1}{\tau}\quad\to& 0\text{dB}\\
\omega\gg\frac{1}{\tau}\quad\to& 20\log_{10}\omega\tau = 20\log_{10}\omega + \cancel{20\log_{10}\tau}\\
\omega=\frac{1}{\tau}\ \quad\to& 20\log_{10}\sqrt{\frac{1}{\tau^ 2}\tau^2+1} = 20\log_{10}\sqrt{2}\approx3\text{dB}
\end{matrix}\right.
\end{gather*}
$$
#### Fase
$$
\angle(s\tau+1)=\angle(j\omega\tau+1)=
\begin{cases}
0^{\circ} & \omega\ll\frac{1}{\tau}\\
90^{\circ} & \omega\gg\frac{1}{\tau}\\
45^{\circ} & \omega=\frac{1}{\tau}
\end{cases}
$$
### Radici trinomiali 
$$
\frac{s^2}{\omega^2_n}+\frac{2\zeta}{\omega_n}s+1
$$
#### Modulo
$$
\begin{gather*}
\left|\frac{s^2}{\omega^2_n}+\frac{2\zeta}{\omega_n}s+1\right| \overset{s=j\omega}{\longrightarrow} \left|\frac{j^2\omega^2}{\omega^2_n}+\frac{2\zeta}{\omega_n}j\omega+1\right| \overset{\text{dB}}{\longrightarrow} 20\log_{10}\sqrt{\left(1-\frac{\omega^2}{\omega_n^2}\right)^2 + \left(\frac{2\zeta\omega}{\omega_n}\right)^2} \\\\
|\centerdot| = \begin{dcases}
\omega\ll\omega_n & 0\text{dB} \\
\omega\gg\omega_n & 10\log_{10} \left[\frac{\omega^4}{\omega_n^4}+\cancel{4\zeta^2\frac{\omega^2}{\omega^2_n}}\right] = 40\log_{10}\omega-\cancel{40\log_{10}\omega_n} = 40\lambda\\
\omega=\omega_n & 10\log_{10}\left(2\zeta\cancel{\frac{\omega}{\omega_n}}\right)^2 = 20\log_{10}2\zeta\\
\end{dcases}\\\\
20\log_{10}2\zeta=\begin{dcases}
\zeta=0 & \to 20\log_{10}0 = -\infty\\
\zeta=1 & \to 20\log_{10}2 \approx 6\text{dB}
\end{dcases}
\end{gather*}
$$
Questo comportamento crea una buca o un rigonfiamento nella frequenza di risonanza, fino a diventare un asintoto per $\zeta = 0$.
#### Fase
$$
\begin{gather*}
\angle\left(\frac{s^2}{\omega^2_n}+\frac{2\zeta}{\omega_n}s+1\right) \overset{s=j\omega}{\longrightarrow} \angle\left(\frac{j^2\omega^2}{\omega^2_n}+\frac{2\zeta}{\omega_n}j\omega+1\right) = \angle\left(\frac{2\zeta}{\omega_n}j\omega+1-\frac{\omega^2}{\omega^2_n}\right) =\\\\
=\begin{dcases}
\omega\ll\omega_n & \to 0^{\circ} \\
\omega\gg\omega_n & \to 180^{\circ} \\
\omega = \omega_n & \to 90^{\circ}
\end{dcases}
\end{gather*}
$$
Il fattore di smorzamento $\zeta$ indica con quanta velocità cambia la fase. Il cambio più repentino si ha con $\zeta = 0$.