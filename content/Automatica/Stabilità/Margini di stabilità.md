---
tags:
  - analisi
  - controllo
  - sistema
  - stabilità
---
Partendo dal presupposto che il sistema a controreazione in analisi sia asintoticamente stabile,
per cui $Y(S) = F(S)U(S)$ e le radici del polinomio caratteristico sono tutte a parte reale
negativa, i margini di stabilità sono dei parametri che forniscono una misura sulla qualità della stabilità.

Per fare ciò si usano due parametri: 
- il margine di guadagno $m_G$
- il margine di fase $m_\varphi$

Prendiamo una generica rappresentazione con i diagrammi di Bode e Nyquist di un sistema
asintoticamente stabile con fase che scende sotto i -180°.

## Bode

### Fase
Nel [[Diagramma di Bode]] si definisce _omega di taglio_ $\omega_t$ il punto in cui il guadagno scende a $0$ dB. Questo è il punto in cui il sistema smette di amplificare il segnale. Il valore di questo punto sul diagramma di fase indica il _margine di fase_.
### Guadagno
Nel [[Diagramma di Bode]] di definisce _margine di guadagno_ il punto sul diagramma dei guadagni in cui la fase scende sotto ai $-180^{\circ}$. Questo vuol dire che il guadagno del sistema può essere aumentato solo di questa quantità prima che diventi instabile.

## Nyquist

Come detto per il [[Criterio di Nyquist]], un sistema è instabile se la sua rappresentazione nel [[Diagramma di Nyquist]] effettua delle rotazioni attorno al punto $-1$. Tracciando una circonferenza centrata nell'origine e passante per quel punto, si possono trovare gli stessi margini. 
### Fase
Il _margine di fase_ è l'angolo con cui la funzione di trasferimento interseca la circonferenza.
### Guadagno
Il _margine di guadagno_ è l'inverso della distanza della funzione dall'origine quando passa per l'asse orizzontale.

![[1747835266.png]]

>[!tip] Limite di stabilità
>Se la curva passa esattamente per il punto $-1$, si avranno delle condizioni ben precise:
>$$
>|F(j\omega)| = 1 \quad ; \quad \angle F(j\omega) = -180^{\circ}
>$$
>Questo vuol dire che l'ingresso del sistema $u(t) = \sin(j\omega t)$ arriva in uscita dal sistema ribaltato di $-180^{\circ}$, quindi $y(t) = -\sin(j\omega t)$.
>
Il sistema quindi mantiene l’oscillazione posta in input, ritrovandosi al limite della stabilità. In
particolare:
>- se il grafico passa a destra di $-1$, il sistema è stabile
>- se passa proprio per il punto $-1$, è al limite della stabilità
>- se passa a sinistra del punto $-1$, il sistema è instabile.


