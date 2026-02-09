---
tags:
  - algoritmi
  - reti
---
Lo **Spanning Tree Algorithm/Protocol (STP, IEEE 802.1D)** è il meccanismo con cui una LAN con switch/bridge e collegamenti ridondanti elimina i cicli a livello 2 senza rinunciare alla ridondanza: calcola un albero ricoprente e mette alcune porte in _blocking_ (pronte però a riattivarsi in caso di guasto).​

>[!important] Assunzioni
>Questo algoritmo assume che tutti gli switch compilino automaticamente le proprie tabelle d'instradamento facendo [[Backward Learning]].
## Il problema

In Ethernet (bridging di livello 2), gli switch imparano dove si trovano i MAC address osservando l’indirizzo sorgente dei frame in ingresso ([[backward learning]]) e associandolo alla porta d’arrivo.​  

Se nella topologia della rete sono presenti dei cicli, frame con lo stesso MAC possono arrivare allo switch da porte diverse (instabilità della tabella MAC) e i frame di broadcast o unknown-unicast possono circolare all'infinito (loop di livello 2), saturando la rete. 

Il problema è che non si può rinunciare alla presenza di cicli nella rete in quanto si perderebbe ogni tipo di ridondanza dei collegamenti.

>[!info] Soluzione
>**STP** risolve il problema costruendo una topologia logica senza cicli (un albero) pur mantenendo i link fisici ridondanti come backup.
## Obiettivi e requisiti

L'algoritmo deve essere distribuito e robusto, evitare cicli non solo a regime ma anche durante i periodi transitori, e consumare poca banda tramite messaggi di controllo (_BPDU_).​

L’amministratore può influenzare l’esito del calcolo modificando opportuni parametri di configurazione.
### Requisiti

- **Robustezza**: l’albero ricoprente deve essere computato in maniera distribuita
- **Efficacia**: la formazione di cicli deve essere esclusa sia in condizioni stazionarie che transitorie
- **Efficienza**: lo spanning tree calcolato deve corrispondere ad un uso efficiente delle risorse di rete disponibili
- **Banda**: pacchetti scambiati dai bridge (_BPDU_) devono comportare un limitato consumo di banda
- **Tempo**: la computazione dell’albero deve essere più veloce possibile per limitare il disservizio (tipicamente 30-40 secondi sono sufficienti allo _STA_ per convergere)
- **Flessibilità**: l’amministratore deve poter assegnare priorità a ciascun bridge e a ciascuna porta per influire sull'output dell’algoritmo
- **Facilità d’uso**: l’algoritmo deve essere in grado di funzionare correttamente anche in assenza di configurazione dell’amministratore

>[!warning] Tempo di convergenza
>Un punto pratico molto importante è il **tempo di convergenza**: in 802.1D classico la transizione delle porte verso l’inoltro può richiedere fino a ~50s con i timer di default (Max Age 20s + Forward Delay 15s in Listening + 15s in Learning), e richiede decine di secondi per stabilizzarsi.
## Parametri di input
- Ogni bridge/switch ha un **Bridge ID** (priorità + MAC), in cui il valore più basso significa più propenso ai fini dell’elezione. Questo ID è lungo _8 byte_ ed è formato come segue:
	- 2 byte per scegliere la priorità
	- 6 byte corrispondenti al MAC address
- Ogni porta ha un **Port ID** (priorità + numero porta) usato come tie-breaker quando altre metriche sono uguali. Questo ID è lungo _2 byte_ ed è formato come segue:
	- 1 byte per scegliere la priorità
	- 1 byte corrispondente al numero della porta sul bridge
- Ogni LAN ha un **costo** associato in base a quanto è oneroso attraversare quel collegamento. 
	- Una classica convenzione è quella in cui link più veloci hanno costo minore (es. $10\ \text{Mb}/s \rightarrow 100$, $100\ \text{Mb}/s \rightarrow 10$, $1\ \text{Gb}/s \rightarrow 1$)
	- L’amministratore può modificarli per influenzare il risultato.​
## Processo di decisione

Lo **STP** funziona scambiando **BPDU** (_Bridge Protocol Data Unit_), in particolare _Configuration BPDU_ e _Topology Change Notification_.

A livello Ethernet, le BPDU sono inviate a un MAC multicast riservato (**01:80:C2:00:00:00**) e usano frame LLC con **DSAP/SSAP = 0x42**, così gli switch le riconoscono come traffico STP.​  

>[!info] Configuration BPDU
>Un pacchetto **Configuration BPDU** contiene in particolare le seguenti informazioni:
>- _root-bridge-identifier_: l'attuale root dello spanning tree
>- _root-path-cost_: il costo del cammino verso il root bridge
>- _bridge-identifier_: l'id del bridge che invia questa bpdu
>- _port-identifier_: la porta da cui è uscita questa bpdu
>
>Questi valori  servono ai bridge per prendere decisioni locali coerenti senza conoscere l’intera rete.

### 1. Elezione del Root Bridge

All'avvio, ogni switch si propone come root e annuncia il proprio Bridge ID come Root ID nelle BPDU; vince il bridge con **Bridge ID più basso**, che diventa il punto di riferimento (radice) dell’albero.​

Da quel momento, gli altri smettono di annunciare sé stessi come root e propagano l’informazione del root migliore che hanno appreso.​​

>[!tip] Inoltro delle Configuration BPDU
>- Quando il pacchetto è prodotto dal Root Bridge, il _root-path-cost_ è messo a _0_.
>- Quando il pacchetto _NON_ è prodotto dal Root Bridge, i valori dei suoi campi vengono aggiornati come segue:
>	- _root-path-cost_: incrementato con costo del collegamento che ha ricevuto il pacchetto
>	- _bridge-identifier_: sostituito con il Bridge ID del bridge corrente
>	- _port-identifier_: sostituito con il Port ID della porta da cui verrà inviato il pacchetto

### 2. Scelta della Root Port

Ogni switch non-root deve scegliere **una sola porta** migliore che lo collega al root: la **Root Port**.​

La scelta segue una priorità lessicografica:
1. costo totale verso il root (_root-path-cost_ + costo porta ricevente) minimo
2. _bridge-identifier_ più basso
3. _port-identifier_ del mittente più basso
4. _port-identifier_ del ricevente più basso
### 3. Scelta delle Designated Port

Per ogni dominio di collisione serve una porta che rappresenti quel segmento nello spanning tree: la **Designated Port**, cioè la porta che annuncia la BPDU migliore verso il root per quella LAN.

Anche la scelta segue una priorità lessicografica:
1. _root-path-cost_ più basso
2. _bridge-identifier_ più basso
3. _port-identifier_ più basso
### 4. Blocking delle porte ridondanti

Finite le elezioni, tutte le porte che non sono né Root Port né Designated Port diventano ridondanti per la topologia logica e vengono messe in **blocking** (non inoltrano traffico utente), mentre Root/Designated vanno in **forwarding**.

Questo elimina i cicli a livello 2 mantenendo però i link fisici: se un link attivo fallisce, STP ricalcola la topologia e può sbloccare una porta prima bloccata per ripristinare la connettività.
## Cambiamenti di topologia

Un cambio di topologia può riaprire finestre di instabilità e potenziali loop che portano i pacchetti a saturare la rete.

I bridge possono rilevare dei cambiamenti alla topologia della rete e lo comunicano al Root Bridge attraverso un **Topology Change Notification BPDU**, mettendo il flag _topology change_ a 1, per innescare comportamenti di adattamento.

Il pacchetto Topology Change Notification viene trasmesso dal bridge fino a quando non riceve una Configuration BPDU con acknowledgement.

Nella pratica STP usa questi meccanismi per accelerare la “pulizia” delle informazioni obsolete (es. entries MAC apprese prima del cambio) e per gestire la riconvergenza in modo controllato.