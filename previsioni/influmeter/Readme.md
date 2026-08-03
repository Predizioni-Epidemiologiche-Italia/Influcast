[![English](https://img.shields.io/badge/lang-English-blue)](Readme.md) [![Italiano](https://img.shields.io/badge/lang-Italiano-green)](Readme.it.md)

# InfluMeter Index & MEM Band Probabilities

This document describes, mathematically, how we compute the InfluMeter index and the MEM band probabilities from an ensemble forecast's predictive quantiles.

## 1. Inputs

For a given forecasting round, location, and horizon, the ensemble provides 23
quantiles of the predictive distribution of incidence (cases per 1,000 assisted
patients):

$$
q \in Q = \{0.01,\ 0.025,\ 0.05,\ 0.10,\ 0.15,\ \dots,\ 0.90,\ 0.95,\ 0.975,\ 0.99\}
$$

with corresponding incidence values $v(q)$. By definition of a quantile,
$v(q)$ is the value such that $P(X \le v(q)) = q$, where $X$ is the (unknown)
true incidence.

## 2. MEM activity bands

The MEM method partitions the incidence axis into 5 ordered bands using 4
season-specific thresholds $t_1 < t_2 < t_3 < t_4$:

| Level         | Range                  |
|---------------|-------------------------|
| `baseline`    | $[0,\ t_1)$            |
| `low`         | $[t_1,\ t_2)$          |
| `medium`      | $[t_2,\ t_3)$          |
| `high`        | $[t_3,\ t_4)$          |
| `very_high`   | $[t_4,\ +\infty)$      |


## 3. From discrete quantiles to a continuous CDF

To compute the probability of $X$ falling inside a band, we need the CDF's
value at the band boundaries $t_1, \dots, t_4$ — but we only know the CDF at
the 23 quantile points in $Q$, and a given threshold generally falls *between*
two of them.

**Step 1 — restrict to the available spread.** We use the full published
range, $q \in [q_{\min}, q_{\max}] = [0.01, 0.99]$ (an interval covering 98%
of probability mass under $Q$'s own indexing). Per the modeling choice made
for this pipeline, this 98% span is treated as the *entire* distribution: no
attempt is made to model or redistribute the missing 2% in the tails.

**Step 2 — rescale to a proper CDF, $F: \mathbb{R} \to [0, 1]$.**

$$
F(v(q)) = \frac{q - q_{\min}}{q_{\max} - q_{\min}}, \qquad q \in [q_{\min}, q_{\max}]
$$

so that $F(v(0.01)) = 0$ and $F(v(0.99)) = 1$ by construction.

**Step 3 — interpolate linearly between known points.** For any incidence
value $x$ that falls between two consecutive known quantile values
$v(q_i) \le x \le v(q_{i+1})$:

$$
F(x) = F(v(q_i)) + \big(F(v(q_{i+1})) - F(v(q_i))\big) \cdot
\frac{x - v(q_i)}{v(q_{i+1}) - v(q_i)}
$$

This is equivalent to assuming the probability density is constant within
each inter-quantile interval — i.e. the CDF is **piecewise-linear** between
the 23 known points. Outside the known range, $F$ is clamped:
$F(x) = 0$ for $x \le v(0.01)$, and $F(x) = 1$ for $x \ge v(0.99)$.

## 4. Band probabilities

The probability (in %) of the true incidence falling in a given band is the
increase in $F$ across that band's boundaries:

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
P(\text{very\_high}) = \big(1 - F(t_4)\big) \times 100
$$

By construction of $F$ (a proper CDF running from 0 to 1 over the 98% span),
these 5 probabilities sum to exactly 100% — up to floating-point error, which
is corrected by a final renormalization: each $P(\text{level})$ is divided by
$\sum_{\text{level}} P(\text{level})$ and multiplied by 100.

## 5. InfluMeter index

While the band probabilities describe the *whole* predictive distribution,
the InfluMeter index is a single point summary: it maps the **median forecast**
(the value at $q = 0.5$) onto a continuous 0-100 scale, using the same
5 MEM bands, each assigned an equal-width 20-point slice:

$$
\text{wks} = [0, 20, 40, 60, 80, 100]
$$

For the median incidence value $m = v(0.5)$, find the band $\ell$ (with index
$i$ in `baseline, low, medium, high, very_high`) such that $m \in [t_i, t_{i+1})$
(with $t_0 = 0$ and $t_5 = +\infty$), and linearly interpolate within that
band's 20-point slice:

$$
\text{index}(m) = \text{wks}_i + \big(\text{wks}_{i+1} - \text{wks}_i\big) \cdot
\frac{m - t_i}{t_{i+1} - t_i}
$$

So, for example, an index of 0 means the median forecast sits at the very
bottom of `baseline` (incidence = 0); an index of 50 means it sits exactly
halfway through `medium`; an index of 100 means it's at or above the
`very_high` threshold $t_4$. This is implemented by `get_influmeter_index()`.

Note that the InfluMeter index reflects only the median of the distribution
— it is a point estimate, distinct from (though generally consistent with)
the band probabilities computed in Section 4, which reflect the full
predictive spread. A high `p_low` alongside an index just above 20 (barely
into `low`) is an example of these two views agreeing: most of the mass is
just past the `baseline`/`low` boundary.

## 6. Worked example

Suppose, for one location and horizon, the median forecast is $m = 6.42$ and
the season's thresholds are $t_1=7.22$, so $m$ falls in `baseline` ($i=0$):

$$
\text{index}(6.42) = 0 + (20 - 0) \cdot \frac{6.42 - 0}{7.22 - 0} \approx 17.8
$$

Meanwhile the full quantile set might give, after interpolation,
$F(7.22) \approx 0.926$, so:

$$
P(\text{baseline}) \approx 92.6\%, \qquad P(\text{low}) \approx 7.4\%
$$

(all other bands $\approx 0\%$) — consistent with the median sitting well
inside `baseline`, close to but not past its upper edge.


