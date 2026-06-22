---
tags:
  - segnale
  - digitale
  - errori
aliases:
  - CRC
---

Il _Cyclic Redundancy Check_ (**CRC**) è il metodo più diffuso ed efficiente al mondo per verificare l'integrità dei dati digitali. Viene impiegato in molti campi: dalle reti Ethernet e Wi-Fi (pacchetti TCP/IP), ai sistemi di archiviazione come hard disk e memorie flash, fino ai file compressi (.zip).

> [!important] Niente correzioni
> È fondamentale ricordare che il **CRC** è un meccanismo di puro rilevamento. Non possiede le istruzioni matematiche per riparare il danno, come fa invece la [[Forward Error Correction|FEC]]. Se il controllo CRC fallisce, l'azione di base consiste nello scartare il pacchetto e innescare un protocollo di tipo [[Automatic Repeat Request|ARQ]] per chiedere la ritrasmissione.

> [!danger] Sicurezza
> A differenza delle funzioni di hash crittografiche (come l'**MD5** o lo **SHA**), il CRC non è stato progettato per prevenire alterazioni malevole, ma per scoprire _modifiche accidentali_ causate da rumore elettromagnetico, attenuazioni di linea o difetti fisici dei supporti di memorizzazione.

## Funzionamento

Il funzionamento del CRC si basa su una lunga divisione eseguita in **aritmetica modulare binaria**. In questa aritmetica non esistono riporti nelle addizioni né nelle sottrazioni: l e operazioni di addizione e sottrazione coincidono esattamente con la porta logica **XOR**.

### Codifica

#### Scelta del Polinomio Generatore (G)

Mittente e destinatario concordano a priori un numero binario fisso (detto Polinomio Generatore). Se questo generatore ha $r+1$ bit, il valore del resto (cioè il CRC) occuperà esattamente $r$ bit.

#### Padding dei dati (D)

Il mittente prende il blocco di dati e gli concatena in fondo una serie di $r$ bit a zero, che matematicamente equivale a moltiplicare il dato per $2r$).

#### La Divisione XOR

Il mittente divide questa nuova stringa allungata per il Generatore $G$ tramite operazioni XOR.

#### Assemblaggio del Frame

Il resto generato da questa divisione è il **Checksum CRC**. Il mittente sostituisce gli zeri di padding col resto ottenuto e trasmette il pacchetto così composto. Il pacchetto intero è ora un multiplo perfetto del generatore $G$.

### Ricezione

1. Il destinatario riceve il blocco di dati con il CRC in coda.
2. Divide l'intera sequenza ricevuta per lo stesso Polinomio Generatore $G$.
3. **Controllo del resto**: Se la trasmissione è avvenuta senza disturbi, il resto matematico di questa divisione sarà **esattamente 0**. Se c'è anche solo un resto di $1$, significa che almeno un bit si è invertito durante il viaggio, e il pacchetto viene scartato.

## Implementazione

Le stringhe binarie sono descritte come polinomi, dove i bit (0 o 1) sono i coefficienti di una variabile $x$, e la posizione del bit è l'esponente della $x$.

> [!example] Esempio
> La stringa `1001` equivale a $1x^3+0x^2+1x^1+1x^0 = x^3+x+1$.

### Shift Register

l motivo per cui il **CRC** ha dominato l'industria è che questa matematica polinomiale si traduce in hardware con una serie di operazioni semplici e veloci. Non serve un microprocessore potente per calcolarlo, bastano pochi transistor organizzati in un circuito chiamato **LFSR (Linear Feedback Shift Register)**.

Come esempio è preso questo algoritmo CRC a 24 bit:

$$
x^{24}+x^{10}+x^9+x^6+x^4+x^3+x^1+1
$$

Per implementare questo algoritmo in hardware, si prendono 24 celle di memoria _flip-flop_ per fare lo _shift register_.

Gli esponenziali del polinomio indicano dove posizionare i cavi per intercettare il segnale, e in quelle prese si mettono delle porte logiche **XOR**. Mentre i bit dei dati entrano in sequenza nel registro, partendo dal bit meno significativo, le porte XOR ricalcolano istantaneamente il sistema innescando delle retroazioni cicliche. Alla fine del flusso di dati, i 24 bit rimasti incastrati nel registro costituiscono direttamente il resto, ovvero il CRC pronto per essere trasmesso.

> [!info] Schema di funzionamento
> ![[Pasted image 20260604213827.png]]

## Burst Error

La scelta dei **Tap** (gli esponenti del polinomio) non è mai casuale, ma è frutto di lunghi studi matematici. Un polinomio ben scelto, come il CRC-24 o il CRC-32 (usato in Ethernet), garantisce:

- Il rilevamento del 100% degli errori di singolo bit.
- Il rilevamento del 100% degli errori di due bit isolati.
- Il rilevamento del 100% di tutti i **Burst Error** che abbiano una lunghezza inferiore al grado del polinomio (es. un CRC-32 rileva sempre raffiche di errore fino a 32 bit consecutivi).
- Per raffiche di errori ancora più lunghe, la probabilità che il CRC produca un "falso positivo" (cioè dia resto 0 pur essendoci un errore disastroso) scende a frazioni microscopiche, rendendo il protocollo matematicamente solidissimo per le reti commerciali.
