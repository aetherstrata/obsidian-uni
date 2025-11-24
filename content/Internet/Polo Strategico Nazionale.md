---
tags:
  - datacenter
  - cloud
  - italia
---
Il **PSN** nasce da una gara europea e viene gestito attraverso un partenariato pubblico-privato. Coinvolge principali player italiani: TIM, CDP, Leonardo e Sogei. L’obiettivo è centralizzare, proteggere e innovare le infrastrutture informatiche della PA, garantendo **sovranità, sicurezza e controllo** sui dati.
## Servizi offerti
### Housing e Hosting dedicato
Spazi fisici o virtuali per ospitare server della PA, con gestione sia fisica che virtuale dei sistemi.
### Private Cloud
Ambiente dedicato a una singola organizzazione, che offre sicurezza, controllo e personalizzazione avanzati.
#### Cloud IaaS
Infrastruttura virtualizzata, privata o condivisa, erogata come servizio.
#### Cloud PaaS
Comprende servizi come IAM, database, backup, disaster recovery, container, big data e AI.
### Public Cloud
Servizi integrati con i principali Cloud Service Provider (CSP) ma il controllo principale resta sempre in mano al personale del PSN.
#### Hybrid Cloud on PSN site
Servizi del CSP installati sull'**infrastruttura locale** del PSN. Combinazione di Cloud pubblico e privato mediante un'infrastruttura integrata.
#### Public Cloud PSN Managed
Servizi CSP erogati da una cloud region **dedicata** solo al PSN, con separazione logico/fisica e operata e controllata dal personale del PSN.
#### Secure Public Cloud
Servizi di messa in sicurezza e sottoinsieme del portafoglio delle offerte di un CSP

>[!note] CSP
>Per implementare questi servizi, il Polo Strategico Nazionale si è affidato ad Azure, Google Cloud e Oracle come fornitori di servizi cloud.

## Sicurezza e Sovranità dei dati

L'infrastruttura del Polo Strategico Nazionale deve garantire che:
- I dati **strategici** e **critici** restino in Italia, mantenendo la sovranità (cioè la piena proprietà e controllo da parte dello Stato/PA).
- Vengano adottate **soluzioni di sicurezza multi-livello**: dalla **protezione fisica** dei data center fino alla **crittografia avanzata** con gestione delle chiavi separata dai cloud provider esterni.
- La gestione degli accessi (IAM) e tutti i backup siano sotto la supervisione del PSN.

L’infrastruttura segue una sicurezza “a strati”:
1. **Physical:** accesso fisico, perimetri, sale dati protette.
2. **Network:** segmentazione e isolamento logico, policy di trasporto sicuro (TLS, LOAS, etc).
3. **Host/Software:** monitoring, auditing, gestione sicura delle credenziali e delle root key.
4. **Application/Operations:** access management, application security, supply chain trust, gestione e auditing dell’intero ciclo di vita hardware e software.
## Architettura di rete

Il PSN è composto da 4 data center distribuiti tra **due region** per continuità operativa e resilienza, 2 a Roma e 2 a Milano. Questo garantisce **continuità operativa** (fault tolerance, disaster recovery), anche tra region diverse.

Per collegarli è stata utilizzata una backbone IP TIM usando i protocollo MPLS e VXLAN per la connettività sicura e isolata tra i data center. Il funzionamento e i flussi dei data center sono analizzati sa un Security Operation Center (SOC) dedicato e sistemi di monitoring a più livelli.

Tutti i servizi includono inoltre strategie multi-livello di protezione (DDoS protection, crittografia, segregazione delle chiavi, monitoring, SIEM-EDR) e reti segregate.