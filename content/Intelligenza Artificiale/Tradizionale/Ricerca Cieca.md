---
tags:
  - algoritmi
  - ai
---
Un algoritmo di **ricerca cieca** (non informata) non riceve *nessuna informazione* su quanto uno stato sia vicino all'obiettivo.
## Algoritmo di Dijkstra

## Ricerca in ampiezza

Nella **ricerca in ampiezza** si procede
- espandendo il nodo radice
- espandendo *tutti* i nodi generati dalla radice
- espandendo *tutti* i nodi successori

>[!important] Nota
>Si passa ad espandere i nodi di profondità maggiore solo quando sono stati espansi tutti i nodi del livello precedente e non si è trovato uno stato obiettivo
>
>Questa è una ricerca sistematica. Trova sicuramente (se esiste) la soluzione realizzabile con il cammino più breve, cioè trova gli stati obiettivo più superficiali. Ma il cammino più breve non è necessariamente quello a costo minimo.

**Completezza** sì

**Ottimalità** sì, sotto una condizione

Questo algoritmo è ottimo se il costo di cammino è una funzione *monotona non decrescente*
della profondità del nodo, ossia se dati due nodi $n$ e $m$:
$$
\begin{align}
\operatorname{depth}(n)<\operatorname{depth}(m)&\ \Longrightarrow\ \operatorname{cost}(n)\le\operatorname{cost}(m)\\
\operatorname{depth}(n)=\operatorname{depth}(m)&\ \Longrightarrow\ \operatorname{cost}(n)=\operatorname{cost}(m)\\
\end{align}
$$
Se questa condizione è soddisfatta si può effettuare un test obiettivo sul nodo non appena viene generato, invece di effettuarlo dopo che sia estratto dalla frontiera. 
## Ricerca in profondità

## Ricerca in profondità limitata

## Ricerca per approfondimenti successivi