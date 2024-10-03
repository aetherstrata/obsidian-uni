La **convoluzione** è un'operazione tra due [[Segnale|segnali]].
$$\large z(t) = x(t) * y(t) = \int_{-\infty}^{\infty} x(\tau) \cdot y(t-\tau)\ d\tau$$
La variabile di uscita è $\large t$ e il risultato sarà un segnale senza punti di discontinuità.
## Proprietà
### Commutativa
$$z(t)\ =\ x(t) * y(t)\ =\ y(t) * x(t)$$
### Associativa
$$z(t)\ =\ x(t) * [y(t) * w(t)]\ =\ [x(t) * y(t)] * w(t)$$
### Distributiva
$$z(t)\ =\ x(t) * [y(t) + w(t)]\ =\ [x(t) * y(t)] + [x(t) * w(t)]$$
## Esercizi
### Convoluzione tra [[Segnale Esponenziale|esponenziale unilatero]] e [[Segnale Gradino|gradino]]

Dati due segnali $x(t) = e^{-\alpha t}u(t)$ e $y(t) = u(t)$, la loro convoluzione vale
$$\large z(t) = \int_{-\infty}^{\infty} e^{-\alpha\tau}u(\tau) \cdot u(t-\tau)\ d\tau$$
>[!info] Grafici dei segnali dentro l'integrale
>Per poter graficare queste funzioni è stato scelto arbitrariamente il parametro $\alpha = 0.5$ e $t=-2$. 
>
>![[convoluzione-es-1.png]]
>$$\large x(\tau)\ =\ e^{-0.5\tau}u(\tau)$$
>---
>![[convoluzione-es-2.png]]
>$$\large y(-2-\tau)=u(-2-\tau)$$

Si può notare che per $t<0$ i segnali non interagiscono (almeno uno di loro è nullo nell'intervallo). 
$$\begin{gather}z(t) = 0 & \text{per} & t<0\end{gather}$$
>[!info] Grafico per $\large t=2$
>![[convoluzione-es-3.png]]

Invece per $t>0$, usando le proprietà del segnale gradino, l'integrale diventa definito in un intervallo limitato e si eliminano i gradini.
$$\large \int_{0}^{t}e^{-\alpha\tau}\cdot 1\ d\tau = \left.\frac{e^{-\alpha\tau}}{-\alpha}\right|_{0}^{t}=\frac{1-e^{-\alpha t}}{\alpha}$$
Quindi si può definire il segnale convoluto a tratti.
$$z(t) = \left\{\begin{matrix*}
0 & \longrightarrow & t < 0\\
\dfrac{1-e^{-\alpha t}}{\alpha} & \longrightarrow & t \ge 0\\
\end{matrix*}\right.$$
### Convoluzione tra due [[Segnale Esponenziale|esponenziali unilateri]]

Dati due segnali $\large x(t) = e^{-\alpha t}u(t)$ e $\large y(t) = e^{-\alpha t}u(t)$, la loro convoluzione vale
$$\large z(t) = \int_{-\infty}^{\infty} e^{-\alpha\tau}u(\tau) \cdot e^{-\alpha(t-\tau)}u(t-\tau)\ d\tau$$
Come per l'esempio sopra, i due segnali non interagiscono per $t<0$.
$$\begin{gather}z(t) = 0 & \text{per} & t<0\end{gather}$$

>[!info] Grafico per $t=4$
>![[convoluzione-es-4.png]]

Anche qui l'integrale si può semplificare per valori di $t>0$ grazie alle proprietà dei gradini.
$$\large \int_{0}^{t}e^{-\alpha\tau}\cdot e^{-\alpha t}\cdot e^{\alpha\tau}\ d\tau =  \int_{0}^{t}e^{-\alpha t}\ d\tau = e^{-\alpha t}\int_{0}^{t}d\tau = te^{-\alpha t}$$
Il risultato è un nuovo segnale definito a tratti.
$$z(t) = \left\{\begin{matrix*}
0 & \longrightarrow & t < 0\\
te^{-\alpha t} & \longrightarrow & t \ge 0\\
\end{matrix*}\right.$$
### Convoluzione tra un [[Segnale Esponenziale|esponenziale unilatero]] e un [[Segnale Finestra#Finestra rettangolare|rettangolo]]

Dati due segnali $\large x(t) = e^{-\alpha t}\cdot u(t)$ e $\large y(t) = \operatorname{rect}(\frac{t-T/2}{T})$, la loro convoluzione vale
$$\large z(t) = \int_{-\infty}^{\infty} e^{-\alpha\tau}u(\tau) \cdot \operatorname{rect}\left(\frac{t-\tau-T/2}{T}\right)\ d\tau$$Mentre negli esempi prima entrambi i segnali avevano durata infinita, la finestra rettangolare ha una durata delimitata.

>[!summary] Finestre di integrazione
>Per $\large t<0$ i segnali non interagiscono quindi il segnale convoluto vale sempre 0.
>
>![[convoluzione-rect-1.png]]
>$$\begin{gather}z(t) = 0 & \text{per} & t<0\end{gather}$$
>---
>Per $\large 0<t<T$ si calcola l'integrale da 0 a $t$.
>
>![[convoluzione-rect-2.png]]
>$$\large z(t) = \int_{0}^{t} e^{-\alpha\tau}\ d\tau = \frac{1-e^{-\alpha t}}{\alpha}$$
>---
>Per $t > T$ bisogna tener conto che la finestra rettangolare ha un limite inferiore a differenza del segnale gradino.
>
>![[convoluzione-rect-3.png]]
>$$\large z(t) = \int_{t-T}^{t} e^{-\alpha\tau}\ d\tau = \left.\frac{e^{-\alpha\tau}}{-\alpha}\right|_{t-T}^{t} = \frac{e^{-\alpha t}\cdot (e^{\alpha T}-1)}{\alpha}$$

Unendo i vari casi, si ottiene questo segnale definito a tratti.
$$\large z(t)=\left\{\begin{matrix*}
0 & \longrightarrow & t < 0 \\
\dfrac{1-e^{-\alpha t}}{\alpha} & \longrightarrow & 0 \le t < T \\
\dfrac{e^{-\alpha t}\cdot (e^{\alpha T}-1)}{\alpha} & \longrightarrow & t \ge T
\end{matrix*}\right.$$
### Convoluzione tra due [[Segnale Finestra#Finestra rettangolare|rettangoli]] di uguale base

Dati due segnali $x(t)$ e $y(t)$, entrambi uguali a $\large\operatorname{rect}\left(\normalsize\dfrac{t}{T}\right)$, la loro convoluzione vale
$$\large z(t) = \int_{-\infty}^{\infty} \operatorname{rect}\left(\frac{\tau}{T}\right)\cdot\operatorname{rect}\left(\frac{t-\tau}{T}\right) \ d\tau$$
>[!summary] Finestre di integrazione
>Ricordando che la finestra rettangolare ha ampiezza $T$, è non nulla solo nell'area attorno all'origine.
>$$-\frac{T}{2}\le\tau\le\frac{T}{2}$$ 
>Per valori di $t<-T$ almeno uno dei segnali è nullo e, di conseguenza, anche il segnale risultante.
>
>![[convoluzione-tri-1.png]]
>
Questo vale anche per valori di $t>T$.
> 
![[convoluzione-tri-4.png]]
>$$z(t) = \left\{\begin{matrix*}
>0 & \longrightarrow & t < -T\\
>\vdots \\
>0 & \longrightarrow & t > T
>\end{matrix*}\right.$$
>---
>Per $-T<t<0$ la finestra sta traslando dentro la finestra fissa nell'origine e l'area aumenta.
>
![[convoluzione-tri-2.png]]
>$$\int_{-T/2}^{t+T/2}\ d\tau = \tau\ \Bigg|_{-T/2}^{t+T/2}=t+T$$
>---
>Per $0<t<T$ la finestra sta traslando fuori la finestra fissa nell'origine e l'area diminuisce.
>
![[convoluzione-tri-3.png]]
>$$\int_{t-T/2}^{T/2}\ d\tau = \tau\ \Bigg|_{t-T/2}^{T/2}=-t+T$$

Unendo tutti i casi di $t$ si ottiene il segnale convoluto.
$$z(t)=\left\{\begin{matrix*}
0 & \longrightarrow & t < -T\\
T+t & \longrightarrow & -T \le t \le 0\\
T-t & \longrightarrow & 0 \le t \le T\\
0 & \longrightarrow & t > T
\end{matrix*}\right.$$
Questo segnale è uguale a una [[Segnale Finestra|finestra triangolare]] di base doppia e altezza uguale al periodo dei rettangoli.
$$z(t)\ =\ \left\{\begin{matrix*}
T-\left|t\right| & \longrightarrow & -T<t<T\\
0 & \longrightarrow & altrove
\end{matrix*}\right.$$
>[!info] Segnale convoluto
![[convoluzione-tri-5.png]]

### Convoluzione di due [[Segnale Finestra#Finestra rettangolare|rettangoli]] di base diversa

Dati due segnali $x(t) = \large\operatorname{rect}\left(\normalsize\dfrac{t}{T_1}\right)$ e $y(t) = \large\operatorname{rect}\left(\normalsize\dfrac{t}{T_2}\right)$ , con $T_1 > T_2$, la loro convoluzione vale
$$\large z(t) = \int_{-\infty}^{\infty} \operatorname{rect}\left(\frac{\tau}{T_1}\right)\cdot\operatorname{rect}\left(\frac{t-\tau}{T_2}\right) \ d\tau$$Questa convoluzione è simile a quella sopra, ma con l'aggiunta di un altro caso. Infatti, dato che le basi sono diverse, esiste un periodo in cui uno dei rettangoli è completamente sovrapposto all'altro e non ci sarà variazione dell'area.

>[!summary] Finestre di integrazione
>Per valori di $t<-\dfrac{T_1+T_2}{2}$ almeno uno dei segnali è nullo e, di conseguenza, anche il segnale risultante.
![[convoluzione-trapezio-1.png]]
>
>Questo vale anche per valori di $t>\dfrac{T_1+T_2}{2}$.
>
>![[convoluzione-trapezio-5.png]]
>$$z(t) = \left\{\begin{matrix*}
>0 & \longrightarrow & t < -\dfrac{T_1+T_2}{2}\\
>\vdots \\
>0 & \longrightarrow & t > \dfrac{T_1+T_2}{2}
>\end{matrix*}\right.$$
>---
>Finché $t<-\dfrac{T_1-T_2}{2}$ l'area della sovrapposizione aumenta linearmente.
>
![[convoluzione-trapezio-2.png]]
>$$\large\int_{-T_1/2}^{t+T_2/2}d\tau = \normalsize\frac{T_1+T_2}{2}+t$$
>---
>Da $-\dfrac{T_1-T_2}{2}$ a $\dfrac{T_1-T_2}{2}$ il rettangolo piú piccolo è pienamente sovrapposto, infatti l'integrale è costante.
>
![[convoluzione-trapezio-3.png]]
>$$\large\int_{t-T_2/2}^{t+T_2/2}d\tau = T_2$$
>---
>Quando $t>\dfrac{T_1-T_2}{2}$ la finestra comincia a scorrere fuori e l'area della sovrapposizione diminuisce linearmente.
>
![[convoluzione-trapezio-4.png]]
>$$\large\int_{t-T_2/2}^{T_1/2}d\tau = \normalsize\frac{T_1+T_2}{2}-t$$

Unendo tutti i casi di $t$ si ottiene il segnale convoluto, che ha la forma di un trapezio.
$$z(t)=\left\{\begin{matrix*}[r,l]
0 & \longrightarrow & t < -\dfrac{T_1+T_2}{2}\\
\dfrac{T_1+T_2}{2}+t & \longrightarrow & -\dfrac{T_1+T_2}{2} \le t \le -\dfrac{T_1-T_2}{2}\\
T_2 & \longrightarrow & -\dfrac{T_1-T_2}{2} \le t \le \dfrac{T_1-T_2}{2}\\
\dfrac{T_1+T_2}{2}-t & \longrightarrow & \dfrac{T_1-T_2}{2} \le t \le \dfrac{T_1+T_2}{2}\\
0 & \longrightarrow & t > \dfrac{T_1+T_2}{2}
\end{matrix*}\right.$$
Si può notare come, ponendo $T_1$ e $T_2$ uguali, la parte centrale scompare e si forma un triangolo, esattamente come [[#Convoluzione tra due Segnale Finestra Finestra rettangolare rettangoli di uguale base|l'esercizio sopra]].

>[!info] Grafico del segnale convoluto
>![[convoluzione-trapezio-6.png]]

### Convoluzione tra due [[Segnale di Gauss|gaussiane]]

Dati due segnali gaussiani $\large x(t) = e^{-\alpha_1 t^2}$ e $\large y(t) = e^{-\alpha_2 t^2}$ , la loro convoluzione vale
$$\begin{gather}
\large z(t) = \int_{-\infty}^{\infty} e^{\huge-\alpha_1\tau^2}\cdot e^{\huge-\alpha_2(t-\tau)^2}\ d\tau = e^{\huge-\alpha_2 t^2} \int_{-\infty}^{\infty} e^{\huge-(\alpha_1+\alpha_2)\tau^2}\cdot e^{\huge-2\alpha_2 t\tau}\ d\tau = \\ 
\large = e^{\huge -\alpha_2 t^2} \int_{-\infty}^{\infty} e^{\huge-(\alpha_1 + \alpha_2)\left[\tau^2\ -\ 2\normalsize\ \dfrac{\alpha_2 t \tau}{\alpha_1+\alpha_2}\ +\ \dfrac{\alpha_2^2t^2}{(\alpha_1+\alpha_2)^2}\ -\ \dfrac{\alpha_2^2t^2}{(\alpha_1+\alpha_2)^2}\right]}\ d\tau = \\
\large = e^{\huge -\alpha_2 t^2} \int_{-\infty}^{\infty} e^{\huge-(\alpha_1 + \alpha_2)\left[\left(\tau\ +\ \normalsize\dfrac{\alpha_2t} {\alpha_1+\alpha_2}\right)^2\large\ -\ \normalsize\dfrac{\alpha_2^2t^2}{(\alpha_1+\alpha_ 2)^2}\right]}\ d\tau = \\
\large = e^{\huge-\alpha_2t^2}\cdot e^{\normalsize\dfrac{\alpha_2^2t^2}{\alpha_1+\alpha_2}} \int_{-\infty}^{\infty} e^{\huge-(\alpha_1+\alpha_2)\left(\tau\ +\ \normalsize\dfrac{\alpha_2t}{\alpha_1+\alpha_2}\right)^2}\ d\tau =
\end{gather}$$
>[!important] Cambio di variabile
A questo punto si fa un cambio di variabile $\tau'$ sull'esponente di $e$ per ottiene un integrale di Gauss.
$$\large \tau' = \sqrt{\alpha_1+\alpha_2}\left(\tau + \frac{\alpha_2t}{\alpha_1+\alpha_2}\right)$$

$$\begin{gather}
\large = e^{-\normalsize\dfrac{\alpha_1\alpha_2}{\alpha_1+\alpha_2}\huge t^2}  \int_{-\infty}^{\infty} e^{\huge\tau'^2}\ \normalsize\dfrac{d\tau'}{\sqrt{\alpha_1+\alpha_2}} = \\ \\
\large = \sqrt{\frac{\huge\pi}{\alpha_1+\alpha_2}}e^{\huge-\normalsize\dfrac{\alpha_1\alpha_2}{\alpha_1+\alpha_2}\huge t^2}
\end{gather}$$
Il risultato sarà un altro segnale gaussiano.

>[!info] Forma normalizzata
>Per fare la convoluzione tra due gaussiane espresse in forma normalizzata allora il risultato sarà un'altra gaussiana che avrà la somma delle varianze dei segnali di partenza.
>$$\large x(t) = \frac{1}{\sqrt{2\pi}\sigma_1}e^{\huge-\normalsize\dfrac{t^2}{2\sigma_1^2}} \qquad \large y(t) = \frac{1}{\sqrt{2\pi}\sigma_2}e^{\huge-\normalsize\dfrac{t^2}{2\sigma_2^2}}$$
>$$\large z(t) = x(t) * y(t) = \normalsize\dfrac{1}{\sqrt{2\pi}\sqrt{\sigma_1^2+\sigma_2^2}}\large e^{\huge - \normalsize\dfrac{t^2}{2(\sigma_1^2+\sigma_2^2)}}$$
