---
tags:
  - multiplexing
  - segnale
  - physical
  - digitale
---
Il *Code Division Multiplexing* (**CDM**) è radicalmente diverso rispetto a [[Frequency Division Multiplexing|FDM]] e [[Time Division Multiplexing|TDM]]: non divide lo spettro e non divide il tempo. Tutti gli utenti trasmettono contemporaneamente e sulla stessa banda spettrale. La separazione avviene a livello logico: a ogni utente è assegnato un codice matematico unico e ortogonale.

$$
\int_{-\infty}^{\infty} c_i(t)\cdot c_j(y)\,dt=0\quad \forall\ i\ne j
$$
## Direct Sequence Spread Spectrum
Il bit d'informazione a bassa velocità viene moltiplicato con una sequenza di codice ad alta velocità. Questa operazione spalma il segnale su una larghissima banda spettrale.

Il rapporto tra chip rate e bit rate è chiamato **Spread Factor** ed è definito come $\dfrac{T_{bit}}{T_{chip}}$.

![[Pasted image 20260606163519.png]]

Uno Spread Factor alto espande molto il segnale, aumentando la resistenza al rumore, ma richiede una maggiore occupazione di banda.

>[!example] Esempio - 3G
Nell'UMTS 3G (che usa W-CDMA), la voce viaggia a 12.2 Kbps ma viene codificata (spread) a 3.84 Mbps.

### Ricezione
Il ricevitore riceve un rumore formato da tutti gli utenti sommati, ma moltiplicandolo di nuovo per il codice matematico dell'utente specifico, riesce a estrarre solo il segnale originale, scartando gli altri come se fossero rumore.

>[!tip] Vantaggi e svantaggi
>Offre un'eccezionale resistenza alle interferenze e crittografia intrinseca (senza conoscere il codice è impossibile intercettare i segnali originali). Inoltre, gestisce in modo morbido (soft) l'aumento degli utenti nella cella senza rigide ripianificazioni.
>
>Il grande difetto è il problema del *near-far*, ovvero che se un utente vicino all'antenna trasmette con una potenza troppo forte, copre il codice di chi è lontano, richiedendo un rigido controllo automatico della potenza e del bit-rate complessivamente limitato dal _processing gain_.