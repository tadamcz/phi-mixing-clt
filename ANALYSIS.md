# IbragimovIosifescuConjectureForMixingSequences.ibragimov_iosifescu_conjecture_for_mixing_sequences.parts.i

**Verdict:** C-possibly-new  (confidence: high that the formal statement faithfully renders the informal conjecture and that the kernel-checked object is a genuine counterexample to it; the residual risks for "this settles the open problem" are only Lean/Mathlib soundness and an oversight in this audit)

**Claim:** disproof

Scorer: `C`, stage `comparator`, claim `disproof`; Lean default kernel accepts; exported axioms for the
`.disproof` constant are exactly `propext, Classical.choice, Quot.sound`. Agent: 1054 turns, ~12 h, $261;
submission `Spec.lean` is 12,900 lines (864 theorems/lemmas, 187 defs/structures, 34 concatenated modules
`Cascade.*`).

## Informal conjecture (and literature status)

Ibragimov (Ibragimov–Linnik 1971, p. 393, problem 3): if $(X_n)$ is strictly stationary, $\varphi$-mixing,
$\mathbb E X_0 = 0$, $\mathbb E X_0^2<\infty$ and $\sigma_n^2=\mathrm{Var}(S_n)\to\infty$, then
$S_n/\sigma_n \Rightarrow N(0,1)$. Iosifescu (1977) conjectured the weak invariance principle under the same
hypotheses (that is `parts.ii`; this sample is the CLT, `parts.i`). Here
$\varphi(n)=\sup_m\sup\{|P(B\mid A)-P(B)| : A\in\sigma(X_j,j\le m),\,P(A)>0,\,B\in\sigma(X_j,j\ge m+n)\}$.

Status: open, as far as the literature I could reach goes.

- Bradley, *Basic properties of strong mixing conditions. A survey and some open questions*
  (Probability Surveys 2005, arXiv math/0511078, §2.3; fetched and grepped): "In the 1960's, I.A. Ibragimov
  conjectured that if a given strictly stationary sequence ... is φ-mixing, has finite second moments, and
  satisfies Var(X_1+...+X_n) → ∞ ..., then it satisfies a CLT. ... Iosifescu conjectured that under the same
  hypothesis, a weak invariance principle holds. **These conjectures remain unsolved.** Peligrad has confirmed
  them under the augmented hypothesis liminf n^{-1} Var(S_n) > 0."
- zbMATH review of Peligrad 1990 (SPA 35, 293–308; fetched via the zbMATH API): "It is not yet known whether
  there exists a strictly stationary φ-mixing sequence with lim σ_n² = ∞ and liminf σ_n²/n = 0. If such a
  sequence exists, then, as shown by N. Herrndorf [1983], Conjecture 2 [Iosifescu's WIP] is not true."
  Peligrad 1990 proves a *related* conjecture of hers under a regular-variation tail assumption; it does not
  settle Ibragimov's.
- Known partial results: Ibragimov 1975 (CLT if additionally $\mathbb E|X_0|^{2+\delta}<\infty$);
  Peligrad 1985 (CLT and WIP if additionally $\liminf \sigma_n^2/n>0$).
- Wikipedia (raw article fetched) lists it as an open conjecture. Web searches for a 2023–2026 resolution or
  counterexample returned nothing.

So the *only* open case is $\sigma_n^2\to\infty$ with $\liminf\sigma_n^2/n=0$, and nobody had even exhibited a
strictly stationary $\varphi$-mixing sequence in that regime.

## What the formal statement actually says (plain math; hand-rolled definitions)

`Challenge.lean` (identical to `Spec.lean:1–149` up to the `section OriginalStatement` wrapper; verified by
`diff`). Ambient data: `Ω : Type*` (universe-polymorphic), `[MeasurableSpace Ω]`, `X : ℕ → Ω → ℝ`,
`μ : Measure Ω`, `[IsProbabilityMeasure μ]`.

Hand-rolled definitions (all in `Challenge.lean`):

| def | lines | meaning | faithful? |
|---|---|---|---|
| `sigmaGen X s := ⨆ j ∈ s, comap (X j) borel` | 45–46 | $\sigma(X_j : j\in s)$ | yes |
| `IsStrictlyStationary X μ := ∀ k, IdentDistrib (fun ω n ↦ X (n+k) ω) (fun ω n ↦ X n ω) μ μ` | 51–52 | joint law of the whole shifted sequence $(X_{n+k})_n$ equals that of $(X_n)_n$ (product σ-algebra on `ℕ → ℝ`) | yes |
| `phiMixingCoeff X μ n := sSup {r \| ∃ m A B, MeasurableSet[sigmaGen X (Iic m)] A ∧ 0 < μ A ∧ MeasurableSet[sigmaGen X (Ici (m+n))] B ∧ r = \|(μ[\|A]).real B − μ.real B\|}` | 60–62 | $\varphi(n)$ exactly as in the literature (one-sided, sup over $m$, $P(A)>0$); `μ[|A] = (μ A)⁻¹ • μ.restrict A` is the genuine conditional probability | yes |
| `IsPhiMixing X μ := Tendsto (phiMixingCoeff X μ) atTop (𝓝 0)` | 66–67 | $\varphi(n)\to 0$ | yes |
| `partialSum X n ω := ∑ j < n, X j ω` | 74–75 | $S_n$ | yes |
| `partialSumStdDev X μ n := √(variance (partialSum X n) μ)` | 78–79 | $\sigma_n$ | yes |
| `normalizedPartialSum := partialSum / partialSumStdDev` | 82–83 | $S_n/\sigma_n$ | yes |

Theorem `ibragimov_iosifescu_conjecture_for_mixing_sequences.parts.i` (134–143): from
`hX : ∀ n, Measurable (X n)`, `IsStrictlyStationary`, `IsPhiMixing`, `MemLp (X 0) 2 μ`, `μ[X 0] = 0`,
`Tendsto (fun n ↦ variance (partialSum X n) μ) atTop atTop`, conclude
`Tendsto (fun n ↦ ⟨μ.map (normalizedPartialSum X μ n), _⟩ : ProbabilityMeasure ℝ) atTop (𝓝 ⟨gaussianReal 0 1, _⟩)`.
The topology on `ProbabilityMeasure ℝ` is Mathlib's weak-convergence topology (`tendsto_iff_forall_integral_tendsto`:
convergence of $\int f\,d\mu_n$ for every bounded continuous $f$), i.e. genuine convergence in distribution to
$N(0,1)$. The `.disproof` companion (147) is `¬ (type_of% @thm)`, universe-polymorphic in `Ω`.

Junk-value audit (none exploitable):
- Mathlib `variance = (evariance).toReal` is 0 for non-$L^2$ variables (`variance_of_not_memLp`), but `h_L2`
  plus stationarity makes every $X_n$, hence every $S_n$, square-integrable, so `variance` is the true variance.
- `x / 0 = 0` in `normalizedPartialSum` only matters when $\sigma_n=0$, which `h_var` excludes for all large $n$.
- `sSup` on ℝ is 0 on empty/unbounded sets; the set is nonempty ($m=0,A=\Omega,B=\emptyset$) and bounded by 1
  (the submission proves `phiMixingCoeff ∈ [0,1]`, `Spec.lean:12296`).
- One-sided (`ℕ`) vs. two-sided (`ℤ`) indexing: for strictly stationary sequences the one-sided and two-sided
  $\varphi$-coefficients coincide (approximate $A\in\sigma(X_j,j\le m)$ by finite-window events and use
  stationarity), and the formal process is literally the ℕ-restriction of a two-sided stationary process.

Conclusion: the formal statement is a faithful formalization of Ibragimov's conjecture. No bundled cases, no
degenerate instances, no weak/strong quantifier slips.

## What the agent proved and how

Top level (`Spec.lean:12898–12900`):
```lean
theorem ...parts.i.disproof : ¬ (type_of% @...parts.i) := by
  exact IbragimovIosifescuConjectureForMixingSequences.not_unrestricted_clt_of_counterexample
    Cascade.explicit_counterexample
```
`Counterexample X μ` (`12742–12753`) is a `Prop` structure bundling *exactly* the six hypotheses plus
`not_gaussian : ¬ Tendsto … (𝓝 ⟨gaussianReal 0 1, _⟩)`. `not_unrestricted_clt_of_counterexample`
(`12802–12816`) lifts a `Type 0` counterexample to `ULift.{u}` (`Counterexample.lift`, `12775`; transport lemmas
`12428–12735` show σ-algebras, $\varphi$-coefficients, moments and normalized-sum laws are preserved) and applies
the universally quantified CLT to it. `Cascade.explicit_counterexample` (`12888–12892`) is
`Diagonal.construction.counterexample` (`12865–12883`).

### The counterexample process

- Sample space `NoisePath := ℤ → ℝ` (`162`) with product σ-algebra; measure
  `iidLaw ℤ ν := Measure.infinitePi (fun _ ↦ ν)` (`174`) — an i.i.d. two-sided innovation sequence $(\xi_t)_{t\in\mathbb Z}$.
- Innovation law `noise = gaussianConvolution (spikeLaw w Q)` (`5388, 5473, 5520`):
  $\xi = Z + J$, $Z\sim N(0,1)$ independent of $J$, where $J$ is a **rare symmetric spike**:
  $J=\pm Q_j$ with probability $p_j/2$ each, $J=0$ with the remaining mass, with
  $p_j = 2^{-(j+1)}/Q_j^2$ (`geometricSpikeEnergy` `6330`, `forSizes` `6243`). Hence
  $p_jQ_j^2 = 2^{-(j+1)}$ (`energy_eq`, `10339`), $\mathbb E\xi^2 = 2$ (`10335`), law symmetric, and — since the
  $Q_j$ grow super-exponentially — $\mathbb E|\xi|^{2+\delta}=\infty$ for every $\delta>0$.
- Process: `X n := atTime (factor K) n`, i.e. $X_t = \xi_t + K(\xi_{t-1},\xi_{t-2},\dots)$
  (`factor`, `181`; `atTime`, `184`), with `K : Past →ᵇ ℝ` a bounded continuous **odd** correction,
  $\|K\|\le 1/8$ (`10505`, `10513`). $K$ is the limit (`KernelLimit`, `9824`; `kernels`, just before `10505`) of a
  cascade of *clamped moving-window feedback controllers*: for parameters $(N,a,s)$, `update c K` is the fixed
  point of $K'\mapsto K - a\cdot\mathrm{clip}_s(\mathrm{window}_N K')$, where `window N K p` is the sum of the
  last $N$ outputs (`403, 433, 493`; `clip`, `225`). Each controller has gain $aN<1$ and per-step budget $as$.
- The cascade has an explicit **bounded causal inverse** $H$ (`reconstruct`, `10518`; `IsCausalInverse`, `10718`):
  $\xi_t = X_t + H(X_{t-1},X_{t-2},\dots)$, $\|H\|\le 1/8$ (`10509`), and a **square-summable tail-oscillation
  profile**: `HasVariationMajorant H d` (`8041`) means $|H(x)-H(y)|\le d_k$ whenever $x,y$ agree in their first
  $k$ coordinates, with $\sum_m d_m^2 \le 24$ (`10592, 10595`, via the "square-variation cost" bookkeeping
  `cost`/`amplitude`/`DoublingMemory`, `1867–1879`).

### The diagonal choice (`StageChoice`/`Diagonal`, `9870–10637`)

Stage $j$ (`Step`, `9948`; `nonempty_step`, `10000`; assembled by `Nat.rec` in `construction`, `10174`) starts
with controllers `cs j` and spike sizes $Q_0,\dots,Q_{j-1}$ and, **in this order**:
1. appends a batch of controllers so that the *core* process (innovations = Gaussian + spikes $<j$ only) has
   partial-sum $L^2$ slope $\le$ `target j` $=4^{-(j+1)}$ (`exists_attenuating_extension`, `4819`), adding
   square-variation cost $\le$ `error j` $=2^{-(j+1)}$ and amplitude $\le r_j$ ($r_{j+1}\le r_j/4$, $r_0=1/16$,
   so $\|K\|,\|H\|\le 1/8$);
2. picks an observation time $n_j$ with $\mathrm{Var}_{\text{core}}(S_{n_j}) \le 2n_j\,\text{target}_j^2$
   (`UpperL2.exists_ge_variance_le`, `6946`);
3. picks a uniform burn-in $T_j$ for the finite-memory inverse (`exists_burnin_testClose_pathLaw_suffix_of_hasMemory`, `9061`);
4. picks $r_{j+1}\le \min(r_j/4,\ \text{error}_j/(16(T_j+n_j+1)))$;
5. **only then** picks the new spike size $Q_j\ge 4^{j+1}$ with $Q_j>16n_j$ and $p_j=2^{-(j+1)}/Q_j^2\le r_{j+1}$
   (`exists_large_size`, `9979`).
The witness is selected with `Classical.choice` from these proved existence statements (`Recursion.chooseStep`);
this is a legitimate nonconstructive choice of a genuinely existing object, not a vacuous one.

### Verification of the hypotheses (`counterexample`, `12865–12883`)

- Stationarity: causal factor of an i.i.d. sequence, `atTime_factor_stationary` (`1327`).
- $L^2$, mean 0: `memLp_atTime_factor` (`1317`), `integral_atTime_factor_eq_zero` (`1322`) — bounded correction,
  symmetric law, odd $K$.
- **$\varphi$-mixing**: `isPhiMixing_of_causalInverse_correction` (`12389`) ← `isPhiMixing_of_causalInverse`
  (`12356`), a *general* theorem: if $X_t=\xi_t-H(X_{t-1},\dots)$ with $\xi$ = (anything) $*$ $N(0,1)$, $H$
  bounded by $B$ with square-summable tail-oscillation profile, then $X$ is $\varphi$-mixing in the exact sense of
  `Challenge.lean`. Mechanism: one-step Doeblin minorization with weight $e^{-B^2}/\sqrt2$
  (`gaussianMinorizationCoeff`, `5126`; `augmentedStepLaw_minorization`, `8651`) gives "refresh blocks" on which
  both chains agree on a block of $b$ recent outputs; after agreement the two conditional path laws differ only
  by drift shifts $\le d_k$ ($k\ge b$), so a Gaussian half-likelihood (Hellinger) estimate bounds their distance
  by $\sqrt{\sum_{k\ge b}d_k^2}$ (`variationTailBound`, `exists_refresh_blocks` `9006`,
  `exists_burnin_testClose_pathLaw_suffix` `9045`: threshold uniform in the future length and in *both arbitrary
  real pasts*). Finite future blocks generate the future σ-algebra (`sigmaGen_future_eq_iSup`), so the bound
  transfers to the actual coefficient (`phiMixingCoeff_le_of_finite_cond` `12313`,
  `testClose_cond_outputBlock_of_natPast` `11418`).
- **$\mathrm{Var}(S_n)\to\infty$**: `variance_tendsto_atTop` (`10616`) ←
  `tendsto_variance_sumProcess_factor_geometricSpikeNoise` (`6865`) ← `variance_sumProcess_factor_ge_tailEnergy_16n`
  (`6836`): since $|S_n-\sum_{t<n}\xi_t|\le n\|K\|\le n$, spikes of size $>16n$ cannot be cancelled inside the
  window, so $\mathrm{Var}(S_n)\ge \tfrac n4\,\mathbb E[\xi^2\mathbf 1_{|\xi|>16n}]$; with $Q_j\ge 4^{j+1}$
  the tail energy is $\gtrsim (16n)^{-1/2}$, so the bound $\to\infty$.
- **No Gaussian limit**: `Escape.no_gaussian_limit_of_geometric_bounds` (`12096`) with
  * `sum_law_close` (`12842`): the law of $S_{n_j}$ under the true process is within `TestClose` error $2^{-(j+1)}$
    (i.e. every event probability within $2^{-(j+1)}$; `TestClose` def `4847`) of the law of the stage-$j$ *core*
    sum — via `testClose_sumProcess_of_geometric_budget` (`11707`), using latent-law closeness $\le 2r_{j+1}$
    (`10428`), kernel closeness $\le 2r_{j+1}$ (`10552`) and the burn-in (`10318`);
  * `selected_variance_lower` (`10602`): $\mathrm{Var}(S_{n_j}) \ge n_j 2^{-(j+1)}/8$ (spike $j$ alone is
    uncancellable since $Q_j>16n_j$);
  * `variance_core` (`10312`): core variance $\le 2n_j 2^{-4(j+1)}$.
  Chebyshev on the core (`tendsto_normalized_tail_of_geometric_bounds`, `12036`) gives
  $P(|S_{n_j}/\sigma_{n_j}|>a)\le 2^{-(j+1)} + 16\cdot 2^{-3(j+1)}/a^2\to 0$: **$S_{n_j}/\sigma_{n_j}\to 0$ in
  probability** along $n_j\to\infty$. `no_gaussian_limit_of_subseq_zero` (`11741`): a full-sequence Gaussian limit
  would force $N(0,1)=\delta_0$, contradicting `noAtoms_gaussianReal`.

In words: the variance of $S_n$ is carried entirely by rare, huge, i.i.d. spikes that a bounded feedback cannot
cancel within the observation window, while the bulk (Gaussian part and all earlier spikes) has been attenuated
by the feedback cascade; hence $S_n^2/\sigma_n^2$ is not uniformly integrable and the limit along $n_j$ is the
degenerate law $\delta_0$. This is the classical Herrndorf/Bradley mechanism for CLT failure under weaker mixing
conditions; the new ingredient is that a *bounded-drift, Gaussian-smoothed nonlinear autoregression* with
$\ell^2$-summable variation is $\varphi$-mixing no matter how wild the innovation tails are, because the future
depends on the past only through the bounded drift $H$.

### Loophole audit

- No `native_decide`; `decide` only on `(2:ℕ) ≠ 0`; `Fin 0`/`Subsingleton.elim` only in trivial empty-product
  lemmas (`7572`, `2290`, `10904`).
- Single `sorry` in the file is the original conjecture statement (`Spec.lean:145`, required by the format);
  the disproof does not use it (agent's own check; axiom export lists only the three standard axioms).
- `Classical.choice` picks the witness from proved `∃`/`Nonempty` statements (`nonempty_step`,
  `nonempty_kernelLimit` `9836`, `exists_diagonal`) — not vacuous.
- No degenerate parameters: sample space `ℤ → ℝ` with a genuine infinite product probability measure; $\xi$ has
  variance 2; $\|K\|\le 1/8$.
- Statement unchanged (diff against `Challenge.lean` clean; the agent's final self-check also asserts this).

### Sanity checks against known theorems (heuristic, all consistent)

- Ibragimov 1975 ($\mathbb E|X|^{2+\delta}<\infty\Rightarrow$ CLT): here $\mathbb E|\xi|^{2+\delta}
  =\sum_j 2^{-(j+1)}Q_j^\delta=\infty$ for all $\delta>0$. Consistent.
- Peligrad 1985 ($\liminf\sigma_n^2/n>0\Rightarrow$ CLT): here $\mathrm{Var}(S_{n_j})\approx n_j2^{-j}$, so
  $\liminf\sigma_n^2/n=0$. Consistent; the example lives exactly in the one open regime.
- Ibragimov's $\rho$-mixing lemma ($\varphi$-mixing $\Rightarrow\rho$-mixing $\Rightarrow\sigma_{2n}^2/\sigma_n^2\to2$)
  forces the cancellation of each spike to be spread over many dyadic time scales; the doubling-memory cascade
  with geometrically decaying amplitudes and summable square-variation cost is precisely what allows a slow
  (rate $\le d_k$, $\sum d_k^2<\infty$) cancellation. I found no contradiction.

## Faithfulness assessment

I find no divergence between the formal statement and the informal conjecture: strict stationarity, the
$\varphi$-coefficient (including $P(A)>0$ and the sup over $m$), centering, finite second moment,
$\mathrm{Var}(S_n)\to\infty$, and convergence in distribution to $N(0,1)$ are all the standard notions.
The disproof therefore is a counterexample to the *informal* conjecture, not merely to the formalization.

## Mathematical significance

If the Lean development is sound (it type-checks in the default kernel with only `propext`,
`Classical.choice`, `Quot.sound`), the agent has proved the following theorem:

> There exists a strictly stationary sequence $(X_t)_{t\in\mathbb Z}$ of real random variables, of the form
> $X_t=\xi_t+K(\xi_{t-1},\xi_{t-2},\dots)$ with $(\xi_t)$ i.i.d. (law: $N(0,1)$ convolved with a symmetric
> rare-spike law of variance 1) and $K$ bounded continuous odd, which is $\varphi$-mixing, has
> $\mathbb E X_0=0$, $\mathbb E X_0^2=2<\infty$ and $\mathrm{Var}(S_n)\to\infty$, and for which
> $S_{n_j}/\sigma_{n_j}\to 0$ in probability along a sequence $n_j\to\infty$; in particular
> $S_n/\sigma_n\not\Rightarrow N(0,1)$.

This would (i) refute Ibragimov's 1971 conjecture, (ii) a fortiori refute Iosifescu's WIP conjecture (`parts.ii`),
and (iii) answer negatively the question recorded in Peligrad 1990 / Bradley 2005 by exhibiting a strictly
stationary $\varphi$-mixing sequence with $\sigma_n^2\to\infty$ and $\liminf\sigma_n^2/n=0$. That is a genuinely
new result on a 55-year-old open problem and, given the extraordinary nature of the claim, should be treated as
"kernel-verified but awaiting human expert confirmation". I could not identify any formalization defect that
would make it a mere artifact; the construction is coherent with every partial result I know.

## Recommended follow-up

1. Re-run the check with an independent checker (e.g. `lean4checker`) on the exact Mathlib commit used, and
   confirm `#print axioms` for the `.disproof` constant (the comparator already reports only the three standard axioms).
2. Have a probabilist (strong-mixing specialist) review a written version of the argument in this report:
   the key lemmas are the general $\varphi$-mixing criterion `isPhiMixing_of_causalInverse` (`Spec.lean:12356`)
   and the attenuation lemma `exists_attenuating_extension` (`4819`).
3. If confirmed: this is publishable; the `parts.ii` (WIP) statement in the same source file is then also
   false and should be reclassified. Note for the benchmark that "correct" here is *not* a formalization flaw.
4. Cross-check the mechanism against Johansson–Öberg-type $\ell^2$ conditions for chains with complete
   connections (square-summable variation is exactly their uniqueness threshold), which is where I would
   expect any subtle gap in the informal intuition — though the Lean proof does not depend on that literature.
