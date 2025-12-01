---
tags:
  - algoritmi
  - reti
  - instradamento
---
L’algoritmo **Link State Packet** è una delle famiglie fondamentali di [[Instradamento#Algoritmi|algoritmi di instradamento]] nelle reti di comunicazione, che costruisce una mappa topologica completa della rete prima di calcolare i percorsi. A differenza degli approcci [[Distance Vector]] (come [[RIP]]), che scambiano solo distanze, il Link State si basa sulla distribuzione capillare dello stato dei collegamenti (_Link State Packet_ - LSP) e sul calcolo autonomo dei percorsi minimi tramite l'[[Ricerca Cieca#Algoritmo di Dijkstra|algoritmo di Dijkstra]].

Questa architettura è la base dei moderni protocolli di routing per grandi reti, specificamente [[OSPF]] (*Open Shortest Path First*) e [[IS-IS]] (*Intermediate System to Intermediate System*).
## Funzionamento

La mappa della rete viene costruita distribuendo dei pacchetti speciali, i LSP. 
### Ciclo di Vita di un LSP

Un LSP è un pacchetto dati in cui viene dichiarata l'identità del router che lo sta inviando, a quali vicini è connesso e con quali costi: _"Io sono il Router X, e sono connesso ai vicini A, B, C con questi costi"_.  
Il processo di [[Flooding#Selective flooding|Selective Flooding]] è il meccanismo che garantisce che tutti i database (*LSDB*) siano identici.

Nella pratica, il flooding deve essere _affidabile_: non basta inviare il pacchetto, servono meccanismi di controllo.

- **Sequence Numbers:** Ogni LSP ha un numero di sequenza (a 32-bit in OSPF). Se un router riceve un LSP con un numero di sequenza _inferiore_ a quello che ha già in memoria, lo scarta (è un'informazione vecchia). Se è _superiore_, aggiorna il database e rilancia il pacchetto.

- **Aging (Invecchiamento):** Ogni record nel database ha un timer (es. `MaxAge` in OSPF). Se un LSP non viene riaggiornato dal creatore originale entro un certo periodo di tempo (es. 30-60 minuti), viene considerato non valido e rimosso. Questo evita che router fantasma o link ormai inesistenti rimangano nella topologia.

>[!warning] Limiti
>- **Micro-loop:** Durante la convergenza (mentre i database si stanno allineando), router diversi possono avere visioni diverse della rete, causando loop temporanei di pacchetti.
>- **Flooding Storms:** Un guasto hardware che fa oscillare un'interfaccia (up/down continuo) può inondare la rete di LSP aggiornati, saturando la CPU dei router. Per mitigare ciò, si usano timer esponenziali per rallentare la generazione di aggiornamenti in caso di instabilità frequente (_LSP Throttling_).
### Il problema delle LAN e lo "Pseudo-Nodo"

Le LAN, i domini di collisione, sono modellati come uno "pseudo-nodo" per evitare un numero quadratico di adiacenze ($n^2$).

![[modello-lan-dr.png]]

>[!example] Esempio
>In una rete Ethernet con 10 router, se tutti dovessero formare adiacenze con tutti, avremmo 45 connessioni logiche.

- **OSPF:** Elegge un **Designated Router (DR)**. Tutti i router parlano solo con il DR, che aggrega le informazioni e le ridistribuisce come se fosse un nodo centrale.

- **IS-IS:** Usa il **Designated Intermediate System (DIS)**. A differenza del DR di OSPF, il DIS in IS-IS è prempitive (può cambiare se arriva un router con priorità più alta) e non esiste un "Backup" (BDR) formale, rendendo il processo leggermente più fluido ma diverso nella gestione dei fallimenti.
## Efficienza

Il file dedica ampio spazio alla dimostrazione matematica della correttezza di Dijkstra tramite il **Lemma dell'Invariante**.

Il concetto chiave è l'espansione *greedy*: l'algoritmo fissa definitivamente la distanza di un nodo solo quando è certo che non esistano percorsi indiretti più brevi.
### Complessità Computazionale

- **$O(n^2)$**: Implementazione naive con array o liste non ordinate.
- $O(n \log n + m)$: Implementazione sofisticata con heap di Fibonacci.

>[!tip] Approfondimento tecnico
>In una rete reale con migliaia di nodi, ricalcolare Dijkstra da zero (Full SPF) a ogni minimo "flap" di un link è troppo oneroso per la CPU. I router moderni possono operare alcune ottimizzazioni:
>- **iSPF (Incremental SPF):** Se cambia solo la connessione di una foglia della rete, non serve ricalcolare tutto l'albero; si aggiorna solo il ramo interessato.
>- **PRC (Partial Route Calculation):** Se cambia solo l'informazione di raggiungibilità (es. una subnet IP) ma non la topologia dei router, si evita completamente il calcolo SPF.
