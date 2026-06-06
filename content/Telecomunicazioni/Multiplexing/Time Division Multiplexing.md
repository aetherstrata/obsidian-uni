---
tags:
  - multiplexing
  - segnale
  - physical
  - digitale
---
Nel *Time Division Multiplexing* (**TDM**) il canale viene concesso per intero, ma diviso in finestre di tempo (slot temporali) di durata $T_s$. Ogni utente trasmette i propri dati a rotazione con un algoritmo round robin all'interno di un intervallo chiamato _Frame_.
$$
T_s=\frac{T}{N}\quad;\quad R=\frac{1}{T}
$$
## Funzionamento 
Gli slot sono fissi e preassegnati quindi, anche se un utente non ha nulla da trasmettere in quel momento, lo slot rimane vuoto. Questo causa un potenziale spreco di risorse, ma garantisce determinismo. Si possono anche assegnare più slot a clienti che richiedono maggiore bit rate.

![[Pasted image 20260606153455.png]]

### TDM Statistico (Asincrono)
Gli slot vengono assegnati dinamicamente solo agli utenti attivi, migliorando l'efficienza. Tuttavia, richiede che ogni pacchetto abbia un'intestazione (header) per capire a chi appartengono i dati.

>[!tip] Vantaggi e svantaggi
>Ha un basso costo e un'implementazione semplice. Inoltre, permette di scalare la banda per chi ne ha più bisogno assegnandogli semplicemente più slot. Lo svantaggio è che questo meccanismo necessita di una sincronizzazione temporale perfetta tra *TX* e *RX* (framing) ed è vulnerabile all'*Inter Symbol Interference* (**[[Condizione di Nyquist sulle interferenze|ISI]]**) ad alte velocità.

## Digital Hierarchy
Un esempio classico di utilizzo della **TDM** è la telefonia [[Pulse Code Modulation|PCM]] (lo standard DS1/T1) dove 24 utenti vocali vengono multiplati su un'unica linea trasmettendo campioni di 8 bit a rotazione ogni $125\ \mu s$.

Il segnale digitale interlacciato nel dominio del tempo con i segnali provenienti da altri utenti  forma una **Digital Hierarchy**.

Il Frame *DS1* si ottiene multiplando i byte dei 24 utenti *DS0* e aggiungendo un bit addizionale (*F-bit*) che viene utilizzato per il controllo degli errori.
$$
\begin{gather}
24\ \text{utenti}\cdot8\ \text{bit}+1\ \text{F-bit} = 193\ \text{bit} \\\\
\frac{193\ \text{bit}}{125\ \mu s}\cdot 10^6\ \mu s = 1.544\ \text{MBps}
\end{gather}
$$

