La funzione densità di probabilità è la funzione che fornisce le probabilità di accadimento dei possibili risultati.
## Distribuzione discreta
Se la [[Variabile Aleatoria]] $X$ è discreta, cioè l'insieme dei possibili valori è finito o numerabile, la distribuzione di probabilità è una funzione discreta ed è chiamata *funzione massa di probabilità*.
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
## Distribuzione continua
Se la [[Variabile Aleatoria]] $X$ è continua la distribuzione di probabilità è una funzione continua ed è definita come l'integrale nell'insieme degli eventi $A$ della *funzione densità di probabilità* $f(x)$.
$$
P(X \in A) = \int_{A} f(x)  dx
$$
## Distribuzione cumulativa
Nel caso in cui la [[Variabile Aleatoria]] sia un valore reale, la distribuzione continua può equamente essere rappresentata da una distribuzione cumulativa che fornisce la probabilità di accadimento del valore dato *o qualsiasi altro valore minore*.
$$
F(x)=P(X\le x)
$$
La distribuzione cumulativa è quindi una funzione 