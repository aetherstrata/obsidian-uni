---
tags:
  - web
  - reti
  - datacenter
  - cloud
---
Una _Content Delivery Network_ (**CDN**) è un overlay di server geograficamente distribuiti che serve contenuti per conto di un content provider, con l'obiettivo principale di ridurre la latenza percepita dagli utenti, il carico sull'origine e aumentare affidabilità/scalabilità.​

>[!info] Reti di distribuzione
>Queste sono alcune delle maggiori aziende che forniscono servizi di CDN:
>- Akamai
>- CloudFlare
>- Amazon CloudFront
>- KeyCDN
>- MaxCDN
>- jsDeliver

## Modalità operative
I vari content provider devono servire ai loro clienti sia risorse _statiche_ che _dinamiche_.

I contenuti _statici_ possono essere serviti facilmente da un server standard.
I contenuti _dinamici_ richiedono server specializzati, facendo _fetch_ dal server di origine e/o trasporto interno su percorsi ottimizzati.

I contenuti _statici_ e _dinamici_ vengono assemblati nell'edge server (_assembly on the fly_) per ridurre round-trip lato client e aumentare le cache hit.

>[!info] Monitoraggio
>Le performance della rete CDN sono monitorate in vari punti critici della rete di distribuzione. Questo serve a raccogliere valori di banda e latenza, per poterli usare nel processo di selezione della _replica_ e ottimizzare i percorsi in Internet.

## Dynamic Assembly

Per poter unire i contenuti _statici_ e _dinamici_ negli edge server, i vari componenti della pagina Web vengono definiti usando un linguaggio di markup dedicato: _Edge Site Includes_ (**ESI**).

Quando il client manda una richiesta alla _replica_, riceve una pagina web normale.

I vari componenti che compongono quella pagina sono messi insieme dalla _replica_. Ognuno di questi ha il suo _TTL_, creando una cache locale nella _replica_.
## Tecniche di request routing
- **Full Site**: Il record DNS del dominio del content provider è un **CNAME** che punta verso un dominio della **CDN**. Il server di origine è sconosciuto su Internet: solo la **CDN** lo può contattare.
- **Partial Site**: La prima hit arriva al server di origine del content provider ma, prima di consegnare la pagina all'utente, cambia gli URL nella pagina in modo che vengano reindirizzati verso i server della **CDN**.

>[!tip] URL Rewriting nel Partial Site Delivery
>Per automatizzare questo processo, le **CDN** provvedono degli script ai content provider per leggere in modo trasparente il contenuto delle pagine web e modificare gli URL integrati.

A questo punto la richiesta DNS è verso un dominio della **CDN**, che applica uno scheduling gerarchico grazie a server DNS intermedi per arrivare ad avere uno (o più) indirizzo IP di un server _replica_ vicino all'utente.

>[!note] Scelta della replica migliore
>- Indirizzo IP del server DNS
>- Nome del content provider
>- Nome dell'oggetto richiesto

### Cache DNS
Questi meccanismi possono diventare inefficaci in quanto i record DNS possono essere immagazzinati nella cache locale. Questo comportamento può intaccare il corretto reindirizzamento dei client verso il server ottimale da parte della **CDN**.

Per risolvere questo problema, le **CDN** usano dei _TTL_ relativamente bassi:
- _edge server_: 20 secondi
- _low-level DNS_: decine di minuti
- _high-level DNS_: parecchie ore

>[!warning] Nulla vieta agli altri DNS di ignorare questi _TTL_ e usare un valore più grande
## Tipi di CDN
### Single ISP
Le CDN **Single ISP** operano come un ISP con un proprio [[Autonomous System|AS]] Number e hanno una loro rete globale per ottenere una buona copertura geografica. Per offrire i loro servizi, instaurano un peering BGP con le _eyeball networks_.
### Multi ISP
Le CDN **Multi ISP** distribuiscono i propri server in vari POP di diversi ISP in giro per il mondo per ottenere una buona copertura geografica. Con questo approccio i server della CDN possono essere usati solo dal customer cone dell'ISP a cui sono collegati.

>[!info] Eyeball Networks
>Le _eyeball networks_ sono le reti di accesso il cui uso principale degli utenti è quello di consumare contenuti:
>- navigare in internet
>- leggere le mail
>- guardare video
>- etc...

### Peering tra CDN
Le **CDN** possono anche effettuare _peering_ tra loro per aumentare la copertura dei propri servizi.

Di solito un content provider stipula un contratto solo con una CDN (_CDN autoritativa_), che a sua volta può pagare e/o lavorare con altre CDN per consegnare i contenuti del provider.