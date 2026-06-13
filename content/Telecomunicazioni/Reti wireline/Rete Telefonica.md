---
tags:
  - reti
  - infrastruttura
  - wireline
  - internet
---

## Rete di accesso

### Infrastruttura TIM

La rete di accesso in rame italiana è stata installata dalla **SIP** a partire dagli anni '60 e successivamente gestita da _Telecom Italia_ (ora **TIM**). La sua caratteristica fondamentale è la topologia **punto-punto**: ogni abitazione ha un proprio doppino di rame dedicato che arriva fino alla centrale telefonica, senza condivisione del mezzo fisico tra utenti.

> [!warning] Dismessa
> La rete in rame di TIM è in fase di _decommissioning_ e tutti i servizi offerti su essa verranno progressivamente migrati verso la rete in fibra ottica.

#### Struttura gerarchica

La rete TIM in rame si articolava nei seguenti livelli gerarchici:

| Elemento                       | Sigla        | Numero in Italia | Descrizione                                                 |
| ------------------------------ | ------------ | ---------------- | ----------------------------------------------------------- |
| **Stadio di Gruppo Urbano**    | SGU          | 628              | Aggregatore metropolitano, corrisponde al Point of Presence |
| **Stadio di Linea**            | SL           | 10.313           | Centrale telefonica locale con MDF                          |
| **Main Distribution Frame**    | MDF          | ~10.313          | Permutatore in centrale, con pressurizzazione               |
| **Armadio di distribuzione**   | SDF          | 140.000          | Punto di giunzione tra rete primaria e secondaria           |
| **Distribution Frame esterno** | DF (box)     | 3,9 milioni      | Box di distribuzione su strada/edificio                     |
| **Distribution Frame interno** | DF (interno) | 1,5 milioni      | Box di distribuzione interno all'edificio                   |

![[Pasted image 20260607211238.png]]

#### Sezioni della rete

La rete di distribuzione si divide in tre segmenti fisici con lunghezze standard:

1. **Rete di giunzione**: dalla centrale (SL) allo SGU, in fibra ottica
2. **Rete di distribuzione primaria**: dalla centrale all'armadio di distribuzione (**1–1,2 km**) cavi ad alta potenzialità interrati in tubazione
3. **Rete di distribuzione secondaria**: dall'armadio al box esterno (**200–300 m**) cavi a bassa potenzialità in trincea o aerea
4. **Raccordo**: dal box esterno alla borchia in casa del cliente (**50–70 m**)

![[Pasted image 20260607222111.png]]

#### Struttura della centrale

All'interno della centrale si trovano:

- **Sala MDF** (_Main Distribution Frame_): permutatore principale che connette i cavi in arrivo dalla rete primaria con gli apparati attivi (DSLAM, commutatori)
- **Sala muffole**: terminazione e giunzione dei cavi in ingresso
- **Cunicoli e camerette**: infrastruttura interrata per il passaggio dei cavi
- **Pressurizzazione**: i cavi vengono mantenuti a pressione positiva per rilevare eventuali infiltrazioni d'acqua

### Tecnologie

Il [[Doppino telefonico#Doppino telefonico|doppino telefonico]] ha ospitato nel tempo tecnologie sempre più avanzate, sfruttando progressivamente una banda di frequenze sempre più ampia.

#### POTS (Plain Old Telephone System)

Trasmissione **analogica** nella banda **300–3400 Hz** (larghezza di banda utile: 3,1 kHz). Il segnale vocale viaggiava come differenza di potenziale analogica direttamente fino alla centrale.

Le vecchie reti **PSTN** (_Public Switched Telephone Network_) usavano questa tecnologia per instradare il segnale voce usando una **commutazione di circuito** analogica. Questa tecnologia può inviare segnali per diversi km senza rigeneratori e può arrivare ad una massima velocità di trasmissione di **56 Kbps** in dial-up.

> [!warning] Dismessa
> Non è più usabile in quanto è stata dismessa per direttiva europea intorno al 2025 e tutti i collegamenti telefonici ora viaggiano su **VoIP**.

#### ISDN (Integrated Services Digital Network)

La tecnologia _ISDN_ è la prima rete **digitale** sul doppino, introdotta negli anni '90. Utilizza la codifica **2B1Q** (2 Binary 1 Quaternary): ogni simbolo codifica **2 bit** usando **4 livelli di ampiezza** ($\pm 2.5\ V$ e $\pm0.833\ V$), per un totale di **80 kbaud -> 160 kbit/s**. Il servizio Basic Rate Interface (**BRI**) prevede 2 canali B da 64 kbit/s ciascuno + 1 canale D da 16 kbit/s per la segnalazione, per un totale di **128 kbit/s utili**. La distanza massima è circa **5 km**.

#### ADSL fino a G.fast

Per questi argomenti fare riferimento alla pagina sul [[Discrete Multi Tone]].

### DSLAM

l **DSLAM** (_Digital Subscriber Line Access Multiplexer_) è l'apparato attivo della rete di accesso che si interpone tra i doppini degli utenti e la rete dell'operatore. In ADSL classico era installato nella **centrale telefonica**, mentre con il passaggio a VDSL2 e G.fast, viene spostato nell'**armadio di distribuzione** stradale per ridurre la lunghezza del tratto in rame.

#### Funzioni

- Termina le singole linee DSL degli utenti (una porta per ogni doppino)
- Aggrega il traffico di decine o centinaia di utenti con tecniche di **multiplexing** (ATM, Frame Relay o IP)
- Instradata il traffico aggregato verso la dorsale dell'operatore a velocità tipiche di **10 Gbit/s**
- Gestisce il **bit loading** adattivo, misurando il SNR su ogni sottoportante durante la fase di training

### Splitter

Lo **splitter** è un filtro analogico passivo (installato sia a casa che in centrale) che separa la banda voce (0 - 4 kHz) dalla banda dati, permettendo la coesistenza di POTS e ADSL sullo stesso doppino. Il **NID** (_Network Interface Device_) è il punto di demarcazione tra la rete dell'operatore e l'impianto interno del cliente.
