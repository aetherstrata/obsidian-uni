---
tags:
  - segnale
  - digitale
  - codifica
---
Le codifiche di linea servono a portare i bit su un mezzo fisico attraverso variazioni di tensione o corrente o propagazione di onde.

L'obiettivo principale di tutte le codifiche (tranne la NRZ) è garantire la **sincronizzazione** tra trasmettitore e ricevitore. Se si devono trasmettere 1000 zeri di fila, il segnale non può rimanere piatto, altrimenti l'orologio interno del ricevitore (il _clock_) perde il conto e non sa più quanti zeri sono stati trasmessi. Le transizioni (cambi di livello) servono proprio al ricevitore per mantenere il ritmo in mancanza di un oscillatore locale.

## NRZ (Non Return to Zero)

La codifica **NRZ** è la codifica più basilare, usata tipicamente all'interno delle schede madri dei PC o in sistemi molto semplici e a corto raggio.

Il bit `1` corrisponde al livello di tensione alto (es. +5V). Il bit `0` corrisponde al livello basso (es. 0V, o -5V se serve DC balance).

Il livello di tensione rimane costante per tutta la durata dell'intervallo di clock, non "ritorna a zero" a metà bit (da qui il nome).

>[!danger] Perdita di sincronizzazione
>Se viene trasmessa una lunga sequenza di bit identici (es. `111111` o `000000`), la tensione non cambia mai. Il ricevitore perde la sincronizzazione, in quanto senza fronti d'onda non sa dove finisce un bit e inizia il successivo. Inoltre, possiede una forte componente continua (DC) che la rende inadatta al passaggio attraverso i trasformatori delle linee telefoniche.

## NRZI (Non Return to Zero Inverted)
È una variante differenziale dell'NRZ, usata ad esempio nei cavi in fibra ottica per l'FDDI e nei cavi USB (con bit-stuffing). Invece di guardare il _livello_ assoluto della tensione, guarda i _cambiamenti_ di livello.

Quando viene trasmesso un `1`, c'è una transizione (il segnale inverte il suo stato: se era alto va basso, se era basso va alto) a inizio bit.

Quando viene trasmesso uno `0`, il segnale non cambia, mantiene il livello precedente.

Questa codifica risolve il problema della sincronizzazione per le lunghe sequenze di `1` (perché il segnale oscillerà continuamente), ma non risolve il problema delle sequenze lunghe di `0`. Per questo viene spesso accoppiata a codifiche di blocco come il *4b/5b* (che vieta l'inserimento di troppi zeri di fila prima che entrino nel codificatore).

## RZ (Return to Zero)

Questo codice prova a risolvere il problema della componente continua e del sincronismo forzando il segnale a tornare a livello zero _a metà_ del periodo di clock.

Quando viene trasmesso un `1`, la prima metà del tempo del bit è *alta*, la seconda metà *torna a zero* (livello nullo).

Quando viene trasmesso uno `0`, il segnale rimane a zero per tutto il periodo. (Nelle varianti bipolari, il bit 0 scende a un voltaggio negativo e poi "torna a zero").

 Per l'`1` si forza sempre una transizione che aiuta il clock, ma per sequenze lunghe di `0` il problema della sincronizzazione persiste. Inoltre, inserire una transizione in un singolo periodo di bit richiede di raddoppiare la banda passante (e quindi la frequenza) richiesta al mezzo trasmissivo.
 
## Codifica Manchester (Bifase)

La codifica di Manchester combina i dati e il segnale di clock insieme, forzando una transizione esattamente a metà di **ogni singolo bit**, indipendentemente dal suo valore.
### Regola IEEE 802.3

- Il bit `1` è rappresentato da un passaggio dal livello **basso** al livello **alto** a metà bit.
- Il bit `0` è rappresentato da un passaggio dal livello **alto** al livello **basso** a metà bit.

Con la codifica Manchester il sincronismo è perfetto. Il ricevitore può agganciarsi al clock usando i fronti d'onda centrali. In più, la componente DC è sempre nulla (media perfetta tra + e - per ogni bit).

Come nell'RZ, le due variazioni (alta/bassa) per singolo bit richiedono una frequenza di commutazione (Baud rate) che è il doppio del bit-rate. Su cavi di alta categoria o per velocità elevate, spreca troppa banda.

## 5. AMI (Alternate Mark Inversion)

La codifica AMI si distacca dalle precedenti perché non è binaria (2 livelli), ma **ternaria** (usa tre livelli di tensione: positivo, nullo e negativo). È famosa per essere usata nelle dorsali PCM (T1/E1) della telefonia.

 Il bit `0` equivale a zero Volt.
 
Il bit `1` viene trasmesso alternando di volta in volta una tensione positiva (+V) e una negativa (-V). Ad esempio, la sequenza `10101` diventerà: 
$$
+V,0,-V,0,+V
$$
### Vantaggi

- Annulla la componente continua del segnale: siccome gli "1" sono uno positivo e uno negativo, la tensione media del cavo resta a 0 Volt.
- Permette di rilevare eventuali errori di linea. Se arrivano due impulsi consecutivi della stessa polarità (es due `+V` di fila), il ricevitore sa immediatamente che c'è stato un errore di trasmissione.

### Difetto
Le lunghe sequenze di `0` (zero Volt continui) fanno perdere il clock al ricevitore. Questo difetto è stato corretto storicamente dalle derivazioni dell'AMI, come il codice _HDB3_ (usato in Europa) o _B8ZS_ (in America), che iniettano dei finti bit *alti* se ci sono troppi zeri consecutivi per mantenere sveglio il clock.