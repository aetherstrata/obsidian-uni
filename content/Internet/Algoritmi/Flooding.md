---
tags:
  - instradamento
  - algoritmi
  - reti
---
Il **flooding** è un [[Instradamento#Tipi di algoritmi|algoritmo di instradamento]] che inoltra ogni pacchetto ricevuto dal router su tutte le linee tranne quella da cui è arrivato. Per questo risulta essere uno degli algoritmi più semplici da implementare e più efficaci in quanto trova sempre il percorso migliore, ma tende a saturare le linee. Questo algoritmo genera un vasto numero di pacchetti duplicati; in effetti, un numero infinito, a meno di non prendere qualche misura per fermare il processo. 
## Varianti 
### Age counter

Si inserisce nel pacchetto un contatore (*age counter*) da decrementare ad ogni nuovo router attraversato. Idealmente il valore di tale contatore deve essere uguale al [[Ricerca Cieca#Algoritmo di Dijkstra|percorso minimo]] fra sorgente e destinazione ma non conoscendo la topologia della rete si può assegnare un valore uguale al _diametro della rete_.
### Sequence number

Ogni router deve conoscere la presenza degli altri router e per ogni router dovrà solo controllare che il pacchetto proveniente da quello abbia un numero sequenza maggiore del precedente. Per evitare la crescita all'infinito si adotta una soglia _k_ che riassume la ricezione di tutte le sequenze fino ad appunto _k_. Raggiunta la soglia il numero si azzera.

Inoltre, un router può anche scartare un pacchetto al suo secondo passaggio nel nodo.

>[!warning] Attenzione
>Le varianti necessitano di memorie estese e di identificatori di pacchetto
## Selective flooding

In questa variante i router non spediscono ogni pacchetto in arrivo in ogni linea di uscita, ma solo su quelle linee che vanno approssimativamente nella giusta direzione. Ha poco senso spedire un pacchetto diretto ad ovest in una linea diretta a est, a meno che la topologia sia estremamente particolare.

Il protocollo [[IS-IS]] fa uso di questo algoritmo.