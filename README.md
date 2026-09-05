> **Note.** This entire repository was machine-written by AI assistants at the direction of Tom Adamczewski. The Lean proof itself was written by GPT-6 Astra, as described below.

# Ibragimov–Iosifescu conjecture for φ-mixing sequences

The counterexample construction and Lean disproof in this repository were obtained by **GPT-6 Astra**. The repository preserves the generated proof, recorded verification results, and subsequent assessments of misformalization risk and novelty.

For background and the conjecture's statement, see [the Wikipedia article on the Ibragimov–Iosifescu conjecture for φ-mixing sequences](https://en.wikipedia.org/wiki/Ibragimov%E2%80%93Iosifescu_conjecture_for_%CF%86-mixing_sequences).

The Lean disproof addresses Ibragimov's central limit theorem claim. If correct, it also rules out Iosifescu's stronger claim that the rescaled partial-sum process converges to Brownian motion: that convergence would imply the central limit theorem at time 1.

The recorded verifier accepted the disproof with Lean's default kernel using only standard axioms.

The [literature review](LITERATURE_REVIEW.md) compares the result with earlier work and documents the search for a prior resolution through 2026-09-05. It found no earlier counterexample satisfying all the conjecture's hypotheses and identifies a 2023 scholarly source explicitly reporting the conjecture as open.

## Informal summary of the argument

Write $S_n=\sum_{t=0}^{n-1}X_t$ and $\sigma_n^2=\operatorname{Var}(S_n)$. The construction makes $S_n/\sigma_n$ converge to **zero in probability along selected times** $n_j\to\infty$, while satisfying the conjecture's assumptions. The idea is to suppress the fluctuations seen on most sample paths while retaining extremely rare, large fluctuations that contribute substantially to the variance.

Start with independent, identically distributed innovations $\xi_t$, each consisting of a standard Gaussian plus a symmetric random spike. A spike has size $\pm Q_j$ with total probability $p_j$, where

$$
p_jQ_j^2=2^{-(j+1)}.
$$

Thus the spike sizes can be made enormous and their probabilities tiny while their total second moment remains finite. Define

$$
X_t=\xi_t+K(\xi_{t-1},\xi_{t-2},\ldots),
\qquad |K|\le\tfrac18.
$$

The correction $K$ is odd and uses the same rule at every time. Symmetry gives mean zero, the bounded correction preserves finite second moments, and applying a fixed rule to the shifted innovation sequence gives strict stationarity.

**Suppress ordinary fluctuations in stages.** Build $K$ from a sequence of small, bounded feedback corrections acting over increasingly long windows. At stage $j$, first consider a “core” process containing only the Gaussian noise and the earlier spike sizes. Add enough feedback to reduce its partial-sum variance relative to the number of terms, then choose an observation time $n_j$. Only afterward choose the next spike size $Q_j>16n_j$, with probability small enough that it rarely affects an observation of that length. Later corrections and spike probabilities are chosen small enough to preserve the earlier comparisons. This produces one limiting process whose sum at each selected time has almost the same distribution as the corresponding low-variance core sum.

**Keep the variance divergent.** A correction bounded by $1/8$ can change an $n$-term innovation sum by at most $n/8$. Spikes much larger than $n$ therefore cannot be cancelled within that observation window. The proof turns this into a lower bound on $\operatorname{Var}(S_n)$ using the second moment contributed by sufficiently large spikes. The spike sizes and probabilities are arranged so that this bound tends to infinity for all $n\to\infty$. At the selected times, the new spike also ensures that the true variance is much larger than the core variance.

**Retain genuine φ-mixing.** The construction also provides a bounded causal inverse, expressing each innovation as $\xi_t=X_t+H(X_{t-1},X_{t-2},\ldots)$. If two output histories agree in their most recent $k$ entries, the difference between their values of $H$ is bounded by $d_k$, with $\sum_k d_k^2<\infty$. The bounded drift and Gaussian smoothing allow processes started from different histories to be coupled so they agree on a recent block; the square-summable bound controls the remaining influence of their older pasts. Together these give forgetting uniformly over the starting histories and the length of the future block. Extending the bound to all future events proves the required φ-mixing condition, including conditioning on arbitrarily rare past events.

**Compare typical size with standard deviation.** Let $v_j$ be the core sum's variance and $\varepsilon_j$ a uniform bound on the difference between event probabilities under the true and core sum laws. The construction gives $\varepsilon_j\to0$ and $v_j/\sigma_{n_j}^2\to0$. Since the core sum is centered, Chebyshev's inequality yields, for every $a>0$,

$$
\mathbb P\!\left(\left|\frac{S_{n_j}}{\sigma_{n_j}}\right|>a\right)
\le \varepsilon_j+\frac{v_j}{a^2\sigma_{n_j}^2}
\longrightarrow 0.
$$

Consequently the normalized sums converge along these times to a point mass at zero, which rules out convergence of the full sequence to $N(0,1)$. There is no contradiction with their having variance one: increasingly rare, large values retain the second moment even as the probability of any fixed nonzero-sized fluctuation tends to zero.

## Contents

- `Challenge.lean` - the formal problem statement (docstring, definitions, the `sorry` theorem, and the `.disproof` companion).
- `submission/` - the agent's kernel-checked submission (`Spec.lean` plus any auxiliary modules).
- `info.json` - recorded verification output.
- `final_messages.md` - the agent's own closing notes (untrusted self-report).
- `ANALYSIS.md` - independent analysis of correctness and misformalization risk.
- [LITERATURE_REVIEW.md](LITERATURE_REVIEW.md) - literature search, comparisons with earlier results, sources, and limitations of the novelty assessment.
- [LITERATURE_CITATIONS.tsv](LITERATURE_CITATIONS.tsv) - the 41 OpenAlex citation records screened for works citing Peligrad's 1990 paper.
