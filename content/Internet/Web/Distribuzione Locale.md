---
tags:
  - web
  - datacenter
  - reti
  - internet
  - cloud
---
Nel caso di un sistema di distribuzione locale (un singolo IDC), il modello classico di progettazione è il seguente: un URL è esposto con un solo _IP virtuale_ (**VIP**) collegato a un **Load Balancer** che mappa il VIP su server reali.​

Il load balancing può essere eseguito da:
- _hardware_ special purpouse
- _programma_ in esecuzione su un sistema operativo general purpouse.

Il Load Balancer può operare a vari livelli di controllo:
- _Hit_
- _Page Request_
- _Session_
## LB Layer 4

Un **Load Balancer** di livello 4 lavora a livello di connessioni TCP ed è quindi _content-blind_: non conosce il contenuto delle richieste e decide l'instradamento basandosi sugli indirizzi IP di sorgente e destinazione e il numero di porta TCP.

Questo sistema instrada i pacchetti relativi alla stessa connessione allo stesso server e gestisce l’affinità di connessione tramite una tabella connessione → server (binding),  usando SYN/FIN per creare/rimuovere stato.​
### Architettura two-way

Nei **Load Balancer** a due vie i pacchetti transitano attraverso il **LB** sia in entrata che in uscita. 

I pacchetti vengono riscritti dal NAT del Load Balancer, sostituendo:
- **in ingresso**: l'IP virtuale con l'IP del server assegnato
- **in uscita**: l'IP del server assegnato con l'IP virtuale

>[!important] Bisogna ricalcolare il checksum del pacchetto TCP
### Architettura one-way

Nei **Load Balancer** a una via i pacchetti transitano attraverso il **LB** solo in entrata. 
#### Meccanismo basato su IP
Questo metodo richiede che a ogni server sia assegnato un indirizzo IP pubblico.

I pacchetti vengono riscritti dal NAT del Load Balancer, sostituendo in ingresso l'IP virtuale con l'IP del server assegnato. 

Quando il server invia la risposta, non manda i pacchetti al Load Balancer ma li invia direttamente al client, però con l'accortezza di impostare il VIP come indirizzo sorgente.
#### Meccanismo basato su MAC
Il VIP è definito sulle interfacce di loopback di ogni server. Il forwarding dei pacchetti dal Load Balancer è effettuato a livello di MAC.

Utilizzando questo metodo non è necessario riscrivere gli indirizzi nei pacchetti, ma richiede che i server e il Load Balancer facciano parte della stessa sottorete fisica.
### Algoritmi L4
I **Load Balancer** di livello 4 possono usare diversi algoritmi di scheduling, sia statici:
- **Random**: Nessuna informazione sullo stato, assegna le connessioni in maniera casuale
- **Round Robin**: Nessuna informazione sullo stato, conosce solo le precedenti assegnazioni
che dinamici:
- **Partizione dei client**: L'assegnazione viene fatta in base all'indirizzo IP o la porta del client. Questo approccio permette di applicare regole di QoS a determinati gruppi di utenti
- **Least Loaded Server**: L'assegnazione viene fatta in base al carico dei server
- **Weighted Round Robin**: L'assegnazione viene fatta in base a pesi configurati sulla base del carico dei server

Per valutare il carico dei server, il Load Balancer può usare diverse metriche:
- **Numero connessioni TCP**: Osservazione del LB in autonomia, senza cooperare con i server
- **Uso CPU e Disco**: I server inviano queste statistiche al LB
- **Emulazione di richieste**: Misurazioni attive del LB
## LB Layer 7

Un **Load Balancer** di livello 7 è _content-aware_: termina la connessione (anche TLS) per poter ispezionare il contenuto della richiesta HTTP (URL, header, cookie) e quindi fare routing più intelligente, al prezzo di maggiore complessità e costo computazionale.​

Per abbassare il tempo di risposta alla richiesta, il LB può mantenere una pool di connessioni pronte con i server per anticipare le richieste dei client.
### Partizionamento in funzione del contenuto
Questo approccio ha il vantaggio di instradare le richieste verso dei server specializzati per quel tipo di risorse:
- **Tipo di file**: Server specializzati
- **Dimensione dei file**: Task più leggeri
- **Hash dei file**: Ottimizza l'hit rate della cache
### Partizionamento in funzione della sessione
Questo approccio ha il vantaggio di instradare le richieste provenienti dalla stessa sessione verso lo stesso server, aumentando la località e l'hit rate della cache. 

Questo approccio richiede che il client memorizzi un cookie per identificare la stessa sessione tra diverse connessioni.
### Distribuzione locality-aware
All'arrivo di una prima richiesta di una certa risorsa, il Load Balancer la assegna al server con meno carico.

Le richieste successive a quella stessa risorsa verranno sempre instradate verso lo stesso server.

### Distribuzione in funzione della cache
Questo approccio usa un manager centralizzato delle cache locali dei server per poter scegliere il server con la maggiore località.