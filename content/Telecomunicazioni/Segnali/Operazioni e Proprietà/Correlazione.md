---
tags:
  - operatore
  - matematica
---
L'operazione di correlazione indica il grado di similitudine di due [[Segnale|segnali]], ovvero un’indicazione di quanto due segnali siano tra loro simili.
$$
z(t) = x(t) \otimes y(t) = \int_{-\infty}^{\infty}x(t+\tau)\cdot y^*(\tau)\ d\tau = R_{xy}(t)
$$
Se i due segnali sono diversi si parla di **cross-correlazione**, quando invece i due segnali sono uguali si parla di **auto-correlazione**. 

>[!fail] La correlazione non gode della proprietà commutativa
>Se si calcola la correlazione $R_{yx}(t)$ si otterrà il seguente integrale.
>$$
>R_{yx}(t)=\int_{-\infty}^{\infty}y(t+\tau)\cdot x^*(\tau)\ d\tau
>$$
>Per trasformarlo in una forma più simile a quella di $R_{xy}$ si effettua un cambio di variabile.
>$$
>t+\tau=\tau'\quad\longrightarrow\quad\int_{-\infty}^{\infty}y(\tau')\cdot x^*(-t+\tau')\ d\tau'
>$$
>Poi si sposta l'operazione di coniugazione da $x$ a $y$.
>$$
>\int_{-\infty}^{\infty}y(\tau')\cdot x^*(-t+\tau')\ d\tau' = \left[\int_{-\infty}^{\infty}y^*(\tau')\cdot x(-t+\tau')\ d\tau'\right]^{\large*}
>$$
>Messe a confronto, $R_{xy}$ e $R_{yx}$ hanno alcune somiglianze.
>$$
>\begin{align*}
> R_{xy}(t)= & \int_{-\infty}^{\infty}x(t+\tau)\cdot y^*(\tau)\ d\tau \\
> R_{yx}(t)= & \left[\int_{-\infty}^{\infty}x(-t+\tau')\cdot y^*(\tau')\ d\tau'\right]^{\large*}
> \end{align*}
>$$
> La correlazione $R_{yx}$ è il complesso coniugato della correlazione $R_{xy}$ ribaltata.
>$$
> R_{xy}(t) = R^*_{yx}(-t)
>$$
## Relazione con la convoluzione

L'operazione di correlazione può essere riscritta come una [[Convoluzione|convoluzione]].
$$
R_{xy}(t) = x(t)* y^*(-t) = \int_{-\infty}^{\infty}x(t-\tau)\cdot y^*(-\tau)d\tau
$$
Cambiando la variabile di integrazione questo diventa uguale a quello per la correlazione.
$$
\tau'\coloneqq-\tau\quad\longrightarrow\quad\int_{-\infty}^{\infty}x(t+\tau')\cdot y^*(\tau')d\tau'
$$
>[!important] Correlazione per segnali reali pari
>Se entrambi i segnali sono reali pari, allora la loro correlazione sarà uguale alla loro convoluzione.
## Autocorrelazione

L'operazione di auto-correlazione avviene quando un segnale $x(t)$ è correlato con se stesso.
$$
R_{xx}(t) = \int_{-\infty}^{\infty}x(t-\tau)\ x^*(\tau)\ d\tau
$$
### Autocorrelazione come energia del segnale
Se si calcola il valore dell'auto-convoluzione in $t=0$, l'integrale sarà uguale a quello per l'[[Energia e potenza#Energia di un segnale analogico|energia]] del segnale.
$$
R_{xx}(0) = \int_{-\infty}^{\infty}x(\tau)\ x^*(\tau)\ d\tau = \int_{-\infty}^{\infty} \left|x(\tau)\right|^2\ d\tau = E_x
$$
