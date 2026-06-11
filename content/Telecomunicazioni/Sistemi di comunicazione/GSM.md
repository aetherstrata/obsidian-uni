---
tags:
  - sistema
  - mobile
  - digitale
---

I sistemi radio cellulari 1G, come l'**AMPS**, usavano solo l'_FDMA_ (un canale da 30 kHz per ogni telefonata) ed erano inefficienti. Con la rete 2G (**GSM**) si è passati a una configurazione ibrida: la banda radio viene prima divisa in canali da 200 kHz (FDMA) e poi ogni canale viene diviso nel tempo in 8 slot (TDMA). Questo permette di servire 8 utenti sulla stessa frequenza, riducendo anche il consumo della batteria dei cellulari (che spengono il trasmettitore nei 7 slot in cui non devono parlare).

## Configurazione

Nei sistemi mobili 2G **GSM** (_Global System for Mobile Communications_), le celle presentano configurazioni multifrequenza (dual band 900/1800 MHz). In Europa le bande sono:

- GSM-900:
  - Uplink: 890-915 MHz
  - Downlink: 935-960 MHz
- GSM-1800:
  - Uplink: 1710-1785 MHz
  - Downlink: 1805-1880 MHz

Queste bande vengono divise sia in frequenza che nel tempo:

- FDMA: 124 (o 374) portanti, canali radio di banda 200 (o 75 MHz).
- TDMA: N=8 canali telefonici per singolo canale radio
