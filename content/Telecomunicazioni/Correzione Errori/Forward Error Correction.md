---
tags:
  - errori
  - segnale
  - digitale
aliases:
  - FEC
---

La **FEC** (_Forward Error Correction_) è un meccanismo di correzione degli errori proattivo: aggiunge al pacchetto originario molti più bit di ridondanza, progettati non solo per accorgersi di un errore, ma per calcolare esattamente quali bit si sono invertiti e ripararli _on the fly_.

## Funzionamento

### Codifica

Applica un codice tramite algoritmi di correzione dell'errore (ECC) ed emette un pacchetto più lungo sul canale.

#### Codifica a blocchi

I codici a blocchi sono una categoria di algoritmi per la correzione degli errori che operano su segmenti di dati di lunghezza fissa. Il trasmettitore aggiunge bit di controllo a un blocco di bit di dati. Il ricevitore usa quei bit per verificare se ci sono stati errori e, se possibile, correggerli.

Alcuni tipi di codici a blocchi utilizzati sono i seguenti:

- **Codici di Hamming**: Possono correggere un singolo bit errato per blocco. Sono stati i primi ad essere utilizzati.
- **[[Reed-Solomon]]**: Sono stati lo standard per i CD/DVD e sono usati nel DVB-S (satellite) e nelle fibre ottiche.
- **BCH** (_Bose-Chaudhuri-Hocquenghem_): Utilizzati nel DVB-S2 e nelle memorie Flash. Correggono errori sparsi in blocchi di dati molto grandi.
- **Codici LDPC** (_Low-Density Parity-Check_): Sono usati nel Wi-Fi 6, nel 5G e nel DVB-S2 e permettono di avvicinarsi al limite teorico di trasmissione.

#### Codifica sistematica

la codifica si dice **sistematica** se i dati originali appaiono non modificati nel segnale codificato di uscita, altrimenti si dice **non-sistematica**.

### Decodifica e Autocorrezione

Il ricevitore esamina la sequenza. Se c'è un'alterazione limitata, il ricevitore è in grado di dedurre quale doveva essere l'informazione originaria e corregge i bit invertiti.

### Nessun ritorno

Nessun pacchetto viaggia all'indietro. Non c'è **ACK**, non c'è **NACK**.

## Requisiti e Limiti

- **Aumento dell'occupazione di banda:** Le informazioni aggiunte in trasmissione generano overhead. Una parte della banda passante o bit-rate utile viene sottratto per inserire dati di controllo.
- **Soglia di correzione:** La FEC funziona molto bene con un rumore moderato. Se è presente un disturbo molto lungo, questo distrugge troppi bit, la decodifica fallisce e, siccome non c'è ritrasmissione, quel pacchetto è irrimediabilmente perso e viene scartato.
- **Hardware dedicato:** Serve potenza di calcolo continua e immediata nei microprocessori di ricezione per risolvere gli algoritmi in tempo reale.

> [!info] Applicazioni ideali
> Poiché il tempo di trasmissione è fisso e deterministico, la FEC è l'unica scelta per i sistemi real-time, come lo streaming video, la telefonia satellitare, le dirette TV, le dorsali in fibra ottica ad altissima velocità e per tutti i sistemi monodirezionali (come la TV satellitare DVB), dove un server non può ricevere miliardi di ACK dagli spettatori.
