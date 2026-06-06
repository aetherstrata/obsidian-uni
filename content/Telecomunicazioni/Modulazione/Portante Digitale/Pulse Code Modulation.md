---
tags:
  - modulazione
  - digitale
---
La *Pulse Code Modulation* (**PCM**) è una tecnica di modulazione usata per trasferire dati in formato digitale su portante digitale.

## Funzionamento
Il processo di modulazione PCM si articola in tre passaggi in sequenza.
### Campionamento (Discretizzazione nel tempo) 
Il segnale analogico continuo viene misurato a intervalli di tempo regolari $T$. La frequenza con cui si estraggono questi campioni deve rispettare il [[Telecomunicazioni/Segnali/Operazioni e Proprietà/Teorema del Campionamento|Teorema del Campionamento]]: la frequenza di campionamento deve essere pari ad almeno il doppio della frequenza massima contenuta nello spettro del segnale. 

Per la telefonia standard (con banda vocale limitata a circa 4 kHz), la frequenza di campionamento è fissata a 8 kHz, producendo 8.000 campioni al secondo.
### Quantizzazione (Discretizzazione in ampiezza)
I campioni estratti, che in analogico possiedono infiniti valori di tensione possibili, vengono [[Quantizzazione|quantizzati]] al livello di riferimento più vicino all'interno di un set predefinito di valori. Questo passaggio introduce l'errore di quantizzazione dato dalla differenza tra il valore reale del campione e il valore arrotondato.
### Codifica
Ogni livello quantizzato viene tradotto in un numero binario. Ad esempio, codificando l'ampiezza del segnale in 8 bit, si potranno avere 256 livelli diversi $(2^8 = 256)$. 

Nel caso della telefonia classica, 8.000 campioni al secondo moltiplicati per 8 bit restituiscono il bit rate standard di 64 kbps per canale PCM.

## Ottimizzazione con Companding

Per migliorare il *rapporto segnale-rumore* ([[Trasmissione Digitale#SNR|SNR]]) senza aumentare il numero di bit, si utilizza la tecnica del _[[companding]]_, applicando una quantizzazione non lineare. Anziché distribuire i livelli di quantizzazione a intervalli regolari su tutta la dinamica del segnale, i livelli vengono addensati per le basse ampiezze e diradati per le ampiezze maggiori. 

Poiché nella voce umana i suoni a bassa ampiezza sono molto più frequenti di quelli ad alta ampiezza, questa tecnica permette di codificarli con maggiore precisione. 

## Ottimizzazione con TDM

Nella PCM, l'informazione non impegna il canale trasmissivo in maniera continua. Nel caso della voce campionata a 8 kHz, un campione codificato viene trasmesso ogni 125 microsecondi. Per sfruttare efficacemente la banda disponibile sul cavo o sulla fibra, si usa la tecnica *Time Division Multiplexing* ([[Time Division Multiplexing|TDM]]).

Il TDM interlaccia le trasmissioni di più utenti sullo stesso canale fisico, assegnando a ciascun utente un breve *time slot*. In questo modo, un unico mezzo di comunicazione ad alta capacità può veicolare decine o centinaia di flussi PCM simultaneamente.

## Vantaggi
- **Robustezza al rumore:** I segnali digitali, essendo costituiti solo da due stati (0 e 1), sono intrinsecamente meno suscettibili ai disturbi, all'interferenza elettromagnetica e all'attenuazione rispetto a un segnale analogico.
- **Rigenerazione del segnale:** Durante le trasmissioni a lunga distanza, il segnale digitale degradato può essere "rigenerato" perfettamente dai ripetitori di linea ricostruendo i fronti d'onda degli impulsi binari, evitando l'accumulo del rumore che si ha invece con gli amplificatori analogici.
- **Alta fedeltà e scalabilità:** Aumentando la frequenza di campionamento e il numero di bit, è possibile ottenere riproduzioni ad altissima fedeltà, standard su cui si basa tutta la registrazione audio moderna (come nel caso dei CD audio, che usano PCM a 44.1 kHz e 16 bit).
- **Elaborazione e crittografia:** Essendo flussi binari, i segnali PCM possono essere facilmente compressi da DSP, cifrati per garantire la sicurezza informatica e memorizzati su memorie di massa.

## Svantaggi
- **Alto consumo di banda:** Il limite principale della PCM pura (senza algoritmi di compressione di sorgente) è che consuma molta più larghezza di banda sul mezzo fisico rispetto alla trasmissione del segnale analogico originale.
- **Complessità circuitale:** Richiede convertitori analogico-digitali (ADC) in trasmissione e digitale-analogici (DAC) in ricezione ad alta velocità e precisione. In passato questo era un limite, ma lo sviluppo della microelettronica moderna ha reso questi componenti economici ed estremamente compatti.