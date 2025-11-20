---
tags:
  - algoritmi
  - ai
  - stabilità
---
Una delle complicazioni più gravi del processo di ricerca è quella di perdere tempo di elaborazione esplorando stati che sono stati già visitati. Ci sono casi in cui la ripetizione degli stati è inevitabile: ad esempio, tutti i problemi in cui le azioni sono reversibili, come le ricerche di itinerari o i rompicapo. In questi casi gli alberi di ricerca sono infiniti.

>[!danger] Attenzione
>La ricerca con stati ripetuti può trasformare un problema lineare in un problema esponenziale!

Gli stati ripetuti non rilevati dall'algoritmo possono in buona sostanza far diventare irrisolvibile un problema che non lo è. Può essere dunque conveniente controllare se uno stato è replicato più volte nell'albero di ricerca. Rilevare le ripetizioni in genere significa confrontare i nodi che si stanno generando con quelli già espansi o generati. Se si ha una corrispondenza, l'algoritmo ha scoperto due diversi cammini che portano allo stesso stato e può scartarne uno.

Per evitare il rischio di esplorare gli stati già visitati si può ricordare il percorso dive si è passati, tenendo quindi più stati in memoria. Con questo metodo i nuovi nodi i cui stati corrispondono a quelli di nodi generati precedentemente possono essere scartati.
