---
tags:
  - errori
  - segnale
  - digitale
---

L'**ARQ** è un meccanismo di correzione degli errori _reattivo_: accetta la possibilità che un pacchetto arrivi errato e, anziché cercare di correggerlo, si concentra sulla probabilità di scoperta dell'errore e, in caso di errori, richiede al mittente una nuova trasmissione.

## Funzionamento

### Calcolo della ridondanza

Prima di spedire un blocco di bit informativi, il sistema trasmittente calcola un codice di controllo (spesso un **[[Cyclic Redundancy Check|CRC]]**, _Cyclic Redundancy Check_). Lo accoda ai dati originali, crea il pacchetto, se ne salva una copia in un buffer temporaneo e lo trasmette.

### Rilevazione dell'errore

Quando il ricevitore ottiene il pacchetto, ricalcola il **[[Cyclic Redundancy Check|CRC]]**. Se l'esito combacia, il pacchetto è integro, altrimenti contiene degli errori e va richiesto di nuovo.

#### Acknowledgement e Timer

- Se tutto è corretto, il ricevitore manda un riscontro positivo (**ACK**). Il trasmettitore svuota il buffer.
- Se c'è un errore, manda un riscontro negativo (**NACK**).

#### Gestione del timeout

Se il trasmettitore non riceve né ACK né NACK entro un tempo massimo prefissato (_timeout_), assume che il pacchetto (o il riscontro) sia andato perso e **ritrasmette in automatico** i dati salvati nel buffer.

> [!tip] Da ricordare
> Questa ritrasmissione in **automatico** è la parte fondamentale di questo meccanismo.

## Requisiti e Limiti

1. **Canale bidirezionale (Full-Duplex):** L'ARQ non può funzionare su canali a senso unico. Il ricevitore deve fisicamente poter comunicare col mittente per mandare i riscontri.
2. **Latenza variabile:** Questo è il difetto più grande dell'ARQ. Aspettare un ACK e ritrasmettere pacchetti richiede molto tempo. Questo genera un ritardo imprevedibile (_jitter_) nella comunicazione.

> [!info] Applicazioni ideali
> Poiché l'ARQ garantisce matematicamente che i dati consegnati siano perfetti, si usa dove l'affidabilità è più importante della velocità: trasferimento file, navigazione web, email e, in generale, tutti i protocolli TCP/IP basati su conferma.
