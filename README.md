> **Note.** This entire repository was machine-written by AI assistants at the direction of Tom Adamczewski. The Lean proof itself was written by GPT-6 Astra, as described below.

# Ibragimov–Iosifescu conjecture for φ-mixing sequences

The counterexample construction and Lean disproof in this repository were obtained by **GPT-6 Astra**. The repository preserves the generated proof, recorded verification results, and subsequent assessments of misformalization risk and novelty.

For background and the conjecture's statement, see [the Wikipedia article on the Ibragimov–Iosifescu conjecture for φ-mixing sequences](https://en.wikipedia.org/wiki/Ibragimov%E2%80%93Iosifescu_conjecture_for_%CF%86-mixing_sequences).

The Lean disproof addresses Ibragimov's central limit theorem claim. If correct, it also rules out Iosifescu's stronger claim that the rescaled partial-sum process converges to Brownian motion: that convergence would imply the central limit theorem at time 1.

The recorded verifier accepted the disproof with Lean's default kernel using only standard axioms.

The [literature review](LITERATURE_REVIEW.md) compares the result with earlier work and documents the search for a prior resolution through 2026-09-05. It found no earlier counterexample satisfying all the conjecture's hypotheses and identifies a 2023 scholarly source explicitly reporting the conjecture as open.

## Summary and roadmap

The [full natural-language argument](natural-language-argument.md) constructs a strictly stationary, centered, φ-mixing sequence with finite second moments whose partial-sum variance diverges, yet whose variance-normalized sums converge to zero in probability along a subsequence. Bounded feedback makes typical sums small, while increasingly rare, large spikes retain the variance and prevent a central limit theorem.

The argument proceeds in four steps:

- **Feedback and attenuation (§§2–3):** Build bounded causal feedback controllers and show that a finite cascade can suppress ordinary fluctuations at arbitrarily small amplitude and variation cost.
- **Uniform forgetting (§4):** Combine square-summable variation of the inverse rule with Gaussian smoothing to prove φ-mixing and compare stationary block laws.
- **Rare spikes (§5):** Prescribe the spikes’ second-moment contributions and prove a variance lower bound that bounded feedback cannot erase.
- **Diagonal construction and conclusion (§§6–7):** Choose each observation time before introducing the next spike, preserve earlier comparisons, and prove both variance divergence at all large times and collapse of the normalized sums along the selected subsequence.

## Build and verify

The project pins the environment of the recorded run: **Lean 4.27.0**, [Formal Conjectures](https://github.com/google-deepmind/formal-conjectures) commit `9cbe1d3c12998c786b7c2cd99ce28a21b6631f66`, Mathlib commit `a3a10db0e9d66acbebf76c5e6a135066525ac900`, and all transitive dependencies in [lake-manifest.json](lake-manifest.json). With [elan](https://github.com/leanprover/elan) installed:

```sh
lake exe cache get
lake build
```

This builds `Challenge.lean` (two expected `sorry` warnings for the conjecture statement and its `.disproof` stub) and `submission/Spec.lean` (one expected `sorry` warning for the conjecture statement, which the disproof does not use). To confirm the axiom dependencies of the disproof, run:

```sh
lake env lean --stdin <<'EOF'
import Spec
#print axioms IbragimovIosifescuConjectureForMixingSequences.ibragimov_iosifescu_conjecture_for_mixing_sequences.parts.i.disproof
EOF
```

The expected output lists only `propext`, `Classical.choice`, and `Quot.sound`.

## Contents

- `Challenge.lean` - the formal problem statement (docstring, definitions, the `sorry` theorem, and the `.disproof` companion).
- `submission/` - the agent's kernel-checked submission (`Spec.lean` plus any auxiliary modules).
- `info.json` - recorded verification output.
- `lean-toolchain`, `lakefile.toml`, `lake-manifest.json` - pinned build environment (see above).
- `final_messages.md` - the agent's own closing notes (untrusted self-report).
- `ANALYSIS.md` - independent analysis of correctness and misformalization risk.
- [LITERATURE_REVIEW.md](LITERATURE_REVIEW.md) - literature search, comparisons with earlier results, sources, and limitations of the novelty assessment.
- [LITERATURE_CITATIONS.tsv](LITERATURE_CITATIONS.tsv) - the 41 OpenAlex citation records screened for works citing Peligrad's 1990 paper.
