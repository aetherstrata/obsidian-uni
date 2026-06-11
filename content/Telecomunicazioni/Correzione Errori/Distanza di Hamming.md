---
tags:
  - segnale
  - errori
  - digitale
---

La distanza di Hamming $d$ tra due codeword è il numero di bit per cui differiscono. Per rilevare $k$ errori è necessario un codice la cui distanza sia $d = k + 1$. Per correggere $k$ errori è necessario un codice con una distanza $d = 2k + 1$.

> [!info] Rappresentazione grafica
> ![[Pasted image 20260603225540.png|center,bg-white]]
> La distanza di Hamming può anche essere rappresentata come la distanza di Manhattan tra due vertici di un ipercubo in cui tutti i vicini hanno una cifra differente.

> [!important] Implementazione
> La distanza di Hamming tra due valori binari può essere implementata applicando l'operatore XOR tra loro.
>
> $$
> a\,\textemdash\,b = a \oplus b
> $$
