---
tags:
  - protocollo
  - reti
  - instradamento
---
Il protocollo **IS-IS** (Intermediate System to Intermediate System) è un [[Instradamento#Protocolli|protocollo di instradamento]] che nasce in ambito ISO come protocollo per *CLNS* e viene poi esteso a *IP* come “Integrated IS‑IS” (ISO 10589 e RFC 1195). A differenza di [[OSPF]], IS‑IS opera direttamente a livello 2 (data link), incapsulando i propri PDU sopra CLNS e non richiede IP per il proprio funzionamento, rendendolo agnostico rispetto a IPv4/IPv6.

IS‑IS usa anch'esso il paradigma link‑state con flooding di [[Link State Packet]] e calcolo SPF, ma le informazioni di routing sono codificate tramite TLV (Type‑Length‑Value), che rendono il protocollo molto estensibile (per esempio per [[MPLS]]‑TE, IPv6, segment routing). Questa modularità e l’indipendenza da IP sono uno dei motivi per cui molti provider lo preferiscono nei backbone di grandi dimensioni.​

## Gerarchia

In *IS-IS* la gerarchia è organizzata per livelli (Level‑1 e Level‑2) e che il confine tra aree passa sui link, non sui router, al contrario di OSPF. I router di Level‑1 fanno routing intranet e conoscono solo la topologia della propria area, mentre i router di Level‑2 svolgono il ruolo di backbone per il routing tra aree, mantenendo una visione globale dell’insieme delle aree.

I router L1/L2 partecipano a entrambi i livelli e sono concettualmente simili agli *ABR* [[OSPF]], fungendo da ponte tra area interna e backbone ma con una gerarchia meno rigida (non esiste un singolo “Area 0” obbligatorio). Su reti broadcast IS‑IS non elegge un *DR*/*BDR* come [[OSPF]] ma un *Designated Intermediate System* (DIS), con funzioni analoghe di riduzione della complessità delle adiacenze e gestione del segmento.