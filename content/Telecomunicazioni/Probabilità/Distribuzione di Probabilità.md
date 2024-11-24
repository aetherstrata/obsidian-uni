La funzione densità di probabilità è la funzione che fornisce le probabilità di accadimento dei possibili risultati.
## Distribuzione discreta
Se la [[Variabile Aleatoria]] $X$ è discreta, cioè lo spazio campionario è finito o numerabile, la distribuzione di probabilità è una funzione discreta ed è chiamata *funzione massa di probabilità*.
### Funzione massa di probabilità
La funzione massa di probabilità è la distribuzione di probabilità di una variabile aleatoria discreta e fornisce le probabilità associate ai possibili valori.
$$
p_X(x)=P(X=x) \quad : \quad \mathbb{R}\to[0,1]
$$
Le probabilità associate a tutti i possibili eventi devono essere non negative e avere somma unitaria.
$$
p_X(x)\ge 0 \quad ; \quad \sum_{x}p_X(x)=1
$$
Questa distribuzione può anche essere riscritta come somma di [[Delta di Dirac]] con ampiezze corrispondenti alle probabilità $p_n$ dei valori $x_n$.
$$
p_X(x)=\sum_{i=1}^{n}p_i\delta(x-x_i)
$$
Questo approccio permette anche di operare su distribuzioni che sono a parti discrete e a parti continue.
### Valore atteso
$$
\mathbb{E}[X]=\sum_{i=1}^{n}x_ip_i
$$
## Distribuzione continua
Se la [[Variabile Aleatoria]] $X$ è continua la distribuzione di probabilità è una funzione continua ed è definita come l'integrale nello spazio campionario $A$ della *funzione densità di probabilità* $f(x)$.
$$
P(X \in A) = \int_{A} f(x)  dx
$$
Le probabilità associate a tutti i possibili eventi devono essere non negative e avere somma unitaria.
$$
\int_{-\infty}^{\infty} f(x) dx = 1
$$
### Valore atteso
$$
\mathbb{E}[X] = \int_{-\infty}^{\infty} x\cdot f(x) dx 
$$
### Distribuzione cumulativa
Nel caso in cui la [[Variabile Aleatoria]] sia un valore reale, la **distribuzione continua** può equamente essere rappresentata da una distribuzione cumulativa che fornisce la probabilità di accadimento di un evento *prima o dopo un certo punto*.
$$
F(x)=P(X\le x)
$$
La distribuzione cumulativa è quindi una funzione monotona crescente continua a destra che mantiene le proprietà fondamentali delle distribuzioni.

- Può assumere solo valori compresi tra $0$ e $1$
$$
F(x) : \mathbb{R}\to[0,1]
$$
- Dato che la funzione è monotona, questi valori sicuramente compaiono almeno agli estremi del dominio 
$$
\begin{array}{c|c}
\lim\limits_{x\to-\infty}F(x)=0 & \lim\limits_{x\to\infty}F(x)=1
\end{array}
$$
- Integrare la funzione restituisce la probabilità che accada un evento compreso tra gli estremi di integrazione
$$
P(a<X<b)=\int_{a}^{b}f(x)dx=F(b)-F(a)
$$
Questa funzione di distribuzione è anche detta **funzione di ripartizione**.
### Distribuzione uniforme
Nel caso in cui gli eventi della [[Variabile Aleatoria]] hanno tutti la stessa probabilità di verificarsi in  un intervallo $(a,b)$, allora la distribuzione di probabilità si dice uniforme.
#### Densità della probabilità
$$
f_X(x)=\left\{\begin{matrix*}
\dfrac{1}{b-a} & a<x<b \\
0 & altrimenti
\end{matrix*}\right.
$$
#### Ripartizione della probabilità
$$
f_X(x)=\left\{\begin{matrix*}
0 & \hphantom{a<{}} x\le a\\
\dfrac{x-a}{b-a} & a<x<b \\
1 & \hphantom{a<{}} x\ge b
\end{matrix*}\right.
$$
>[!info] Grafico per $(-1,1)$
>![[1732458296.png|center]]
>$$
>\begin{align*}
>\mathbf{Blu } & \text{ funzione densità di probablità} \\
>\mathbf{Rosso } & \text{ distribuzione cumulativa di probabilità}\
>\end{align*}
>$$
### Distribuzione normale
#### Densità della probabilità
$$
f_X(x)=\frac{e^{-(x-\mu)^2/2\sigma^2}}{\sqrt{2\pi\sigma^2}}
$$