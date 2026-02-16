---
tags:
  - internet
  - reti
  - datacenter
  - cloud
  - web
---
Lo sviluppo di servizi basati sul Web è diventato sempre più importante man mano che l'informatica si è diffusa in più domini di applicazione:
- e-commerce
- online banking
- pubblica amministrazione
- servizi streaming
- etc...

La reputazione delle aziende/organizzazioni è sempre più legata all'esperienza utente dei loro siti. Il numero di utenti e la loro distribuzione geografica impongono esigenze stringenti in termini di prestazioni.

>[!note] Terminologia
>- **Oggetto**: Una singola risorsa che il client scarica con una richiesta HTTP
>	- **Statico**
>	- **Dinamico**
>	- **Sicuro**
>- **Hit**: Richiesta di un singolo _oggetto_
>- **Page Request**: Insieme di più _hit_ che compongono la stessa pagina web
>- **Session**: Sequenza delle _page request_ provenienti dallo stesso client
## HTTP

Una stessa connessione TCP può essere usata per più richieste, e relative risposte, per evitare di eseguire il three-way handshake per ogni richiesta, diminuendo la latenza, e di effettuare lo [[Controllo di congestione#Slow start|slow start]], migliorando l'uso della banda. 

In HTTP/1.1 le connessioni TCP sono persistenti per default. Server e client possono comunque chiuderle in base a politiche locali (timeout, limiti risorse).​

Questo comportamento è di interesse sia per il client che per il server:
- **Client**: Tenere attive le connessioni TCP per molto tempo aiuta ad offrire un servizio più efficiente all'utente
- **Server**: Non consuma più socket per lo stesso client e termina le connessioni non attive

Dal punto di vista operativo, parametri come `KeepAlive`, `KeepAliveTimeout`,  `MaxKeepAliveRequests` (_Apache_) esistono perché ogni connessione mantiene stato e consuma risorse, quindi l’ottimo è sempre un compromesso tra latenza e capacità del server.​

## Caching lato client

I client Web fanno uso di caching locale per incrementare le prestazioni. 

Le richieste **DNS** sono memorizzate per essere riutilizzate in seguito. 

>[!warning] DNS e TTL
>Molti browser non tengono conto del TTL dei record DNS quando vengono memorizzati nella cache. Questa cosa è da tenere in considerazione quando si vuole progettare un sistema di distribuzione globale basato sul DNS.

Per gli **oggetti statici** fanno uso di validazioni condizionate (es. ETag) per evitare download inutili di contenuti statici.​

>[!warning] Coerenza e validazione
Il caching locale di contenuti riduce il traffico verso l’origine e smorza i picchi, ma introduce anche problemi di coerenza, invalidazione e contenuti personalizzati (meno cachabili perché dinamici).​

## Meccanismi di distribuzione

La scalabilità di un servizio Web dipende da diversi attori:
- vari _server_
- un _meccanismo di scheduling_ per assegnare le richieste al server migliore
- un _algoritmo di scheduling_ per stabilire il server migliore
- un'entità che esegue l'algoritmo e il meccanismo di scheduling

### Tassonomia dei meccanismi

| Meccanismo         | Entità        | Livello                       |
| ------------------ | ------------- | ----------------------------- |
| Name Resolution    | DNS           | Sessione                      |
| Anycast BGP        | Router        | Sessione                      |
| HTTP Redirection   | Web Server    | Page Request                  |
| Packet Redirection | Load Balancer | Sessione / Page Request / Hit |
### Tassonomia degli algoritmi

- **Statici**: Senza informazioni
- **Dinamici**:
	- Con informazioni sul _client_
	- Con informazioni sul _server_
	- Con informazioni su _entrambi_

### Tassonomia dei servizi scalabili

- **[[Distribuzione Locale]]**:
	- Scheduling a **un livello** (Load Balancer)
	- Scheduling a **due livelli** (Load Balancer e Redirection)
- **[[Distribuzione Globale]]**:
	- **MIrroring**
	- **Redirection**
	- **DNS**
	- **Anycast BGP**


