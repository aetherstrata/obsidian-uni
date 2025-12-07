---
tags:
  - protocollo
  - reti
  - instradamento
---
Il protocollo **OSPF** (Open Shortest-Path First) è il [[Instradamento#Protocolli|protocollo di instradamento]] IGP più diffuso per IP ed è standardizzato dall'IETF. Questo protocollo fa uso dell'algoritmo [[Link State Packet]] per distribuire le informazioni della rete. 
## Funzionamento

La rete viene suddivisa in aree, con una backbone (Area 0) obbligatoria a cui tutte le altre aree devono essere logicamente collegate, e il routing è distinto in intranet e internet.

Ogni router OSPF appartiene ad una o più aree attraverso le sue interfacce, e il protocollo supporta più tipi di area (normal, stub, NSSA, ecc.) per controllare la quantità di informazioni di routing e il tipo di pacchetti LSA propagati. OSPF usa IP protocol 89 per incapsulare i pacchetti, richiede connettività IP tra router adiacenti e supporta autenticazione (plain o MD5) sui messaggi per proteggere la formazione delle adiacenze.
### Ruoli

I router all'interno di una rete OSPF possono svolgere quattro ruoli principali:
- *internal* - router interni a un’area non backbone
- *backbone* - router che hanno almeno un'interfaccia sull'area 0
- *area border* - router che hanno interfacce in più aree
- *AS boundary* - router che collega la rete OSPF ad altri [[Autonomous System|AS]]

Gli **ABR** riassumono e propagano prefissi tra aree, mentre gli **ASBR** importano rotte esterne nell'AS OSPF.
### Designated Router

OSPF forma adiacenze usando pacchetti Hello, in cui si negoziano parametri come area ID, timer, tipo di area, autenticazione e priorità per l’elezione del *Designated Router* (DR) su reti multi‑accesso come Ethernet. In reti broadcast viene eletto un DR e un Backup DR per ridurre il numero di relazioni full‑mesh, mentre in point‑to‑point non è necessario DR/BDR, semplificando la topologia logica.
