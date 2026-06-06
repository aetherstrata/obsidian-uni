---
tags:
  - multiplexing
  - segnale
  - digitale
  - physical
---
L'*Orthogonal Frequency Division Multiplexing* (**OFDM**) ha rivoluzionato il panorama delle telecomunicazioni (è la base di 4G, 5G, Wi-Fi moderno e ADSL) per rimediare all'inefficienza dello spettro del vecchio [[Frequency Division Multiplexing|FDM]].
## Funzionamento
### Ortogonalità
Anziché dividere un flusso dati molto veloce su una singola portante molto larga, che subirebbe fortissime distorsioni e riflessioni (_multipath fading_), l'OFDM lo divide su centinaia di piccole sottoportanti vicinissime tra loro a bassissima velocità.

![[Pasted image 20260606172701.png]]
### Sovrapposizione
A differenza dell'[[Frequency Division Multiplexing|FDM]] tradizionale che deve tenere staccati i canali, l'OFDM permette che gli spettri delle singole sottoportanti **si sovrappongano**. Questo è possibile perché sono matematicamente ortogonali tra loro: i loro picchi coincidono con lo zero esatto delle altre.

### Prefisso Ciclico (CP) 
Per evitare l'interferenza tra simboli successivi a causa degli echi del canale, tra un simbolo e l'altro viene inserito un tempo di vuoto copiando in esso l'ultima parte del simbolo stesso. Questo annulla del tutto la distorsione multi-percorso (ISI), a costo di sprecare una piccola frazione di tempo ed efficienza.
## Implementazione
I circuiti analogici necessari per filtrare centinaia di frequenze ravvicinate sarebbero impossibili da costruire, quindi le operazioni richieste dall'OFDM vengono eseguite da microchip DSP attraverso la trasformata di Fourier Veloce Inversa (*IFFT*) in trasmissione e la *FFT* in ricezione.

>[!info] Schema di un trasmettitore OFDM
>![[Pasted image 20260606173352.png]]

>[!info] Schema di un ricevitore OFDM
>![[Pasted image 20260606173450.png]]
