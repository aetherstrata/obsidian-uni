---
tags:
  - proprietà
  - sistema
---
Un [[Sistema|sistema]] si dice **causale** se la sua uscita, all'istante $t_0$, dipende solo dagli ingressi applicati prima.
In senso fisico questo significa che l'uscita attuale del sistema non può dipendere da ingressi futuri.
$$y(t_0)\ \text{dipende da}\ x(t)\ \text{per}\ t \le t_0$$
## Esempi

### Sistema causale

Dato il sistema rappresentato dalla seguente funzione,
$$y[n] = \frac{1}{2}x[n] + \frac{5}{2}x[n-2]$$
si può notare come tutti gli ingressi siano $n \le 0$ quindi si può affermare che il sistema è **causale**.

### Sistema non causale

Dato il sistema rappresentato dalla seguente funzione,
$$y[n] = \frac{1}{4}x[n-1] + \frac{1}{2}x[n+1] - \frac{2}{5}y[n-1]$$
si può notare come alcuni ingressi siano $n > 0$ quindi si può affermare che il sistema è **non causale**.