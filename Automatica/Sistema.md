---
tags:
  - fda
---
Un sistema può essere definito come un unità funzionale, costituita da più sottosistemi in relazione, che contribuiscono allo svolgimento di un fine comune.

```mermaid
flowchart LR
	U["`Ingressi _u(t)_`"] --> X["`Sistema **Σ**`"] --> Y["`Uscite _y(t)_`"]
```

Questi sistemi sono scomposti in un diagramma funzionale che può esprimere le relazioni di causa-effetto necessarie per capire il funzionamento e intervenire sul sistema.

> [!example]- Esempio
> ```mermaid
> ---
> title: Una macchina
> ---
> flowchart LR
> 	A[Acceleratore]
> 	A --> SS --> Ruote
> 	subgraph SS[Sistema]
> 		direction LR
> 		Centralina --> Motore --> Differenziale
> 	end
> ```
## Sistemi statici e dinamici
### Sistema statico

#### Le uscite dipendono dagli ingressi attuali
 Un sistema statico ha un'uscita che non dipende dallo stato del sistema. Dato un ingresso, l'uscita sarà sempre lo stesso valore indipendentemente dal tempo.
$$
y=f(u)
$$
### Sistema dinamico
#### Le uscite dipendono dagli ingressi passati
Un sistema dinamico ha un'uscita che dipende dallo stato del sistema. Per determinare l'uscita del sistema, oltre che l'andamento dell'ingresso, è necessario conoscere le condizioni iniziali. 
$$
g(y,y^{(1)},...,y^{(n)},u,u^{(1)},...,u^{(m)})=0
$$