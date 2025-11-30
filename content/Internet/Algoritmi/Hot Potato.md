---
tags:
  - algoritmi
  - instradamento
  - reti
---
L’algoritmo **Hot Potato** è una famiglia di strategie di routing senza buffering, in cui ogni pacchetto viene inoltrato immediatamente sul link disponibile con la coda minore, anche se non è il più ideale, per evitare di essere trattenuto nel nodo. È usato come modello teorico e in alcune applicazioni pratiche di reti di calcolatori e sistemi distribuiti, soprattutto in topologie regolari e in contesti ad alta velocità.
## Funzionamento

Nel routing **Hot Potato** ogni nodo non dispone (o dispone solo minimamente) di buffer per i pacchetti in transito, quindi non può memorizzare molti pacchetti in coda. Quando un pacchetto arriva, il nodo deve immediatamente inoltrarlo scegliendo una delle uscite disponibili, spesso privilegiando un link che lo avvicina alla destinazione, ma in caso di conflitto può deviarlo su un percorso alternativo.

>[!tip] Patata bollente
L’analogia è quella della patata bollente: nessuno la tiene in mano, quindi ogni nodo cerca di liberarsene il prima possibile, mantenendo i pacchetti sempre in movimento finché non raggiungono la destinazione. Questo approccio è anche detto *deflection routing*, perché i pacchetti possono essere deflessi rispetto al cammino più breve per risolvere i conflitti di instradamento.
## Modello di rete e regole

Il modello classico considera una rete come un grafo orientato: i nodi sono processori o router e gli archi sono i link di comunicazione. A ogni passo di tempo, ogni nodo può ricevere alcuni pacchetti in ingresso e deve decidere su quali link in uscita inviarli, rispettando il limite di capacità (tipicamente un pacchetto per porta per unità di tempo).

Le regole tipiche di un algoritmo Hot Potato sono:
- Nessun buffering interno (o buffering trascurabile): il pacchetto non può rimanere fermo nel nodo oltre uno step.
- Priorità ai percorsi che riducono la distanza alla destinazione, ad esempio in termini di hop rimanenti.
- In caso di conflitti (più pacchetti vogliono lo stesso link), si applica una politica di priorità o randomizzazione per decidere chi ottiene il link “migliore” e chi viene deviato su un link alternativo.

>[!info] Limite
>Queste scelte devono garantire che, nonostante le deviazioni, ogni pacchetto arrivi alla destinazione entro un numero di passi ragionevole, idealmente con un bound teorico in funzione della distanza iniziale e del carico di traffico.
## Vantaggi

- Semplicità dell’hardware: non servono grandi buffer nei nodi intermedi, cosa interessante in switch o router ad altissima velocità.
- Latenza di attraversamento potenzialmente bassa in condizioni di carico moderato, perché i pacchetti non devono attendere in coda ma si muovono continuamente.
- Comportamento adattivo: le deflessioni distribuiscono dinamicamente il traffico su percorsi alternativi, riducendo il rischio di colli di bottiglia localizzati.

## Svantaggi

- Possibili percorsi molto più lunghi del cammino minimo, soprattutto in condizioni di congestione, con aumento di latenza e jitter.
- Analisi e progettazione più complesse: servono prove formali o argomentazioni probabilistiche per garantire che i pacchetti non restino “intrappolati” in cicli di deflessione e che il tempo di consegna rimanga limitato.
- Maggiore imprevedibilità dei percorsi rispetto a metodi store-and-forward tradizionali, il che può complicare la qualità del servizio e il debugging.
## Applicazioni pratiche

Il concetto di Hot Potato routing ha influenzato diversi ambiti, tra cui:

- Reti di interconnessione per multiprocessori e supercalcolatori, dove il costo di buffering può essere alto e la regolarità della topologia rende il deflection routing particolarmente adatto.
- Instradamento tra sistemi autonomi su Internet, dove “hot potato routing” indica la pratica di consegnare il traffico al peer nel punto di interscambio più vicino, minimizzando la distanza percorsa nella propria rete.
- Studi su varianti come “hard potato routing”, che impongono ulteriori vincoli, e su algoritmi randomizzati che migliorano la distribuzione del traffico su mesh e tori.

>[!example] Esempio su topologie regolari
Gran parte della letteratura teorica studia il routing Hot Potato su topologie regolari come ipercubi, mesh bidimensionali, tori e reti a farfalla. In questi contesti è possibile definire metriche di distanza molto regolari (per esempio distanza Manhattan su una mesh 2D) e progettare algoritmi deterministici o randomizzati con garanzie di tempo di consegna.
>
Per esempio, nella mesh 2D ogni pacchetto è identificato da una coppia di coordinate sorgente e destinazione, e a ogni passo il nodo cerca di inoltrarlo lungo l’asse che riduce la distanza alla destinazione; se la porta è occupata, lo devia sull'altra dimensione o su una porta neutra, mantenendo comunque il pacchetto in movimento. L’analisi mostra che, con regole di priorità adeguate (ad esempio dando precedenza ai pacchetti più vicini alla destinazione), si ottengono limiti superiori sul tempo di consegna proporzionali alla distanza più un termine che dipende dal numero di pacchetti in rete.
