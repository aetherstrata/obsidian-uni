---
tags:
  - algoritmi
  - instradamento
  - reti
---
L’instradamento **Backward Learning** è un [[Instradamento#Algoritmi|algoritmo di instradamento]] in cui ogni nodo di rete impara verso quale linea uscire osservando da quale linea arrivano i pacchetti, usando in particolare l’indirizzo sorgente anziché quello destinazione.
## Funzionamento

Nel Backward Learning, quando un *intermediate system* riceve un pacchetto proveniente da, ad esempio, una certa sorgente A su una linea L3, memorizza nella propria tabella che per raggiungere A si usa la linea L3. In questo modo l'IS costruisce dinamicamente le proprie tabelle di instradamento basandosi sul traffico che attraversa la rete, senza scambi espliciti di informazioni di routing con gli altri nodi.
​
Un esempio tipico è il filtering nei [[bridge]] Ethernet conformi allo standard IEEE 802.1D, dove la porta associata a un certo indirizzo MAC viene appresa quando un frame con quell'indirizzo come sorgente entra su una porta, e successivamente i frame diretti a quell'indirizzo vengono inoltrati solo su quella porta.
## Arricchimento con il costo

Una raffinata estensione del **Backward Learning** consiste nell'aggiungere un campo con il “costo” nell'header dei pacchetti, inizializzato a zero dalla macchina sorgente e incrementato a ogni attraversamento di un IS. Così ogni IS, oltre a sapere su quale linea raggiungere una destinazione, può mantenere più alternative per la stessa destinazione, ordinate per costo crescente, ad esempio hop count.

Questo permette al nodo di preferire sistematicamente i percorsi con costo minore, ottenendo un comportamento simile a un routing minimo ma senza eseguire algoritmi globali come [[Ricerca Cieca#Algoritmo di Dijkstra|Dijkstra]], perché l’informazione di costo è trasportata nel traffico stesso.

>[!warning] Apprende solo le migliorie
>Il limite principale di questo approccio è che gli IS apprendono solo le migliorie e non i peggioramenti delle condizioni di rete. 
>
>Se si rende disponibile un nuovo cammino a costo più basso, prima o poi qualche flusso lo userà, e i nodi aggiorneranno le loro entry con un costo minore, rimpiazzando il vecchio next-hop nella tabella.
>
>Se invece un link si guasta o un cammino peggiora (maggiore congestione o ritardo), l'IS non riceve più pacchetti da quella direzione, ma non riceve neanche un’informazione esplicita che quel cammino non è più valido. 
>
>---
>Per evitare che le tabelle restino piene di percorsi obsoleti, ogni entry ha un tempo di validità: allo scadere del timer l’entry viene eliminata, e solo alla successiva ricezione di traffico dal nuovo percorso la voce verrà aggiornata.
## Destinazione ignota e flooding

Quando un IS riceve un pacchetto diretto a una destinazione che non conosce (quindi non ha nessuna entry nella tabella) usa il [[flooding]]: inoltra il pacchetto su tutte le linee tranne quella di arrivo. Questo massimizza la probabilità che il pacchetto raggiunga la destinazione, che a sua volta risponderà e permetterà ai vari nodi lungo il percorso di apprendere l’associazione indirizzo - linea (Backward Learning a partire dalla risposta).

Il flooding ha però un costo elevato in termini di banda e può amplificare problemi di topologia (maglie e cicli), quindi nella pratica si cerca di limitarlo (es. [[Flooding#Selective flooding|selective flooding]], TTL, o memorizzazione dei pacchetti già visti per scartarli alla seconda occorrenza).
## Problema dei cicli

In topologie altamente magliate il Backward Learning, combinato con il flooding, può generare loop: un pacchetto può rimbalzare su percorsi ciclici se le tabelle sono incoerenti o se il pacchetto viene replicato su più rami che poi si riconnettono. I routing loop consumano inutilmente banda e risorse computazionali, oltre a introdurre ritardi imprevedibili.