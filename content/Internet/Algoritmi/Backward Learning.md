---
tags:
  - algoritmi
  - instradamento
  - reti
---
L’instradamento **Backward Learning** è un [[Instradamento#Algoritmi|algoritmo di instradamento]] in cui ogni nodo di rete impara verso quale linea uscire osservando da quale linea arrivano i pacchetti, usando in particolare l’indirizzo sorgente anziché quello destinazione.
## Funzionamento

Nel Backward Learning, quando un *intermediate system* riceve un pacchetto proveniente da, ad esempio, una certa sorgente A su una linea L3, memorizza nella propria tabella che “per raggiungere A si usa la linea L3”. In questo modo l’IS costruisce dinamicamente le proprie tabelle di instradamento basandosi sul traffico che attraversa la rete, senza scambi espliciti di informazioni di routing con gli altri nodi.
​
Un esempio tipico è il filtering nei [[bridge]] Ethernet conformi allo standard IEEE 802.1D, dove la porta associata a un certo indirizzo MAC viene appresa quando un frame con quell'indirizzo come sorgente entra su una porta, e successivamente i frame diretti a quell'indirizzo vengono inoltrati solo su quella porta.
## Arricchimento con il costo

Una raffinata estensione del **Backward Learning** consiste nell'aggiungere un campo con il “costo” nell'header dei pacchetti, inizializzato a zero dalla stazione sorgente e incrementato a ogni attraversamento di un IS. Così ogni IS, oltre a sapere su quale linea raggiungere una destinazione, può mantenere più alternative per la stessa destinazione, ordinate per costo crescente (ad esempio hop count o un proxy del ritardo).

Questo permette al nodo di preferire sistematicamente i percorsi con costo minore, ottenendo un comportamento simile a un routing minimo ma senza eseguire algoritmi globali come Dijkstra, perché l’informazione di costo è trasportata nel traffico stesso.

​

## Limite