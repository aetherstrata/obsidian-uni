---
tags:
  - web
  - internet
  - cloud
  - datacenter
---
Nei sistemi di distribuzione globale, il servizio è replicato in più sedi (data center), e a seconda del meccanismo si può avere un solo nome con più IP, o nomi diversi, o addirittura un solo IP condiviso.​
## Siti mirror

Il mirroring è l’approccio più semplice: per l'indirizzamento vengono usati nomi multipli e un IP per ogni server.

Non esiste un algoritmo di scheduling: la scelta del mirror è lasciata all'utente. Questo fa si che non sia possibile applicare nessuna regola di controllo della distribuzione del carico.

## HTTP redirection
Questo approccio è trasparente all'utente ma non al client.

L'HTTP redirect fa parte dello standard ed è supportato da tutti i browser. La redirect può ridirigere le richieste HTTP verso:
- un _indirizzo IP_
- un _nome_

L'entità che opera il redirect può essere un solo server che ridistribuisce il carico a tutti gli altri, oppure qualsiasi server che ridistribuisce il proprio carico quando è sovraccarico.

Il sistema può gestire la selezione delle pagine di cui effettuare il redirect:
- tutte le _page request_
- tutte le _page request_ con _dimensione_ superiore a una soglia
- tutte le _page request_ con un numero di _hit_ superiore a una soglia

>[!warning] Il client instaura due connessioni per ogni richiesta

## DNS

Lo scheduling basato su DNS è diviso in due grandi famiglie: 
1) risposta diretta con **record A/AAAA**: il DNS autoritativo sceglie l'IP
2) ridirezione via **record CNAME** verso un altro nome, e quindi potenzialmente verso un’altra infrastruttura DNS che fa scheduling più locale/specializzato
### DNS senza ridirezione
Qui il _TTL_ e il caching sono centrali: in DNS il _TTL_ definisce per quanto tempo un record _può_ essere mantenuto in cache prima di dover ricontattare la sorgente autoritativa, ma nella pratica ridurre troppo il _TTL_ può non dare controllo perfetto, a causa di cache intermedie, comportamento dei client, carico sul nameserver. 

#### TTL Costante
Si può impostare un _TTL_ costante con valori molto bassi per avere pieno controllo dello scheduling da parte del DNS. Il problema di un _TTL_ basso è che:
- gli altri name server possono ignorare il _TTL_ e impostare un valore più alto
- le cache locali dei browser possono mantenere il record per più tempo del dovuto
- il server autoritativo deve gestire più richieste
#### TTL Adattivo
Si può impostare un _TTL_ in modo dinamico in base al carico sul server e la frequenza di richieste.
### DNS con ridirezione
Il DNS autoritativo espone dei record di tipo **CNAME** (_canonical name_) che specificano un dominio come alias di un altro.

>[!info] I record CNAME sono anche usati per la localizzazione

Quando il client riceve il record **CNAME**, ricomincia il processo di query usando il canonical name al posto del nome originale. 

Questo meccanismo consente al name server principale di ridirigere il processo di risoluzione verso dei name server secondari in grado di effettuare uno scheduling locale.
## Anycast BGP

Con anycast lo stesso prefisso/indirizzo viene annunciato da più [[Autonomous System]] nella rete e il routing BGP porta il client verso l'istanza più vicina secondo le metriche di routing, rendendo l’indirizzamento molto semplice (un nome, un IP).​  ​

## Scheduling

Dei metodi elencati sopra, solo con alcuni è possibile scegliere il server che risponderà alle richieste dei client:
- **HTTP Redirect**
- **DNS**

mentre su questi la selezione non è possibile:
- **Mirror**: Il server è selezionato dall'utente
- **Anycast BGP**: Il server è implicitamente scelto dalla posizione del client

Ci sono diverse variabili di cui tenere conto per poter allocare in modo efficiente i server al clienti:
- la **prossimità** tra client e server
- il **carico** del server, in funzione di:
	- data e ora
	- fusi orari
	- distribuzione geografica degli utenti

### Stima della prossimità
Per stimare la _prossimità geografica_ si può far uso di due principali informazioni topologiche:
- l'**indirizzo IP** del client: si estrae l'[[Autonomous System|AS]] dalle informazioni di registro
- il **numero di hop**

Il problema di usare la prossimità geografica è che non tiene conto della banda o latenza dei link, che sono metriche indipendenti dalla posizione geografica.

Per stimare la _prossimità in Internet_ si può far uso di alcune misure attive:
- il _round trip time_ (**RTT**) della rete: misurato con `ping`
- tempo di _latenza_ su una richiesta HTTP
- disponibilità di banda sui link
### Scheduling gerarchico
Spesso la soluzione migliore è usare uno scheduling gerarchico, che consiste nel far uso di più meccanismi di scheduling in successione. Ad esempio, si può avere uno scheduling a due livelli:
1) _primo scheduling basato sul DNS_: considera la posizione del client
2) _secondo scheduling basato su HTTP Redirect_: considera il carico dei server

o uno scheduling a tre livelli:
1) _primo scheduling basato su anycast BGP_: partiziona i client per continente
2) _secondo scheduling basato sul DNS_: considera la posizione del client
3) _terzo scheduling basato su HTTP Redirect_: considera il carico dei server

>[!info] Web Cluster
>Questo riflette un principio pratico dei data center: 
>- usare meccanismi di _scheduling globale_ (robusti e scalabili) per identificare il **cluster** da interpellare
>- usare meccanismi di _scheduling locale_ (ricchi di segnali, più controllabili) per identificare il **server** da interpellare.
