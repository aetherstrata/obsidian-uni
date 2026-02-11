---
tags:
  - reti
  - instradamento
---
Le **VLAN** sono una “virtualizzazione” di una LAN Ethernet: lo stesso insieme di switch fisici viene partizionato in più domini logici separati (tipicamente per sicurezza e per contenere il traffico broadcast), e quando servono più switch viene usato lo standard IEEE **802.1Q** per trasportare più VLAN sullo stesso link tramite tagging.​
## Motivazioni

Avendo un singolo switch che collega gli host di due uffici senza VLAN, tutte le postazioni condividono lo stesso dominio di broadcast e, di fatto, la stessa “LAN logica”.​

![[Pasted image 20260210180942.png]]

Creare due LAN fisicamente separate comprando un secondo switch funziona, ma è rigido e costoso: la virtualizzazione permette di ottenere lo stesso isolamento attraverso una configurazione, senza dover cambiare cablaggio ogni volta che gli host cambiano posto o reparto.​
## Idea

Una VLAN è un **flusso di pacchetti/frame in transito nello switch**, determinato da regole di classificazione applicate **all'ingresso** (_ingress ports_).​

Queste regole possono essere definite:
- solo sulle **porte** (es. e porte 1, 3, 4 e 7 appartengono alla VLAN rossa; le porte 2, 5 e 6 alla VLAN blu e le porte 8 e 9 alla VLAN arancione)
- sulle **porte** e il **contenuto del pacchetto** (es. MAC, EtherType, protocollo L3)

Una volta assegnato a una VLAN, il traffico può uscire solo da un insieme di porte consentite (_egress ports_), e un frame appartiene a una sola VLAN nel momento in cui entra nello switch.​

![[Pasted image 20260210180901.png]]

I pacchetti che entrano in uno switch sono quindi partizionati in VLAN.

>[!important] Un pacchetto può appartenere solo a una VLAN
>Se nello switch non è stata definita alcuna VLAN dall'amministratore, allora tutti i pacchetti transitano nella VLAN di default.

### VLAN ID

Ogni VLAN è identificata da un **VLAN ID**: un intero tra **1 e 4094**, con VLAN di default tipicamente **VID = 1**.
Questo limite nasce dal fatto che 802.1Q usa un campo VLAN ID da 12 bit (con alcuni valori riservati), che porta a 4094 VLAN utilizzabili.
### Routing tra VLAN

Una VLAN è un dominio di livello 2 separato: due host in VLAN diverse non si scambiano frame L2 direttamente (a meno di configurazioni particolari), quindi per comunicare serve un dispositivo di **routing** (router o switch L3) che abbia connettività L3 verso ciascuna VLAN.​  

![[Pasted image 20260210180750.png]]
## Filtering Database

Quando arriva un frame, lo switch esegue le seguenti operazioni: 
1) lo associa alla giusta VLAN, 
2) consulta il **filtering database** (tabella MAC/forwarding) per trovare la porta di uscita, 
3) inoltra oppure fa [[Flooding]] _solo_ sulle porte egress della VLAN se la destinazione è ignota.​

Gli switch possono usare due modalità di apprendimento/lookup MAC:​

- **IVL** (_Independent VLAN Learning_): una tabella MAC separata per VLAN (ogni VLAN ha il suo filtering database).
- **SVL** (_Shared VLAN Learning_): una tabella MAC condivisa tra più VLAN (o tra tutte), utile per pattern più complessi.

## VLAN simmetriche e asimmetriche

### VLAN simmetriche
Le VLAN **simmetriche** sono quelle standard: se una porta è ingress per una VLAN, è anche egress per la stessa VLAN, e l’isolamento è bidirezionale e intuitivo.​ 

### VLAN asimmetriche
Nelle VLAN **asimmetriche** ingress ports ed egress ports possono differire, permettendo di costruire connettività con diverse policy (es. una VLAN server/verde che parla con tutti; VLAN dipartimentali rosso/blu che parlano col verde ma non tra loro).​

In presenza di VLAN asimmetriche è preferibile utilizzare switch in modalità SVL.

>[!important] VLAN come insieme di vincoli
Questa parte è importante perché separa il concetto di VLAN come isolamento dall'idea più generale di VLAN come classificazione e vincoli di uscita, che è più vicina a come operano molti switch enterprise (policy/segmentation).​

## Trunk 802.1Q

Con più switch, una soluzione è quella di collegare fisicamente una VLAN per volta (un link per VLAN).

>[!error] Porte bloccate
Il problema di questo approccio è lo spreco di porte e rischia di introdurre dei **cicli L2** se aumentano i collegamenti. Infatti l'algoritmo [[Spanning Tree Algorithm]] in funzione sugli switch, in presenza di loop, blocca alcune delle porte per ottenere una topologia senza cicli.​

La soluzione standardizzata dall'_IEEE_ è il **trunk 802.1Q**: un singolo link tra switch trasporta traffico di molte VLAN, marcando i frame con un tag VLAN (tagged) quando attraversano il trunk.

Lo switch ricevente usa il tag per assegnare il frame alla VLAN corretta e rimuove il tag quando inoltra verso porte “utente” (access), così gli host finali non devono sapere nulla di 802.1Q.​​
### Tag 802.1Q

Il tag 802.1Q è un'etichetta di **4 byte** inserita nel frame Ethernet, identificata dal valore **TPID = 0x8100** (in posizione EtherType/Length) e seguita da **TCI** che include priorità e VLAN ID (PCP, DEI, VID). 

La dimensione massima del frame passa da **1518** a **1522** byte per accomodare questi 4 byte (estensione storicamente associata a IEEE 802.3ac).​​

>[!info] Struttura dei pacchetti Ethernet
>![[Pasted image 20260211210949.png]]

### Porte access/trunk/ibride

In questo standard sono definiti tre tipi di porte coerenti con la semantica di tagging:​
- **Access**: riceve/invia frame non taggati (tipicamente appartengono a una VLAN “associata alla porta”).​
- **Trunk**: riceve/invia frame taggati (trasporta più VLAN).​
- **Ibrida**: può mescolare tagged e untagged secondo regole/configurazione (concetto vicino alla “native VLAN” o a policy più articolate, a seconda del vendor).​
## Double tagging (802.1ad / Q-in-Q)

Lo standard IEEE **802.1ad** introduce il **double tagging** (Q-in-Q), molto usato dagli ISP per incapsulare VLAN del cliente (C-TAG) dentro una VLAN di servizio (S-TAG) dell’operatore.

Nella descrizione standard, l’S-TAG esterno può usare TPID **0x88A8**, mentre il C-TAG interno usa spesso **0x8100**.
