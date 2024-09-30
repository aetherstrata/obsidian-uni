---
tags:
  - segnale
---
Un segnale è una variazione temporale dello stato fisico di un [[Sistema|sistema]] o di una grandezza fisica, come il campo elettromagnetico per i segnali radio, o la tensione in un cavo per i segnali elettrici, per rappresentare e trasmettere delle informazioni.

I due principali tipi di segnali sono i segnali analogici e i segnali digitali.
## Classificazione dei segnali
### Segnali analogici e digitali

#### Segnali analogici
Un segnale analogico può variare gradualmente in un intervallo costituito da
un numero infinito di possibili valori. 

Un segnale analogico può essere rappresentato mediante una funzione del tempo che gode delle seguenti caratteristiche: 
- la funzione è continua nel dominio, ovvero è definita per ogni valore del tempo
- la funzione è continua
#### Segnali digitali
Un segnale digitale può variare solamente passando bruscamente da uno all'altro di un insieme molto piccolo di valori (da due a qualche decina).

Un segnale digitale può essere rappresentato da una funzione “tempo discreta” e “quantizzata”. Tale funzione risulta pertanto: 
- definita solamente in un insieme numerabile di istanti equispaziati
- dotata di un codominio costituito da un insieme discreto di valori
### Durata dei segnali
#### Durata rigorosamente limitata
I segnali a durata rigorosamente limitata si annullano identicamente al di fuori di un certo intervallo temporale. Esempi di questi segnali sono la [[Funzione Finestra#Finestra rettangolare|finestra rettangolare]] e la [[Funzione Finestra#Finestra triangolare|finestra triangolare]].
#### Durata illimitata
I segnali a durata illimitata assumono valori non trascurabili su tutto l'asse temporale. Esempi di questi segnali sono il [[Segnale Gradino|segnale gradino]] e la [[Funzione Segno|funzione segno]] 
#### Durata praticamente limitata
I segnali a durata praticamente limitata decadono asintoticamente a zero, per cui si possono ritenere trascurabili al di fuori di un certo intervallo che indica la misura della durata. Esempi di questi segnali sono il [[Segnale Esponenziale|segnale esponenziale]] e il [[Segnale di Gauss|segnale di Gauss]]. 
### Energia dei segnali
#### Segnali di energia
Un segnale è un segnale di energia quando il suo [[Energia e potenza#Energia di un segnale analogico|integrale di energia]] è diverso da $0$ e $\infty$.
$$\large \int_{-\infty}^{\infty}|x(t)|^2\ dt\ \neq\ \{0,\infty\}$$
#### Segnali di potenza
Un segnale è un segnale di potenza quando il suo [[Energia e potenza#Potenza di un segnale analogico|integrale di potenza]] è diverso da $0$ e $\infty$.
$$\large\lim\limits_{\Delta T \to\infty}\frac{1}{\Delta T} \int_{-\Delta T/2}^{\Delta T/2} |x(t)|^2\ dt \neq \{0,\infty\}$$

>[!info] Le categorie di segnali di energia e potenza sono disgiunte
>- Se un segnale ha energia finita, la sua potenza è nulla
>- Se un segnale ha potenza finita, la sua energia è infinita
## Operazioni sui segnali

### Somma
I segnali vengono sommati tra loro per tutto il dominio.
$$z(t) = x(t) + y(t)$$
### Moltiplicazione
I segnali vengono moltiplicati tra loro per tutto il dominio.
$$z(t)=x(t)\cdot y(t)$$

>[!summary] Impatto sulla finestra di integrazione
>Se per alcune regioni del suo dominio un segnale vale $0$, quando viene moltiplicato per altre funzioni può restringere la finestra di integrazione del segnale.
> $$\text{Segnale moltiplicato per un gradino :}\quad \large\int_{-\infty}^{\infty} x(t)u(t)\ dt=  \int_{0}^{\infty} x(t)\ dt$$

>[!example] Esempio - Moltiplicazione tra un rettangolo e un triangolo
>Dati i segnali $x(t)=\operatorname{rect}(t)$ e $y(t)=\operatorname{tri}(t)$, il risultato della loro moltiplicazione è:
>$$z(t)=\left\{\begin{matrix*}
>1-|t| & \longrightarrow & -\frac{1}{2} < t < \frac{1}{2} \\
>0 & \longrightarrow & altrove
>\end{matrix*}\right.$$
>![[moltiplicazione_finestra.png|center]]
### Ribaltamento
I segnali vengono ribaltati invertendo il segno dell'argomento.
$$z(t) = x(-t)$$
>[!important] Il segnale ribaltato è uguale a quello di partenza se la funzione è pari

>[!example] Esempio - Ribaltamento di un esponenziale
>Dato l'esponenziale $x(t) =e^{-2t}$, il segnale ribaltato vale $x(-t)=e^{2t}$.
>
>![[ribaltamento_esponenziale.png]]
### Traslazione
Il segnale viene traslato aggiungendo un valore costante all'argomento.
$$z(t) = x(t - \tau)$$
>[!example] Esempio -  Finestra rettangolare ritardata
>Dato un segnale rettangolare $x(t) = \operatorname{rect}(t)$, il suo risultato traslato di 3 unità nel passato vale $x(t-3)=\operatorname{rect}(t-3)$.
>$$ rect(t-3) = \left\{ \begin{matrix*}
1 & \longrightarrow & 3-\small\dfrac{1}{2}\normalsize< t < 3+\small\dfrac{1}{2}\normalsize\\
0 & \longrightarrow & altrove
\end{matrix*}\right.$$
>![[finestra_ritardata.png]]

### Cambio di scala
La scala viene manipolata moltiplicando un valore all'argomento.
$$z(t) = x(\alpha t)$$
>[!example] Esempio - Coseno con frequenza raddoppiata
>Dato un segnale coseno $x(t) = cos(2\pi f_0t)$, il segnale scalato $x(2t)$ vale $cos(4 \pi f_0 t)$. Si può notare come il segnale in scala è uguale a quello iniziale, ma a frequenza doppia $(2f_0)$.
>
>![[coseno_scala.png]]
