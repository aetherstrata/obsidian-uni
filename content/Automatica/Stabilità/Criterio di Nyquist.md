---
tags:
  - analisi
  - controllo
  - sistema
  - stabilità
---
Il criterio di Nyquist sulla stabilità dei sistemi a ciclo chiuso si basa sul [[Teorema dell'indicatore logaritmico]].

Questo criterio si applica sulle funzioni di anello, quindi dei sistemi in cui la controreazione è unitaria. Se ci dovesse essere un induttore in controreazione, va spostato in catena diretta, dopo il sommatore.
$$
1 + F(S)
$$
Per applicare questo criterio va scelta una curva tale che, partendo dall'asse immaginario, inglobi tutti i poli del semipiano destro, cioè a parte reale positiva. Questa curva è chiamata _Curva di Nyquist_.

Le rotazioni della funzione $1+F(S)$ attorno all'origine saranno quindi uguali al numero di zeri a parte reale positiva meno il numero dei poli a parte reale positiva.
$$
R_{1+F(S),0}=\#zeri_{prp}(1+F(S))-\#poli_{prp}(1+F(S))
$$
Per arrivare ad avere una rappresentazione della funzione a ciclo chiuso si possono applicare alcune sostituzioni algebriche.
$$
\begin{gather*}
F(S)=\frac{N(S)}{D(S)}\qquad\longrightarrow\qquad 1+F(S)=1+\frac{N(S)}{D(S)}=\frac{D(S)+N(S)}{D(S)}\\
W(S) = \frac{F(S)}{1+F(S)} = \frac{N(S)}{D(S)}\frac{D(S)}{D(S)+N(S)}=\frac{N(S)}{D(S)+N(S)}
\end{gather*}
$$
Da queste funzioni si possono ricavare alcune relazioni:
- I poli di $F(S)$ sono gli stessi di $1+F(S)$
- I poli di $W(S)$ coincidono con gli zeri di $1+F(S)$

Con queste relazioni si può riscrivere la formula delle rotazioni.
$$
R_{1+F(S),0}=\#poli_{prp}(W(S))-\#poli_{prp}(F(S))
$$
È condizione necessaria e sufficiente per avere nessun polo a parte reale positiva nella funzione a ciclo chiuso che le rotazioni di $1+F(S)$ attorno all'origine siano uguali all'opposto del numero dei poli della funzione $F(S)$.
$$
R_{1+F(S),0}=-\#poli_{prp}(F(S))
$$
Calcolare le rotazioni di $1+F(S)$ in $0$ equivale a calcolare le rotazioni di $F(S)$ in $-1$, e questa è la forma finale del criterio di Nyquist.
$$
R_{F(S),-1}=-\#poli_{prp}(F(S))
$$