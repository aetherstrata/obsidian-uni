---
tags:
  - sistema
  - proprietà
---
Per comportamento a regime si intende il comportamento del sistema a controreazione dopo
un certo tempo dall’applicazione del segnale d’ingresso. L’analisi del comportamento a regime
è importante per comprendere con precisione il funzionamento del sistema, in particolare la sua
risposta. Una volta estinto il transitorio, la differenza che esiste tra il valore desiderato e quello
ottenuto identifica l’indice di fedeltà della risposta, o l’errore.

![[Pasted image 20250515201830.png]]

L'obiettivo è avere l'uscita del sistema $Y(S)$ il più vicino possibile all'uscita desiderata $Y_d(S)$, quindi l'errore di uscita $E(S)$ tra i due deve essere il più vicino possibile a $0$.
$$
y=y_d \quad \longrightarrow \quad \lim_{t\to\infty}e(t)=0
$$
L'errore è esprimibile con la seguente funzione di trasferimento, con $W(S)$ anello a ciclo chiuso