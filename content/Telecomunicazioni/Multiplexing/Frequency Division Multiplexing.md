---
tags:
  - multiplexing
  - segnale
  - physical
  - digitale
aliases:
  - FDM
---

Nel _Frequency Division Multiplexing_ (**FDM**) tutti gli utenti trasmettono nello stesso momento, ma il canale fisico viene tagliato in più sottobande. Questo è il principio alla base della TV analogica e della radio [[Frequency Modulation|FM]]: ogni stazione trasmette sempre, ma su una frequenza diversa.

### Guard Band

I filtri elettronici non sono mai perfetti e non tagliano le frequenze in modo netto. Per evitare aliasing tra segnali di utenti vicini (_Inter Channel Interference_), si inseriscono bande di guardia vuote tra un canale e l'altro. Questo però spreca frequenze nella banda, riducendo l'efficienza spettrale.
