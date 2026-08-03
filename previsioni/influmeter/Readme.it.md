[![English](https://img.shields.io/badge/lang-English-blue)](Readme.md) [![Italiano](https://img.shields.io/badge/lang-Italiano-green)](Readme.it.md)

# Indice InfluMeter & Probabilità delle Fasce MEM

Questo documento descrive, in termini matematici, come calcoliamo l'indice
InfluMeter e le probabilità delle fasce MEM a partire dai quantili
predittivi di una previsione d'insieme (ensemble).

## 1. Input

Per un dato round di previsione, località e orizzonte temporale, l'ensemble
fornisce 23 quantili della distribuzione predittiva dell'incidenza (casi per
1.000 assistiti):

$$
q \in Q = \{0.01,\ 0.025,\ 0.05,\ 0.10,\ 0.15,\ \dots,\ 0.90,\ 0.95,\ 0.975,\ 0.99\}
$$

con i corrispondenti valori di incidenza $v(q)$. Per definizione di
quantile, $v(q)$ è il valore tale che $P(X \le v(q)) = q$, dove $X$ è la
(sconosciuta) incidenza reale.

## 2. Fasce di attività MEM

Il metodo MEM suddivide l'asse dell'incidenza in 5 fasce ordinate usando 4
soglie specifiche per stagione $t_1 < t_2 < t_3 < t_4$:

| Livello       | Intervallo             |
|---------------|-------------------------|
| `baseline`    | $[0,\ t_1)$            |
| `low`         | $[t_1,\ t_2)$          |
| `medium`      | $[t_2,\ t_3)$          |
| `high`        | $[t_3,\ t_4)$          |
| `very_high`   | $[t_4,\ +\infty)$      |


## 3. Dai quantili discreti a una CDF continua

Per calcolare la probabilità che $X$ ricada all'interno di una fascia,
abbiamo bisogno del valore della CDF ai confini di fascia $t_1, \dots, t_4$
— ma conosciamo la CDF solo nei 23 punti quantile di $Q$, e una data soglia
generalmente cade *tra* due di essi.

**Passo 1 — restringere all'intervallo disponibile.** Usiamo l'intero
intervallo pubblicato, $q \in [q_{\min}, q_{\max}] = [0.01, 0.99]$ (un
intervallo che copre il 98% della massa di probabilità secondo l'indicizzazione
propria di $Q$). Per la scelta modellistica adottata in questa pipeline,
questo intervallo del 98% viene trattato come l'*intera* distribuzione:
non si tenta di modellare o ridistribuire il 2% mancante nelle code.

**Passo 2 — riscalare a una CDF propria, $F: \mathbb{R} \to [0, 1]$.**

$$
F(v(q)) = \frac{q - q_{\min}}{q_{\max} - q_{\min}}, \qquad q \in [q_{\min}, q_{\max}]
$$

in modo che $F(v(0.01)) = 0$ e $F(v(0.99)) = 1$ per costruzione.

**Passo 3 — interpolare linearmente tra i punti noti.** Per qualsiasi
valore di incidenza $x$ che cada tra due valori quantile noti consecutivi
$v(q_i) \le x \le v(q_{i+1})$:

$$
F(x) = F(v(q_i)) + \big(F(v(q_{i+1})) - F(v(q_i))\big) \cdot
\frac{x - v(q_i)}{v(q_{i+1}) - v(q_i)}
$$

Ciò equivale ad assumere che la densità di probabilità sia costante
all'interno di ciascun intervallo tra quantili — cioè che la CDF sia
**lineare a tratti** tra i 23 punti noti. Al di fuori dell'intervallo noto,
$F$ viene troncata: $F(x) = 0$ per $x \le v(0.01)$, e $F(x) = 1$ per
$x \ge v(0.99)$.

## 4. Probabilità delle fasce

La probabilità (in %) che l'incidenza reale ricada in una data fascia è
l'incremento di $F$ tra i confini di quella fascia:

$$
P(\text{baseline}) = \big(F(t_1) - F(0)\big) \times 100
$$

$$
P(\text{low}) = \big(F(t_2) - F(t_1)\big) \times 100
$$

$$
P(\text{medium}) = \big(F(t_3) - F(t_2)\big) \times 100
$$

$$
P(\text{high}) = \big(F(t_4) - F(t_3)\big) \times 100
$$

$$
P(\text{very high}) = \big(1 - F(t_4)\big) \times 100
$$

Per costruzione di $F$ (una CDF propria che va da 0 a 1 sull'intervallo del
98%), queste 5 probabilità sommano esattamente a 100% — a meno di errori di
virgola mobile, che vengono corretti con una rinormalizzazione finale:
ciascun $P(\text{level})$ viene diviso per
$\sum_{\text{level}} P(\text{level})$ e moltiplicato per 100.

## 5. Indice InfluMeter

Mentre le probabilità di fascia descrivono l'*intera* distribuzione
predittiva, l'indice InfluMeter è un riassunto puntuale: mappa la
**previsione mediana** (il valore a $q = 0.5$) su una scala continua da
0 a 100, usando le stesse 5 fasce MEM, ciascuna assegnata a una fetta di
pari ampiezza di 20 punti:

$$
\text{wks} = [0, 20, 40, 60, 80, 100]
$$

Per il valore mediano di incidenza $m = v(0.5)$, si trova la fascia $\ell$
(con indice $i$ in `baseline, low, medium, high, very_high`) tale che
$m \in [t_i, t_{i+1})$ (con $t_0 = 0$ e $t_5 = +\infty$), e si interpola
linearmente all'interno della fetta di 20 punti di quella fascia:

$$
\text{index}(m) = \text{wks}_i + \big(\text{wks}_{i+1} - \text{wks}_i\big) \cdot
\frac{m - t_i}{t_{i+1} - t_i}
$$

Ad esempio, un indice pari a 0 significa che la previsione mediana si
trova esattamente alla base di `baseline` (incidenza = 0); un indice di 50
significa che si trova esattamente a metà di `medium`; un indice di 100
significa che è pari o superiore alla soglia $t_4$ di `very_high`. Questo è
implementato dalla funzione `get_influmeter_index()`.

Si noti che l'indice InfluMeter riflette solo la mediana della
distribuzione — è una stima puntuale, distinta (sebbene generalmente
coerente) dalle probabilità di fascia calcolate nella Sezione 4, che
riflettono l'intera dispersione predittiva. Un valore alto di `p_low`
insieme a un indice appena sopra 20 (appena dentro `low`) è un esempio di
come queste due visioni siano coerenti tra loro: la maggior parte della
massa si trova appena oltre il confine tra `baseline` e `low`.

## 6. Esempio pratico

Supponiamo che, per una località e un orizzonte temporale, la previsione
mediana sia $m = 6.42$ e le soglie stagionali siano $t_1=7.22$, per cui $m$
ricade in `baseline` ($i=0$):

$$
\text{index}(6.42) = 0 + (20 - 0) \cdot \frac{6.42 - 0}{7.22 - 0} \approx 17.8
$$

Nel frattempo l'intero insieme di quantili potrebbe dare, dopo
interpolazione, $F(7.22) \approx 0.926$, quindi:

$$
P(\text{baseline}) \approx 92.6\%, \qquad P(\text{low}) \approx 7.4\%
$$

(tutte le altre fasce $\approx 0\%$) — coerente con una mediana che si
colloca ben dentro `baseline`, vicina al proprio limite superiore ma senza
superarlo.
