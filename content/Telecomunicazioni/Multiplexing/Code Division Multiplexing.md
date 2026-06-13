---
tags:
  - multiplexing
  - segnale
  - physical
  - digitale
aliases:
  - CDM
---

Il _Code Division Multiplexing_ (**CDM**) è radicalmente diverso rispetto a [[Frequency Division Multiplexing|FDM]] e [[Time Division Multiplexing|TDM]]: non divide lo spettro e non divide il tempo. Tutti gli utenti trasmettono contemporaneamente e sulla stessa banda spettrale. La separazione avviene a livello logico: a ogni utente è assegnato un codice matematico unico e ortogonale.

$$
\int_{-\infty}^{\infty} c_i(t)\cdot c_j(y)\,dt=0\quad \forall\ i\ne j
$$

## Direct Sequence Spread Spectrum

Il bit d'informazione a bassa velocità viene moltiplicato con una sequenza di codice ad alta velocità. Questa operazione spalma il segnale su una larghissima banda spettrale.

Il rapporto tra chip rate e bit rate è chiamato **Spread Factor** ed è definito come $\dfrac{T_{bit}}{T_{chip}}$.

![[Pasted image 20260606163519.png]]

Uno Spread Factor alto espande molto il segnale, aumentando la resistenza al rumore, ma richiede una maggiore occupazione di banda.

> [!example] Esempio - 3G
> Nell'UMTS 3G (che usa W-CDMA), la voce viaggia a 12.2 Kbps ma viene codificata (spread) a 3.84 Mbps.

### Ricezione

Il ricevitore riceve un rumore formato da tutti gli utenti sommati, ma moltiplicandolo di nuovo per il codice matematico dell'utente specifico, riesce a estrarre solo il segnale originale, scartando gli altri come se fossero rumore.

> [!tip] Vantaggi e svantaggi
> Offre un'eccezionale resistenza alle interferenze e crittografia intrinseca (senza conoscere il codice è impossibile intercettare i segnali originali). Inoltre, gestisce in modo morbido (soft) l'aumento degli utenti nella cella senza rigide ripianificazioni.
>
> Il grande difetto è il problema del _near-far_, ovvero che se un utente vicino all'antenna trasmette con una potenza troppo forte, copre il codice di chi è lontano, richiedendo un rigido controllo automatico della potenza e del bit-rate complessivamente limitato dal _processing gain_.

## Code Division Multiple Access (CDMA)

Il **CDMA** è l'applicazione del CDM come tecnica di accesso multiplo nelle reti cellulari: mentre il CDM descrive il principio matematico di separazione tramite codici ortogonali, il CDMA definisce come tale principio viene usato per permettere a più utenti di condividere lo stesso canale radio simultaneamente.

### Spreading e Despreading

In trasmissione, ogni bit informativo (a bassa velocità) viene moltiplicato chip per chip con il codice assegnato all'utente, producendo un segnale a banda larga:

$$
s_i(t) = d_i(t) \cdot c_i(t)
$$

dove $d_i(t)$ è il segnale dati e $c_i(t)$ è il codice di spreading dell'utente $i$. Il segnale trasmesso sul canale condiviso è la **somma** di tutti gli utenti attivi:

$$
r(t) = \sum_{i=1}^{N} s_i(t) + n(t) = \sum_{i=1}^{N} d_i(t)\cdot c_i(t) + n(t)
$$

In ricezione, per estrarre il segnale dell'utente $k$, si moltiplica il segnale ricevuto per $c_k(t)$ e si integra sul periodo di bit:

$$
\hat{d}_k = \int_0^{T_{bit}} r(t) \cdot c_k(t)\, dt = d_k \underbrace{\int_0^{T_{bit}} c_k^2(t)\,dt}_{=\ 1} + \sum_{i \ne k} d_i \underbrace{\int_0^{T_{bit}} c_i(t)\cdot c_k(t)\,dt}_{=\ 0}
$$

Grazie all'ortogonalità dei codici, tutti i contributi degli altri utenti si annullano, restituendo esattamente $d_k$​.

### Codici Ortogonali Walsh-Hadamard e PN

In W-CDMA ([[Sistemi mobile#3G - UMTS / WCDMA|UMTS]]) si usano due tipi di codici sovrapposti:

- **Codici di spreading** (**OVSF** - _Orthogonal Variable Spreading Factor_): codici ortogonali di Walsh-Hadamard, usati per separare gli utenti della stessa cella. Lo _Spreading Factor_ (**SF**) è variabile da 4 a 512, permettendo di adattare il bit rate alle esigenze di QoS.

- **Codici di scrambling** (**PN** - _Pseudo-Noise_): sequenze pseudo-casuali lunghe, usate per distinguere le celle tra loro (ogni cella ha un codice scrambling diverso). Non aumentano la banda ma «colorano» il segnale di ogni cella.

Il segnale finale trasmesso è il seguente:

$$
s(t) = [d(t) \cdot c_{OVSF}(t)] \cdot c_{PN}(t)
$$

### Processing Gain

Il **Processing Gain** (o **Spreading Gain**) rappresenta il guadagno in immunità all'interferenza ottenuto dallo spreading:

$$
G_p = \frac{\text{Chip Rate}}{\text{Bit Rate}} = \text{SF}
$$

In [[Sistemi mobile#3G - UMTS / WCDMA|UMTS]], con chip rate fisso a **3.84 Mcps**:

| Servizio    | Bit Rate  | SF         | Processing Gain |
| ----------- | --------- | ---------- | --------------- |
| Voce        | 12.2 kbps | 256        | ~25 dB          |
| Dati lenti  | 64 kbps   | 64         | ~18 dB          |
| Dati veloci | 384 kbps  | ~8         | ~10 dB          |
| HSDPA       | 3.84 Mbps | 16 (fisso) | ~6 dB           |

A differenza di [[Sistemi mobile#2G - GSM (Global System for Mobile Communications)|GSM]], dove il numero di utenti per cella è rigidamente limitato dal numero di time slot/frequenze, nel CDMA **il limite non è discreto**: ogni utente aggiuntivo aggiunge un po' di interferenza, degradando gradualmente la qualità. La capacità è quindi determinata dal rapporto segnale/interferenza minimo accettabile ($E_b/N_0$​) e dal numero di utenti attivi.

### Controllo della Potenza

Il **problema near-far** è il difetto critico del CDMA: un utente vicino alla base station, trasmettendo a potenza elevata, può coprire i codici degli utenti lontani, rendendo impossibile la loro decodifica. La soluzione è il **Power Control**, che in W-CDMA opera a **1500 Hz** (ogni 0.667 ms):

- **Open Loop**: il dispositivo stima la perdita di percorso misurando la potenza ricevuta in downlink e regola la potenza in uplink di conseguenza
- **Inner Loop (Closed Loop)**: la base station misura il $E_b/N_0$​ ricevuto dall'UE e invia comandi TPC (Transmit Power Control) di 1 bit ogni slot, ordinando di aumentare o ridurre la potenza di 1 dB
- **Outer Loop**: regola la soglia target del $E_b/N_0$​ dell'Inner Loop in base al BER (Bit Error Rate) misurato, garantendo la qualità del servizio

L'obiettivo è che **tutti gli utenti arrivino alla base station con la stessa potenza**, indipendentemente dalla distanza.

### Soft Handover

Il CDMA abilita nativamente il **Soft Handover**, impossibile in [[Sistemi mobile#2G - GSM (Global System for Mobile Communications)|GSM]] e TDMA. Poiché tutte le celle usano le stesse frequenze, un UE ai bordi di una cella può ricevere contemporaneamente lo stesso segnale da più base station usando codici di scrambling diversi.

Il terminale esegue un **combining** delle copie ricevute (tecnica **MRC** - _Maximum Ratio Combining_), migliorando la qualità del segnale invece di subirne l'interruzione. In [[Sistemi mobile#2G - GSM (Global System for Mobile Communications)|GSM]], per fare handover, bisognava rilasciare il time slot della cella sorgente e acquisirne uno nuovo nella cella destinazione, causando una breve interruzione.
