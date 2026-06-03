---
tags:
  - segnale
  - digitale
  - sistema
---
Il **companding** è una tecnica di quantizzazione non lineare, usata soprattutto nella telefonia [[Pulse Code Modulation|PCM]], per migliorare la qualità percepita dei [[segnale|segnali]] deboli senza aumentare il bitrate complessivo. L'idea è comprimere dinamicamente il segnale prima della [[quantizzazione]] uniforme e riespanderlo in ricezione, così da riservare più risoluzione ai campioni di piccola ampiezza, che altrimenti soffrirebbero di più il rumore di quantizzazione.
## Funzionamento

Nella [[Pulse Code Modulation|PCM]] uniforme tutti i livelli di quantizzazione sono equispaziati, quindi l'errore di quantizzazione assoluto è circa costante su tutto il range dinamico. Questo però penalizza i segnali piccoli, perché lo stesso errore rappresenta una frazione molto maggiore del [[segnale]] utile e quindi produce un [[Trasmissione Digitale#SNR|SNR]] peggiore alle basse ampiezze. Il companding risolve il problema rendendo la quantizzazione più fine vicino allo zero e più grossolana per ampiezze elevate.

>[!info] Valori dei livelli di quantizzazione logaritmica
>![[Pasted image 20260603213201.png]]

- In **trasmissione** il segnale analogico viene prima fatto passare in un compressore non lineare, tipicamente con andamento logaritmico, poi viene campionato e quantizzato in modo uniforme. 
- In **ricezione** il processo viene invertito: dopo la decodifica PCM, un espansore applica la funzione inversa e ricostruisce una versione del segnale con dinamica riportata alla scala originale. 

>[!tip] Realizzazione
Il sistema si comporta come se si usasse un quantizzatore non uniforme, pur mantenendo una realizzazione semplice tramite compressione + quantizzazione uniforme + espansione.
### Effetto su SNR 

Il vantaggio principale è che i segnali a bassa ampiezza vengono spostati prima della quantizzazione ad ampiezze più alte, quindi cadono su livelli con meno rumore e subiscono un errore relativo minore.

Per i segnali con grande ampiezza, invece, si accetta una granularità più grossa perché l'errore di quantizzazione incide proporzionalmente meno sul segnale. Il risultato è un rapporto segnale-rumore più uniforme lungo il range dinamico della voce, che è proprio il motivo per cui il companding è stato adottato nei sistemi telefonici PCM.
## μ-law e A-law

Nello standard ITU-T G.711, le due leggi di companding sono *μ-law* e *A-law*:

- La **μ-law** è usata in Nord America e Giappone
- La **A-law** è adottata in Europa e nelle connessioni internazionali in cui almeno uno dei paesi usa questo standard. 

Entrambe codificano il campione compresso in 8 bit a 8 kHz, portando al bitrate di 64 kb/s della telefonia PCM.
### Differenze

La differenza principale tra A-law e μ-law sta nella curva di compressione, quindi nel compromesso tra dinamica, distorsione e comportamento sui piccoli segnali. In generale, μ-law offre un SNR leggermente migliore per segnali deboli, mentre A-law tende ad avere un profilo di distorsione più uniforme e per questo è stata preferita in Europa. 

>[!warning] Interoperabilità
> Le due codifiche non sono direttamente compatibili, quindi nelle interconnessioni servono conversioni dedicate tra un formato e l'altro.

Indicando con $x$ il campione normalizzato in ingresso, la legge μ-law è comunemente espressa come segue, con $\mu = 255$ nello standard telefonico nordamericano e giapponese.
$$
y(x)=\operatorname{sgn}⁡(x)\frac{\ln{(1+\mu\left|x\right|)}}{\ln{(1+\mu)}}​
$$
La A-law è invece definita a tratti e usa il parametro $A\simeq87.6$ nello standard europeo.
$$
y(x)=\operatorname{sgn}(x) \begin{cases}
\dfrac{A\left|x\right|}{1+\ln A} & ,\ \left|x\right| \lt \dfrac{1}{A} \\ \\
\dfrac{1+\ln (A\left|x\right|)}{1+\ln A} & , \ \dfrac{1}{A}\le \left|x\right| \le 1
\end{cases}
$$

>[!info] Confronto della compressione con A-law e μ-law
>![[Pasted image 20260603214155.png|bg-white]]
