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
\begin{align*}
\operatorname{depth}(n)<\operatorname{depth}(m)&\ \Longrightarrow\ \operatorname{cost}(n)\le\operatorname{cost}(m)\\
\operatorname{depth}(n)=\operatorname{depth}(m)&\ \Longrightarrow\ \operatorname{cost}(n)=\operatorname{cost}(m)\\
\end{align*}
$$
Se questa condizione è soddisfatta si può effettuare un test obiettivo sul nodo non appena viene generato, invece di effettuarlo dopo che sia estratto dalla frontiera. 

**Complessità temporale** $O(b^d)$ 
- $b$ = fattore di ramificazione (numero massimo di figli di un nodo)
- $d$ = lunghezza minima di un cammino dal nodo iniziale alla soluzione

Il numero massimo di nodi da generare prima di trovare una soluzione è 
$$
\begin{gather*}
\left(\text{nodi livello 1}\right) + \left(\text{nodi livello 2}\right) + \dots +\left(\text{nodi livello d}\right) + \left(\text{nodi livello d+1}\right) - b\\
O\!\left(1+b+b^2+b^3+\dots+b^d+(b^{d+1}-b)\right) = O\!\left(b^d\right)
\end{gather*}
$$
**Complessità spaziale** $O(b^d)$
Tutte le foglie dell'albero devono essere conservate in memoria.

```python
import collections

def breadth_first_search(graph, start_node, goal_node):
  """
  Performs a Breadth-First Search to find the shortest path from a start node to a goal node.

  Args:
    graph: A dictionary representing the graph's adjacency list.
    start_node: The starting node for the search.
    goal_node: The target node to find.

  Returns:
    A list representing the path from the start node to the goal node, or None if no path is found.
  """
  # fringe ← MAKE-QUEUE(MAKE-NODE(INITIAL-STATE[problem])
  # A queue for the nodes to visit (the "fringe")
  fringe = collections.deque([start_node])
  # A set to keep track of visited nodes to avoid cycles and redundant processing.
  visited = {start_node}
  # A dictionary to reconstruct the path from the goal back to the start.
  parent = {start_node: None}

  # loop do
  while fringe:
    # node ← REMOVE-FRONT(fringe)
    current_node = fringe.popleft()

    # if GOAL-TEST(problem, STATE[node]) then return SOLUTION(node)
    if current_node == goal_node:
      path = []
      while current_node is not None:
        path.append(current_node)
        current_node = parent[current_node]
      return path[::-1]  # Return reversed path

    # fringe ⟵ ENQUEUE-AT-END(fringe,EXPAND(node,OPERATORS(problem)))
    for neighbor in graph.get(current_node, []):
      if neighbor not in visited:
        visited.add(neighbor)
        parent[neighbor] = current_node
        fringe.append(neighbor)

  # If EMPTY?(fringe) then return failure
  return None # Goal not reachable
```
## Ricerca in profondità

## Ricerca in profondità limitata

## Ricerca per approfondimenti successivi