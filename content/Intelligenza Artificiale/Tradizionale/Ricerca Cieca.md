---
tags:
  - algoritmi
  - ai
---
Un algoritmo di **ricerca cieca** (non informata) non riceve *nessuna informazione* su quanto uno stato sia vicino all'obiettivo.
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

#### Esempio

```python
import collections

def breadth_first_search(graph, start_node, goal_node):
  """
  Esegui una ricerca in ampiezza per trovare il percorso più breve dal nodo
  iniziale al nodo obiettivo.

  Args:
    graph: Una mappa che rappresenta la lista di adiacenza del grafo.
    start_node: Lo stato di partenza.
    goal_node: Lo stato obiettivo.

  Returns:
    Una lista rappresentante il percorso dal nodo iniziale al nodo finale, 
    o None se non esiste un percorso valido.
  """
  # Una coda per i nodi da visitare (la frontiera)
  fringe = collections.deque([start_node])
  # Un set per tenere traccia dei nodi già visitati.
  visited = {start_node}
  # Una mappa per ricostruire il percorso dallo stato iniziale 
  # allo stato obiettivo.
  parent = {start_node: None}

  while fringe:
    # Prendi il primo nodo della coda
    current_node = fringe.popleft()

    # Se il nodo corrente è obiettivo, ritorna il cammino
    if current_node == goal_node:
      path = []
      while current_node is not None:
        path.append(current_node)
        current_node = parent[current_node]
      return path[::-1]  # Ritorna il percorso inverso

    # Espandi il nodo e metti i figli nella coda
    for neighbor in graph.get(current_node, []):
      if neighbor not in visited:
        visited.add(neighbor)
        parent[neighbor] = current_node
        fringe.append(neighbor)

  # Se la coda è vuota allora l'obiettivo è irraggiungibile
  return None
```
## Ricerca in profondità

La **ricerca in profondità** espande sempre per primo il nodo a profondità maggiore nella frontiera.

Si procede
- espandendo il nodo radice
- espandendo sempre per primo il nodo più profondo nella frontiera dell'albero di ricerca

Questa ricerca si può implementare con una funzione ricorsiva, usando lo stack di attivazione come lista di nodi. Quando si espande un nodo non obiettivo e senza figli si fa **backtracking**, cioè si torna indietro no all'ultimo nodo in cui è possibile effettuare una scelta.

**Completezza** no

**Ottimale** no, in quanto esplora completamente un percorso, incorrendo così nella possibilità di trovare una soluzione più costosa di un'altra.

**Complessità temporale** $O(b^m)$
- $b$ = fattore di ramificazione dell'albero
- $m$ = profondità massima dell'albero di ricerca

**Complessità spaziale** $O(b\cdot m)$
Si deve memorizzare solo un cammino radice-foglia e i fratelli non espansi di ciascun nodo del cammino.
#### Esempio
```python
Grafo = dict[str, list[str]] # Dizionario: nodo -> lista di nodi adiacenti

def dfs_path(graph: Grafo, start: str, goal: str, path=[], visited=set()):
	
	# Aggiungi il nodo corrente al percorso e all'insieme dei visitati
	path.append(start)
	visited.add(start)
	
	if start == goal: # Obiettivo raggiunto, ritorna il percorso attuale
		return list(path)
	
	# Esplora i nodi adiacenti
	for neighbor in graph[start]:
		if neighbor not in visited: # Nodo non ancora visitato
			result = dfs_path(graph, neighbor, goal, path, visited)
			if result is not None: # Percorso trovato
				return result
	
	# Se non si trova il percorso, rimuovi il nodo corrente dal percorso
	path.pop()
	return None

# Esempio di grafo come dizionario di liste
graph: Grafo = {
	'A': ['B', 'C'],
	'B': ['A', 'D', 'E'],
	'C': ['A', 'F'],
	'D': ['B'],
	'E': ['B', 'F'],
	'F': ['C', 'E']
}

percorso = dfs_path(graph, 'A', 'F')
print("Percorso da 'A' a 'F':", percorso)
```
## Algoritmo di Dijkstra

Anche chiamato ricerca a *costo uniforme*, questo algoritmo di ricerca si contraddistingue dal fatto che, dato il costo del cammino dalla radice a $n$, detto $g(n)$, viene scelto per l'espansione il nodo che ha il costo $g(n)$ minore.

>[!tip] Caso particolare
>Se il costo $g(n)$ è uguale alla profondità $\operatorname{depth}(n)$ allora l'algoritmo si comporta come una [[Ricerca Cieca#Ricerca in ampiezza|Ricerca in ampiezza]]. 

Questo algoritmo è completo e ottimale se il costo di ogni step $g(\operatorname{next}(n)) - g(n)$ è
sempre maggiore o uguale a una costante positiva $\varepsilon$.

**Completezza** sì

**Ottimalità** sì, purché tutti i costi degli archi siano non negativi.
#### Esempio
```python
import heapq

max_int = 2**31 - 1

ListaAdiacenza = list[tuple[int, int]] # Lista di tuple (nodo_vicino, peso)
Grafo = dict[int, ListaAdiacenza] # Dizionario: nodo -> lista di adiacenza

def dijkstra(graph: Grafo, source: int, goal: int):
	
	# Imposta le distanze iniziali al massimo
	dist = {node: max_int for node in graph}
	# Imposta il predecessore di ogni nodo a -1 (nessuno)
	prev = {node: -1 for node in graph}
	# La distanza del nodo sorgente a se stesso è 0
	dist[source] = 0
	# Coda di priorità per i nodi da esplorare
	heap = [(0, source)]
	  
	while heap:
		# Estrai il nodo con la distanza minima
		curr_dist, curr_node = heapq.heappop(heap)
		
		if curr_node == goal: # Obiettivo raggiunto, termina la ricerca
			break
		
		if curr_dist > dist[curr_node]: # Nodo già elaborato con distanza minore
			continue
		
		for node, weight in graph[curr_node]:
			# Se troviamo un percorso più breve verso il nodo vicino
			if dist[curr_node] + weight < dist[node]
				# Aggiorna la distanza e il predecessore
				dist[node] = dist[curr_node] + weight
				prev[node] = curr_node
				heapq.heappush(heap, (dist[node], node))
	
	# Ricostruisci il percorso da source a goal
	path = []
	curr = goal
	while curr >= 0:
		path.append(curr)
		curr = prev[curr]
	path.reverse()
	
	return dist[goal], path
	
# Esempio di grafo come dizionario di adiacenza 
graph = {     
	0: [(1, 10), (2, 3)],    
	1: [(2, 1), (3, 2)],    
	2: [(1, 4), (3, 8), (4, 2)],    
	3: [(4, 7)],    
	4: [(3, 9)] 
}

costo, percorso = dijkstra(graph, 0, 4) 
print(f"Costo minimo dal nodo 0 al nodo 4: {costo}") 
print(f"Percorso: {percorso}")
```
## Ricerca in profondità limitata

Opera come la [[Ricerca Cieca#Ricerca in profondità|ricerca in profondità]] imponendo, però, un limite alla profondità massima dei cammini: un nodo viene espanso solo se la lunghezza del cammino corrispondente è minore del massimo stabilito.

**Ottimalità** no

**Completezza** sì, se il problema ha una soluzione di lunghezza minore o uguale al limite massimo.

**Complessità temporale** $O(b^l)$
- $b$ = fattore di ramificazione dell'albero
- $l$ = limite di profondità di ricerca fissato

**Complessità spaziale** $O(b\cdot l)$
Si deve memorizzare solo un cammino radice-foglia e i fratelli non espansi di ciascun nodo del cammino di lunghezza massima $l$.\

#### Esempio
```python
Grafo = dict[str, list[str]] # Dizionario: nodo -> lista di nodi adiacenti

def depth_limited_search(graph, start, goal, limit):
	# Funzione ricorsiva della ricerca limitata in profondità
	def recursive_dls(node, depth, path=[], visited=set()):
		
		if depth < 0: # Superamento del limite di profondità
			return None
			
		# Aggiungi il nodo corrente al percorso
		path.append(node)
		visited.add(node)
		
		if node == goal: # Obiettivo raggiunto, ritorna il percorso attuale
			return list(path)
		
		# Esplora i nodi adiacenti
		for neighbor in graph[node]:
			if neighbor not in visited: # Nodo non ancora visitato
			result = recursive_dls(neighbor, depth - 1, path, visited)
				if result is not None: # Obiettivo trovato, ritorna il percorso
					return result
		
		# Rimuovi il nodo corrente dal percorso prima di tornare indietro
		path.pop()
		return None

return recursive_dls(start, limit)

# Esempio di grafo come dizionario di liste
graph: Grafo = {
	'A': ['B', 'C'],
	'B': ['A', 'D', 'E'],
	'C': ['A', 'F'],
	'D': ['B'],
	'E': ['B', 'F'],
	'F': ['C', 'E']
}

limite = 2
percorso = depth_limited_search(graph, 'A', 'F', limite)
print(f"Percorso da 'A' a 'F' entro profondità {limite}: {percorso}")
```
## Ricerca per approfondimenti successivi

Anche detto *iterative deepening search*, questo algoritmo evita il problema della scelta di un limite provando iterativamente tutti i limiti possibili, in quanto non sempre si conosce un limite adeguato per la ricerca in profondità limitata.

Combina i benefici della [[Ricerca Cieca#Ricerca in profondità|ricerca in profondità]] con quelli della [[Ricerca Cieca#Ricerca in ampiezza|ricerca in ampiezza]].

**Completezza** sì

**Ottimalità** sì

**Complessità temporale** $O(b^d)$
- Nodi livello $1$ $(b)$ generati $d$ volte
- Nodi livello $2$ $(b^2)$ generati $d-1$ volte
- Nodi livello $3$ $(b^3)$ generati $d-2$ volte
- $\hphantom{+}\dots\hphantom{+}$
- Nodi livello $d$ $(b^d)$ generati $1$ volta

Numero di nodi generati:
$$
b\cdot d + b^2\cdot(d-1) + \dots + b^{d-1}\cdot d + b^d\cdot 1
$$

**Complessità spaziale** $O(b\cdot d)$
- $b$ = fattore di ramificazione dell'albero
- $d$ = profondità minima di una soluzione
#### Esempio

```python
Grafo = dict[str, list[str]] # Dizionario: nodo -> lista di nodi adiacenti

def depth_limited_search(graph: Grafo, start: str, goal: str, limit: int):
	# Funzione ricorsiva della ricerca limitata in profondità
	def recursive_dls(node: str, depth: int, path=[], visited=set()):
		
		if depth < 0: # Il limite di profondità è stato superato
			return None
		
		# Aggiungi il nodo corrente al percorso e al set dei visitati
		path.append(node)
		visited.add(node)
		
		if node == goal: # Obiettivo raggiunto, ritorna il percorso
			return list(path)
		
		# Esplora i nodi adiacenti
		for neighbor in graph[node]:
			if neighbor not in visited: # Nodo non ancora visitato
				result = recursive_dls(neighbor, depth - 1, path, visited)
				if result is not None:
					return result
		
		# Rimuovi il nodo corrente dal percorso prima di tornare indietro
		path.pop()
		return None
	return recursive_dls(start, limit)

def iterative_deepening_search(graph, start, goal, max_depth):
	# Esegui DLS con profondità crescente fino a max_depth
	for depth in range(max_depth + 1):
		path = depth_limited_search(graph, start, goal, depth)
		if path is not None: # Se trovi un percorso, restituiscilo
			return path
	return None

# Esempio di grafo come dizionario di liste
graph = {
	'A': ['B', 'C'],
	'B': ['A', 'D', 'E'],
	'C': ['A', 'F'],
	'D': ['B'],
	'E': ['B', 'F'],
	'F': ['C', 'E']
}

max_depth = 4
percorso = iterative_deepening_search(graph, 'A', 'F', max_depth)
print(f"Percorso da 'A' a 'F' con IDDFS: {percorso}")
```