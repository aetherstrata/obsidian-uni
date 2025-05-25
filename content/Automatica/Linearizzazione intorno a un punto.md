---
tags:
  - analisi
  - controllo
  - matematica
  - sistema
  - stabilità
---
La linearizzazione attorno a un punto è un approccio utilizzabile per linearizzare un sistema con comportamenti non lineari e studiarlo più facilmente, tenendo conto che questo modello è valido solo nell'intorno del punto preso come riferimento.

Alla base di questo metodo c'è lo sviluppo in serie di Taylor della funzione centrata nel punto di interesse $x_0$.
$$
y=f(x)\quad\longrightarrow\quad y=\sum_{n=0}^\infty \frac{f^{(n)}(x_0)}{n!}(x-x_0)^n
$$
Di questa serie si prendono solo i termini $0$ e $1$, in quanto gli altri presentano il termine $(x-x_0)^n$ con un grado maggiore di $1$, quindi non lineare.
## Esempio
Questo approccio si può usare per studiare il comportamento di un pendolo in un punto di equilibrio $\theta_0$.
![[pendolo.png|center|300]]
$$
\underbrace{Ml^2\ddot\theta}_{\text{Inerzia}} + \underbrace{D\dot\theta}_{\mathclap{\text{Attrito}}} + \underbrace{Mgl\sin\theta}_{\text{Forza}} = \tau \text{ (coppia imposta)}
$$
Il termine non lineare è la forza $Mgl\sin\theta$. Per risolvere il problema si identifica il punto d’equilibrio in $\theta_0$, quindi si può esprimere $\theta$ come $\theta_0 + \Delta\theta$.

Utilizzando lo sviluppo di Taylor per il termine $\sin\theta$, si possono riscrivere tutti i termini $\theta$ del sistema.
$$
\begin{gather*}
\dot\theta = \Delta\dot\theta \qquad  \ddot\theta = \Delta\ddot\theta \qquad \sin\theta = \sin\theta_0 + \cos\theta_0\Delta\theta \\\\
Ml^2\Delta\ddot\theta + D\Delta\dot\theta + Mgl\left(\sin\theta_0 + \cos\theta_0\Delta\theta\right) = \tau
\end{gather*}
$$
Bisogna analizzare le condizioni da rispettare per far arrivare il sistema nel punto scelto. In questo caso è un punto di equilibrio, quindi le derivate si annullano.
$$
Mgl\sin\theta_0 = \tau_0 \quad\text{Coppia necessaria per equilibrare il sistema}
$$
Avendo trovato un valore di $\tau_0$ per cui il sistema è in equilibrio, si può riscrivere l'equazione iniziale tenendo in considerazione che $\tau = \tau_0 + \Delta\tau$.
$$
\begin{align*}
Ml^2\Delta\ddot\theta + D\Delta\dot\theta + Mgl\left(\sin\theta_0 + \cos\theta_0\Delta\theta\right) &= \tau_0 + \Delta\tau \\\\
Ml^2\Delta\ddot\theta + D\Delta\dot\theta + Mg\cos\theta_0\Delta\theta &= \Delta\tau 
\end{align*}
$$
Questa è l'equazione linearizzata che descrive l'andamento del sistema nell'intorno di $\theta_0,\tau_0$.

Applicando la [[Trasformata di Laplace]] si ottiene la funzione di trasferimento del sistema linearizzato.
$$
F(S) = \frac{1}{Ml^2s^2 + Ds + Mgl\cos\theta_0}
$$
Il sistema lineare prende in ingresso $\Delta\tau$ e produce $\Delta\theta$; quello non lineare prende $\tau$ e produce $\theta$.

Esiste una relazione importante tra la stabilità del sistema non lineare e quello linearizzato: le
traiettorie del sistema non lineare convergono asintoticamente al punto d’equilibrio considerato
se il sistema linearizzato associato è asintoticamente stabile.