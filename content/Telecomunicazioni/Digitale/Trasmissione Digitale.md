---
tags:
  - segnale
  - sistema
  - digitale
---
Una trasmissione si dice *digitale* se i dati sono codificati in sequenze di [[Entropia|bit]].

Si può avere una trasmissione digitale anche partendo da una sorgente analogica, attraverso un *convertitore analogico-digitale* che effettua il [[Teorema del Campionamento|campionamento]], la [[Quantizzazione|quantizzazione]] e, per finire, la codifica di sorgente per comprimere i dati e la codifica di canale per aggiungere ridondanza per la rivelazione e/o correzione degli errori.
$$
x(t)\longrightarrow\boxed{\vphantom{\int}\text{ campionatore }}\overset{X[n]}{\longrightarrow}\boxed{\vphantom{\int}\text{ quantizzatore }}\overset{X'[n]}{\longrightarrow}\boxed{\vphantom{\int}\text{ codificatore }}\longrightarrow C_n
$$
A questo poi si aggiunge un modulatore che effettua una codifica di linea con cui il segnale viene adattato alle caratteristiche del canale di trasmissione ed il segnale viene inviato nel canale.

Alla ricezione corrispondono le operazioni complementari a quanto detto (es. decodifica, demodulazione, ecc...).
$$
x(t)\longrightarrow\boxed{\vphantom{\int}\text{ ADC }}\longrightarrow\boxed{\vphantom{\int}\text{ canale }}\longrightarrow\boxed{\vphantom{\int}\text{ DAC }}\longrightarrow x(t)
$$
