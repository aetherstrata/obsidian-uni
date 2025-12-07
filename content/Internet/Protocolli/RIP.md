---
tags:
  - protocollo
  - instradamento
  - reti
---
Il protocollo **RIP** è un [[Instradamento#Protocolli|protocollo di instradamento]] IGP storico introdotto negli anni '80 nello stack TCP/IP, basato su algoritmo [[distance vector]], con invio periodico ogni 30 secondi del vettore delle distanze ai router vicini. La metrica è unicamente il numero di hop, con massimo 15 salti, il che lo rende adatto solo a reti di piccole dimensioni e con topologia relativamente stabile. Queste caratteristiche lo rendono un protocollo lento nella convergenza ma molto facile da configurare.

Ogni pacchetto del protocollo **RIP** contiene:
- il comando (*richiesta* o *risposta*)
- la versione (*RIPv1* è classfull, *RIPv2* usa le netmask)
- una lista di subnet descritte da indirizzi IP e subnet mask (solo se *RIPv2*)

I pacchetti di *richiesta* chiedono ai vicini i rispettivi [[distance vector]].
I pacchetti di *risposta* trasferiscono i [[distance vector]] richiesti.

>[!warning] Evitare i sincronismi
>La temporizzazione prevede l’aggiunta di *jitter* ai 30 secondi per evitare sincronismi globali e l’invalidazione di un prefisso se non aggiornato per un certo intervallo (ad esempio 180 secondi).
