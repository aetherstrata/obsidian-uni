---
tags:
  - protocollo
  - reti
  - datacenter
---
Il protocollo **MPLS** (_Multi Protocol Label Switching_) **L3VPN** (BGP/MPLS IP VPN) è stato sviluppato come soluzione carrier-grade al problema di connettere sedi geograficamente distribuite in un’unica rete IP privata, mantenendo indirizzamento, isolamento tra clienti e _QoS_, ma senza far esplodere la complessità di configurazione nel _backbone_. 

L’idea chiave è separare il control-plane dal data-plane: il core del provider trasporta etichette MPLS e resta “ignorante” delle rotte dei clienti, che vivono ai bordi (Provider Edge) dentro **VRF** (_Virtual Routing and Forwarding_) e vengono scambiate con MP-BGP.​

>[!info] Terminologia
>- **P**: Router del provider _core_
>- **PE**: Router del provider _sul bordo della rete_, parla con il cliente
>- **CE**: Router del cliente
## Motivazioni e vincoli

Il problema tipico che si vuole risolvere è un’organizzazione che vorrebbe dei collegamenti privati tra sedi (stessa rete IP, come se fosse una grande LAN/WAN proprietaria), mentre il provider vuole vendere dei collegamenti virtuali, sfruttando la sua backbone condivisa per più clienti. 

I vincoli del cliente (indirizzi invariati, isolamento del traffico, _QoS_) e quelli del provider (bassi costi operativi, nessuna penalizzazione prestazionale, scalabilità che non dipenda dal numero di VPN/siti) spingono verso un modello in cui la complessità resta ai bordi e il core rimane semplice.
### Etichette e incapsulamento

**MPLS** incapsula i pacchetti inserendo un header con una o più etichette (label stack) e campi come Traffic Class (QoS/ECN), Bottom-of-Stack e TTL. Dal punto di vista architetturale, MPLS è pensato per l'inoltro basato su label (lookup esatto) e per costruire percorsi commutati (_LSP_) sopra una rete che comunque continua ad avere routing IP sottostante. Questa combinazione è importante: l’IP decide l'instradamento, **MPLS** rende l’inoltro nel core efficiente e scalabile perché evita di popolare il backbone con tutte le rotte dei clienti.
## Funzionamento

 Il processo di configurazione di una VPN MPLS si può organizzare in tre step principali (mate-in-3):​
1) **IP reachability tra PE**: Si assegnano _loopback_ ai PE e si garantisce raggiungibilità IP tra queste loopback con un IGP ([[OSPF]]/[[IS-IS]] o anche statiche, ma con meno resilienza). Questo crea la base: prima ancora di parlare di VPN, i router del provider devono sapere come raggiungere le loopback degli edge router (_PE_).​
2) **distribuzione rotte VPN con MP-BGP**: Le reti dei clienti vengono annunciate tra i PE con MP-BGP (tipicamente iBGP, spesso con [[Route Reflectors]] per scalare). Qui nasce la separazione logica: rotte di clienti diversi non devono entrare in conflitto, anche se usano gli stessi prefissi privati.​​
3) **tunnel MPLS nel backbone**: I pacchetti del cliente, arrivati al PE di ingresso, vengono incapsulati in MPLS e attraversano il backbone dentro un envelope fino al PE di uscita, lasciando il core a fare solo label switching.

>[!important] VRF - Virtual Routing and Forwarding
>Il punto fondamentale è che i PE mantengono più tabelle di routing/forwarding separate (**VRF**) per distinguere i clienti e gestire anche spazi di indirizzamento sovrapposti.
>
>Questo è coerente con il “peer model” descritto per le VPN IP BGP/MPLS: i CE non vedono overlay complessi tra siti, parlano col PE, e il backbone trasporta i dati incapsulati senza conoscere le rotte della VPN.​
### Route Distinguishers

Il **Route Distinguisher (RD)** serve a rendere univoci prefissi potenzialmente sovrapposti, creando identificativi RD + IPv4. 

RFC 4364 chiarisce che l’RD è _solo un valore_: il suo scopo è la disambiguazione, non decide a quali VPN distribuire una rotta.​​ 

>[!error] Il fallimento dell'associazione tra RD e VPN
>I Route Distinguisher aiutano ad evitare ambiguità tra reti private con gli stessi prefissi. Il problema di associare un RD a una VPN è il fatto che dei siti possano essere collegati tra loro con una VPN la cui _la topologia non sia una rete mesh_.
>
>Se si prova ad usare un RD come identificatore di una VPN, allora tutti siti che importano quella VPN finirebbero per importare **tutte** quelle rotte.
>
>Questo implica una topologia **full-mesh di connettività IP** tra i siti della VPN: ogni _PE_ che partecipa a quella VPN (cioè che importa quelle rotte) vedrà le rotte di tutti gli altri e potrà inoltrare verso tutti.
>
>In una topologia non-mesh, il comportamento voluto di solito è che:
>- certe rotte siano visibili solo tra alcuni siti (es. spoke → hub sì, spoke → spoke no), oppure
>- lo stesso prefisso possa essere raggiungibile in modi diversi a seconda del contesto (intranet vs extranet).
>    
>Con il solo RD non c'è un meccanismo per filtrare le rotte da importare, perché l'RD non porta semantica di import/export: RFC 4364 dice esplicitamente: “other means are used to determine where to redistribute the route” (cioè non l'RD).

### Route Target

Per controllare _chi importa/esporta cosa_ si usano i **Route Target (RT)**, che sono [[BGP Community|extended communities BGP]] e permettono topologie non banali (non solo full-mesh tra tutti i siti) tramite regole di import/export nelle _VRF_. 

Questo è esattamente il mezzo alternativo (rispetto all'_RD_) con cui RFC 4364 governa la redistribuzione delle rotte tra _PE_ e _VRF_.​​
### Penultimate Hop Popping

Nel modello L3VPN tipico, il _PE_ di ingresso impone **due label**: una esterna per attraversare l’LSP nel core (label di trasporto) e una interna che identifica la VPN/VRF al PE di uscita (label VPN). I router _P_ fanno swapping della label esterna a ogni hop, e spesso il penultimo router fa **pop** della label esterna (_PHP_), così il PE di egress riceve direttamente la label VPN come top label e può consegnare il pacchetto alla _VRF_ corretta. 

Dal punto di vista standard, l'idea di label stack e di forwarding basato su label è parte della logica MPLS: il controllo di queste label e del loro significato deve essere coerente tra **LSR** (_Label Switching Routers_) adiacenti. 
### Label Switching Protocol

Il protocollo **LDP** (_Label Distribution Protocol_) è usato come risposta al problema di stabilire chi deve costruire il data-plane di label swapping verso le loopback dei PE. 

RFC 5036 descrive **LDP** come protocollo con cui gli **LSR** (_Label Switching Routers_) si scambiano **label bindings** per supportare MPLS forwarding lungo i percorsi normalmente determinati dall'IP routing (cioè importati dal piano IP).

In altre parole l'IGP calcola i next-hop IP verso le loopback, e LDP distribuisce le label necessarie perché lo stesso cammino possa essere percorso in modalità MPLS (LSP hop-by-hop).

>[!tip] Aspetti operativi
>- **Label space**: alcune piattaforme usano label space per-router (per-platform), altre per-interfaccia; questo impatta debugging e interpretazione delle tabelle MPLS/LFIB.​
>- **MTU**: ogni label aggiunge 4 byte; con due label il rischio è superare l’MTU e frammentare (o far fallire PMTU), con impatto prestazionale. In rete reale spesso si alza l’MTU in core (jumbo o “MPLS-friendly MTU”) proprio per evitare questi problemi.​
>- **TTL**: i router LSR non vedono l’IP TTL mentre il pacchetto è incapsulato, quindi:
>	- MPLS usa il proprio TTL nell’etichetta 
>	- il PE copia il TTL IP nel TTL MPLS all’ingresso
>	- i P decrementano il TTL nell'etichetta
>	- quando si rimuovono le label, i valori vengono propagati fino a ripristinare il TTL IP.
>Questo previene loop infiniti anche nel dominio MPLS.​
>- **QoS**: Traffic Class/EXP - la marcatura IP ToS/DSCP può essere copiata nel campo QoS dell’etichetta per applicare politiche nel backbone, dove il payload IP non è il criterio principale di forwarding.​

## Internet access e route leaking

Molti clienti vogliono sia connettività privata tra sedi sia uscita Internet, e RFC 4364 descrive più modi di realizzarla:
- default route dal CE, oppure
- leaking di rotte Internet dentro le VRF, oppure
- import di una 0/0 associata a specifici RT. 

Il punto architetturale è che l'Internet routing può essere trattato come un servizio importabile in VRF con policy BGP (RT), senza rompere l’isolamento tra clienti.