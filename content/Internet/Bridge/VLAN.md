---
tags:
  - reti
  - instradamento
---
Le **VLAN** sono una “virtualizzazione” di una LAN Ethernet: lo stesso insieme di switch fisici viene partizionato in più domini logici separati (tipicamente per sicurezza e per contenere il traffico broadcast), e quando servono più switch si usa IEEE **802.1Q** per trasportare più VLAN sullo stesso link tramite tagging.​
## Motivazioni: sicurezza e scalabilità del broadcast

Nell’esempio iniziale del data center, con un singolo switch che collega host di due uffici, senza VLAN tutti condividono lo stesso dominio di broadcast e, di fatto, la stessa “LAN logica”.​  
Creare due LAN fisicamente separate comprando un secondo switch funziona, ma è rigido e costoso: la virtualizzazione permette di ottenere lo stesso isolamento con configurazione, senza cambiare cablaggio ogni volta che gli host cambiano posto o reparto.​

## “Semantica” di VLAN: classificazione ingress/egress

Le slide propongono una definizione operativa: una VLAN è un **insieme di pacchetti/frame in transito nello switch**, determinato da regole di classificazione applicate **all’ingresso** (ingress).​  
In uno scenario semplice, la VLAN dipende solo dalla porta (port-based VLAN), ma può dipendere anche da campi del frame/pacchetto (es. MAC, EtherType, protocollo L3), a seconda del vendor e del modello di switch.​  
Una volta assegnato a una VLAN, il traffico può uscire solo da un insieme di porte consentite (**egress ports**), e un frame appartiene a una sola VLAN nel momento in cui entra nello switch.​

## VLAN ID: spazio di numerazione e default

Ogni VLAN è identificata da un **VLAN ID**; nel materiale: un intero tra **1 e 4094**, con VLAN di default tipicamente **VID = 1**.
Questo limite nasce dal fatto che 802.1Q usa un campo VLAN ID da 12 bit (con alcuni valori riservati), che porta a 4094 VLAN utilizzabili.
## VLAN e livello 3: perché serve il routing tra VLAN

Una VLAN è un dominio L2 separato: due host in VLAN diverse non si scambiano frame L2 direttamente (a meno di configurazioni particolari), quindi per comunicare serve un dispositivo di **routing** (router o switch L3) che abbia connettività L3 verso ciascuna VLAN.​  
Nell’esempio “VLAN e livello 3” si vede infatti un router con più interfacce/subnet per instradare tra i prefissi associati alle VLAN.​

## Forwarding nello switch: Filtering Database e modalità IVL/SVL

Quando arriva un frame, lo switch: (1) lo associa alla VLAN, (2) consulta il **filtering database** (tabella MAC/forwarding) per trovare la porta di uscita, (3) inoltra oppure fa flooding _solo_ sulle porte egress della VLAN se la destinazione è ignota.​

Le slide distinguono due modalità di apprendimento/lookup MAC:​

- **IVL (Independent VLAN Learning)**: una tabella MAC separata per VLAN (in pratica, ogni VLAN ha il suo “FDB/FID”).
- **SVL (Shared VLAN Learning)**: una tabella MAC condivisa tra più VLAN (o tra tutte), utile per pattern più complessi.

Questo si collega bene alle VLAN **asimmetriche** discusse nel file: se un host B deve essere raggiungibile da traffico classificato in una VLAN diversa da quella “naturale” di B, con IVL potresti non trovare il MAC nella tabella della VLAN di ingresso e causare flooding inutile; con SVL, invece, puoi trovare la porta corretta senza degradare le prestazioni.​​

## VLAN simmetriche vs asimmetriche

Le VLAN **simmetriche** sono quelle “standard” da manuale: se una porta è ingress per una VLAN, di solito è anche egress per la stessa VLAN, e l’isolamento è bidirezionale e intuitivo.​  
Le VLAN **asimmetriche** generalizzano: ingress ports ed egress ports possono differire, così puoi costruire connettività “a politica” (es. una VLAN server/verde che parla con tutti; VLAN dipartimentali rosso/blu che parlano col verde ma non tra loro).​  
Questa parte è didatticamente utile perché separa il concetto “VLAN = isolamento” dall’idea più generale “VLAN = classificazione + vincoli di uscita”, che è più vicina a come ragionano molti switch enterprise (policy/segmentation).​

## VLAN su più switch: il problema e la soluzione 802.1Q trunk

Con più switch, se provi a collegare fisicamente una VLAN per volta (un link per VLAN), sprechi porte e rischi di introdurre **cicli L2** se raddoppi i collegamenti; infatti il file richiama STP (802.1D) che, in presenza di loop, blocca porte per ottenere una topologia senza cicli.​  
La soluzione scalabile è il **trunk IEEE 802.1Q**: un singolo link tra switch trasporta traffico di molte VLAN, marcando i frame con un tag VLAN (tagged) quando attraversano il trunk.
Lo switch ricevente usa il tag per assegnare il frame alla VLAN corretta e in genere rimuove il tag quando inoltra verso porte “utente” (access), così gli host finali non devono sapere nulla di 802.1Q.​​

## Tag 802.1Q: dove sta nel frame e cosa contiene

Il tag 802.1Q è un’aggiunta di **4 byte** inserita nel frame Ethernet, identificata dal valore **TPID = 0x8100** (in posizione EtherType/Length) e seguita da **TCI** che include priorità e VLAN ID (PCP, DEI, VID).
Le slide notano anche l’effetto pratico: la dimensione massima del frame passa da **1518 a 1522 byte** per accomodare questi 4 byte (estensione storicamente associata a IEEE 802.3ac).​​

## Porte access/trunk/ibride (semantica operativa)

Il materiale definisce tre “stili” di porta coerenti con la semantica di tagging:​

- **Access**: riceve/invia frame non taggati (tipicamente appartengono a una VLAN “associata alla porta”).​
- **Trunk**: riceve/invia frame taggati (trasporta più VLAN).​
- **Ibrida**: può mescolare tagged e untagged secondo regole/configurazione (concetto vicino alla “native VLAN” o a policy più articolate, a seconda del vendor).​

## Double tagging (802.1ad / Q-in-Q)

Le slide citano IEEE **802.1ad**: introduce il **double tagging** (Q-in-Q), molto usato dagli ISP per incapsulare VLAN del cliente (C-TAG) dentro una VLAN di servizio (S-TAG) dell’operatore.

Nella descrizione standard, l’S-TAG esterno può usare TPID **0x88A8**, mentre il C-TAG interno usa spesso **0x8100**.