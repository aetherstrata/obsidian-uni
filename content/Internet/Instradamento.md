---
tags:
  - algoritmi
  - instradamento
  - reti
---
L'**instradamento** (routing) è un processo globale che determina il percorso che intraprende un pacchetto IP lungo tutta la rete dal sistema mittente a quello destinatario. 

L'**inoltro** (forwarding) è un processo locale al router per trasferire un pacchetto da un link di ingresso a un link di uscita.

>[!tip]
>Ogni host è *direttamente connesso* a un router quindi il problema dell'instradamento si riduce all'instradamento del pacchetto dal router sorgente al router destinatario.

Un **protocollo** di routing è un processo di comunicazione tra i router per scambiarsi informazioni utilizzate per formare la tabella di routing.

Il funzionamento di ogni protocollo è retto da un **algoritmo** di routing che determina il percorso da seguire per ciascun pacchetto.
## Algoritmi

```mermaid
graph TD;
	HEAD[Algoritmi di instradamento]-->STATIC[Algoritmi statici]
	HEAD-->DYNAMIC[Algoritmi dinamici]
	STATIC-->SR[Static routing]
	STATIC-->FLOOD[Flooding]
	DYNAMIC-->ISOLATED[Routing isolato]
	DYNAMIC-->CENTER[Routing centralizato]
	DYNAMIC-->DIST[Routing distribuito]
	ISOLATED-->HP[Hot potato]
	ISOLATED-->BL[Backward learning]
	DIST-->DV[Distance Vector]
	DIST-->LSP[Link-state packet]
```
### Qualità degli algoritmi

Gli algoritmi di instradamento sono fondamentali per il corretto funzionamento di una rete.  Si usano diversi criteri per scegliere il giusto algoritmo da implementare.
#### Efficienza

Gli algoritmi dovrebbero essere abbastanza efficienti nel calcolo dei cammini verso la destinazione per evitare che questo abbia un peso eccessivo rispetto all'instradamento dei pacchetti. Questo è un requisito importante soprattutto nelle reti più complesse, con grandi tabelle di instradamento, in quanto i processori e le memorie attualmente disponibili sui router sono talvolta insufficienti.
#### Ottimalità

Normalmente, per calcolare l'ottimalità di un algoritmo, vengono usate metriche come il numero di hop su cui deve passare un pacchetto o il costo delle linee su cui transita. Di solito queste sono metriche statiche perché tenere in considerazione il carico corrente della rete è complicato.
#### Robustezza

Un buon algoritmo di instradamento dovrebbe essere affidabile e potersi adattare a variazioni della topologia della rete. In reti di grandi dimensioni si presentano spesso delle variazioni, come dei guasti a linee o router.
#### Stabilità

Se l'algoritmo ha scelto un determinato percorso verso una destinazione, allora quel cammino dovrebbe restare tale finché non si presenteranno variazioni alla topologia della rete. Quando accade, l'algoritmo dovrebbe essere in grado di convergere velocemente verso un nuovo percorso.
#### Equità

Tutti i nodi della rete devono essere trattati allo stesso modo. Non ci possono essere nodi privilegiati o danneggiati.
#### Economicità

L'algoritmo dovrebbe avere costi ridotti di manutenzione e configurazione

>[!note] Difficoltà nella scelta
>I criteri per la scelta del giusto algoritmo possono essere contrastanti tra loro. Ad esempio, non si può minimizzare il ritardo di pacchetti e massimizzare l’utilizzo delle linee allo stesso tempo senza introdurre dei compromessi. Gli algoritmi più complessi possono comportare configurazioni difficili e costi più alti di manutenzione a causa del personale richiesto per la gestione.

### Tipi di algoritmi
#### Algoritmi statici

I criteri di instradamento usati per calcolare i percorsi sono fissi, indipendenti dallo stato della topologia.
#### Algoritmi dinamici

L'algoritmo tiene conto del carico e/o della topologia corrente della rete per instradare i pacchetti.

>[!tip] Rapporto tra routing statico e dinamico
>Il routing statico è preferito nelle parti periferiche della rete, in cui si ha una topologia prevalentemente ad albero e poche variazioni nel tempo. Al contrario, il routing dinamico è preferito nella parte centrale della rete, in cui è presente una topologia fortemente magliata e molte variazioni.

