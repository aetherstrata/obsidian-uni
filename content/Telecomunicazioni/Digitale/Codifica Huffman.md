---
tags:
  - segnale
  - digitale
---
Per diminuire la quantità di dati necessari per trasmettere un segnale, si può adottare una codifica a lunghezza variabile, come la codifica Huffman.

La codifica Huffman si usa nei casi in cui nel segnale esistono simboli più frequenti di altri. In questo modo, assegnando codici più corti ai simboli più frequenti e viceversa, si ottiene una riduzione della banda necessaria alla trasmissione del segnale.
## Descrizione
Per definire queste associazioni si usa un *albero binario* i cui valori dei nodi equivalgono alla somma delle probabilità (o occorrenze) dei due figli.

### Esempio
Dato il seguente insieme di simboli e i loro pesi proporzionali, si può ottenere un *albero di Huffman*.

| **Simbolo**     | a    | b    | c    | d    | e    | f    | g    | h    |
| --------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| **Probabilità** | 0.02 | 0.29 | 0.03 | 0.04 | 0.33 | 0.04 | 0.06 | 0.19 |
Per prima cosa i simboli vengono ordinati per peso.

| **Simbolo**     | a    | c    | d    | f    | g    | h    | b    | e    |
| --------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| **Probabilità** | 0.02 | 0.03 | 0.04 | 0.04 | 0.06 | 0.19 | 0.29 | 0.33 |
Per finire si costruisce l'albero prendendo sempre i due elementi con il peso più piccolo nella lista.
#### Step 1

