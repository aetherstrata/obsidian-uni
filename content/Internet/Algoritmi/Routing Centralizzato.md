---
tags:
  - instradamento
  - reti
  - algoritmi
---
Il **Routing Centralizzato** è una tecnica in cui l’intelligenza di [[instradamento]] è concentrata in un unico nodo logico, il Routing Control Center (**RCC**), che si occupa di calcolare e distribuire le tabelle di instradamento a tutti i nodi della rete. Questa soluzione permette un controllo molto fine delle rotte, ma introduce importanti problemi di scalabilità e affidabilità, che ne limitano l’applicabilità nelle grandi reti moderne.
## Presupposti del routing centralizzato

Nel routing centralizzato si assume l’esistenza di un **RCC** che conosce (o tenta di conoscere) l’intera topologia della rete e lo stato dei collegamenti. Ogni nodo invia periodicamente al **RCC** informazioni sui propri vicini, sullo stato dei link e, talvolta, sul traffico processato, in modo che il centro possa costruire una mappa globale e aggiornata della rete.

Questa ipotesi di “conoscenza globale” è spesso poco realistica in reti di grandi dimensioni o molto dinamiche, perché la topologia e le condizioni di traffico possono cambiare più velocemente di quanto il **RCC** riesca ad aggiornare la propria visione. Di conseguenza, le decisioni di instradamento possono basarsi su informazioni già obsolete nel momento in cui vengono applicate dai router.
## Funzioni del Routing Control Center

Il **RCC** svolge tre funzioni fondamentali: raccolta delle informazioni, calcolo delle tabelle di instradamento e distribuzione delle tabelle ai nodi. In fase di raccolta, si comporta come un collettore centralizzato di messaggi di stato inviati da tutti i router, che includono almeno la lista dei vicini e lo stato dei link.

Una volta costruita la mappa della rete, il **RCC** può usare algoritmi di cammino minimo (ad esempio basati su shortest path) e altre tecniche sofisticate di ottimizzazione per generare tabelle di routing globalmente ottimali o quasi ottimali. Infine, le tabelle così calcolate vengono inviate ai singoli nodi, che si limitano a usarle per instradare i pacchetti senza dover eseguire complessi algoritmi localmente.
## Vantaggi concettuali

La centralizzazione permette una gestione molto accurata del traffico, perché il **RCC** ha una visione globale e può, in linea di principio, effettuare un bilanciamento “predittivo” del carico tra percorsi multipli. Questo consente anche l’uso di algoritmi complessi (ad esempio ottimizzazioni multi‑vincolo) che sarebbero troppo pesanti da eseguire su ogni router.

In reti di dimensioni ridotte o con topologia relativamente stabile, un controllo centralizzato può semplificare l’amministrazione e offrire prestazioni molto buone con minore complessità sui nodi periferici. Inoltre, la presenza di un unico punto di decisione facilita l’implementazione di politiche uniformi di instradamento e di qualità del servizio.
## Problemi principali

Il primo problema è la forte concentrazione di traffico attorno al **RCC**: tutti i router devono inviare periodicamente informazioni di stato al centro e ricevere da esso le nuove tabelle di instradamento. Questo genera un notevole traffico di servizio (overhead di controllo) nella porzione di rete vicina al **RCC**, con possibili colli di bottiglia.

Il secondo problema è l’affidabilità: il **RCC** costituisce un singolo punto di guasto, per cui un malfunzionamento o un isolamento del centro può compromettere l’intero processo di instradamento. Per mitigare questo rischio è spesso necessario duplicare o ridondare il RCC, con conseguente aumento di complessità e costi di gestione.
## Problemi dinamici e di consistenza

In reti con traffico altamente variabile o con frequenti cambiamenti topologici, il routing centralizzato fatica ad adattarsi tempestivamente. I tempi di raccolta delle informazioni, calcolo delle tabelle e distribuzione comportano inevitabili ritardi, durante i quali la situazione reale della rete può cambiare.

Inoltre, le tabelle di routing aggiornate non arrivano simultaneamente a tutti i nodi: alcuni router applicano già le nuove rotte mentre altri usano ancora quelle vecchie. Questo periodo di “incoerenza temporanea” può portare a percorsi subottimali, loop transitori o decisioni di instradamento non uniformi, con possibili peggioramenti delle prestazioni.