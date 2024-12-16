---
tags:
  - modulazione
  - segnale
  - digitale
---
La modulazione di ampiezza in quadratura, anche detta **QAM**, trasmette il segnale in ingresso distribuendolo su due portanti alla stessa frequenza ma sfasate di $90^\circ$, un coseno e un seno, e modulando le loro ampiezze.

Questo metodo permette di trasmettere due segnali sulla stessa frequenza di banda grazie al fatto che sono ortogonali tra loro.
## Descrizione
Il segnale è diviso in due componenti $x_1(t)$ e $x_2(t)$.

Prima di essere trasmesse, le componenti sono moltiplicate per l'oscillatore locale, che trasla il segnale sulla frequenza portante e crea lo sfasamento.
$$
z(t)=x_1(t)\cos(2\pi f_0t)+x_2(t)\sin(2\pi f_0t)
$$
Per demodulare il segnale ricevuto, si applica lo stesso procedimento della trasmissione e si aggiunge un [[Filtro]] passa-basso in cascata.
$$
\begin{align*}
y_1(t)&=2\,z(t)\cos(2\pi f_0t)=\\
&=2\,x_1(t)\cos^2(2\pi f_0t)+2\,x_2(t)\sin(2\pi f_0t)\cos(2\pi f_0t)=\\
&=x_1(t)[1+\cos(4\pi f_0t)]+x_2(t)\sin(4\pi f_0t)=\\
&=x_1(t)+x_1(t)\cos(4\pi f_0t)+x_2(t)\sin(4\pi f_0t)=\\
\boxed{\text{LPF}}\quad&=x_1(t)\\\\
\hline \\
y_2(t)&=2\,z(t)\sin(2\pi f_0t)=\\
&=2\,x_1(t)\cos(2\pi f_0t)\sin(2\pi f_0t)+2\,x_2(t)\sin^2(2\pi f_0t)=\\
&=x_1(t)\sin(4\pi f_0t)+x_2(t)[1-\cos(4\pi f_0t)]=\\
&=x_1(t)\sin(4\pi f_0t)+x_2(t)-x_2(t)\cos(4\pi f_0t)=\\
\boxed{\text{LPF}}\quad&=x_2(t)
\end{align*}
$$
Infine, le due componenti sono riunite ed il segnale originare è ricostruito.
## Costellazioni
Nel caso di modulazione di un segnale digitale, è utile rappresentare i vari possibili stati in un diagramma di costellazione.

>[!Grafico costellazioni]
>![[Pasted image 20241216153858.png|centre|bg-white]]
>In questo diagramma l'asse delle ascisse corrisponde alla componente in fase $I(t)$ mentre l'asse delle ordinate corrisponde alla componente in quadratura $Q(t)$. 
>$$
>\begin{align*}
>I(t)&=\Re\set{e^{i2\pi f_0t}}=\cos(2\pi f_0t)\\
>Q(t)&=\Im\set{e^{i2\pi f_0t}}=\sin(2\pi f_0t)
>\end{align*}
>$$

Dato che il segnale è diviso in due componenti, allora ci saranno anche due simboli $a_n$ e $b_n$.
$$
\begin{align*}
I(t)&=a_n\cos(2\pi f_0t)\\
Q(t)&=b_n\sin(2\pi f_0t)
\end{align*}
$$
Per conoscere quale valore sta trasmettendo il segnale bisogna, quindi, conoscere simultaneamente sia $a_n$ che $b_n$.
## Energia per simbolo
Dato il seguente segnale $z(t)$ opportunamente modulato si può ricavare la sua energia per simbolo prendendo un solo simbolo dalla sommatoria.
$$
z(t)=a_n\,g(t)\cos(2\pi f_0t)+b_n\,g(t)\sin(2\pi f_0t)
$$
L'energia può essere calcolata con il [[Teorema di Parseval]].
$$
\begin{align*}
\end{align*}
$$