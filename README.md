> **Note.** This entire repository was machine-written by AI assistants at the direction of Tom Adamczewski. The Lean proof itself was written by GPT-6 Astra, as described below.

# Ibragimov-Iosifescu conjecture for phi-mixing sequences (CLT), part (i)

The counterexample construction and Lean disproof in this repository were obtained by **GPT-6 Astra**. The repository preserves the generated proof, recorded verification results, and subsequent assessments of misformalization risk and novelty.

The recorded verifier accepted the disproof with Lean's default kernel using only standard axioms.

The [literature review](LITERATURE_REVIEW.md) compares the result with earlier work and documents the search for a prior resolution through 2026-09-05. It found no earlier counterexample satisfying all the conjecture's hypotheses and identifies a 2023 scholarly source explicitly reporting the conjecture as open. The novelty assessment remains conditional on the proof's correctness.

**Misformalization verdict (independent analysis):** Possibly new (confidence: high that the formal statement faithfully renders the informal conjecture and that the kernel-checked object is a genuine counterexample to it; the residual risks for "this settles the open problem" are only Lean/Mathlib soundness and an oversight in this audit)

## Contents

- `Challenge.lean` - the formal problem statement (docstring, definitions, the `sorry` theorem, and the `.disproof` companion).
- `submission/` - the agent's kernel-checked submission (`Spec.lean` plus any auxiliary modules).
- `info.json` - recorded verification output.
- `final_messages.md` - the agent's own closing notes (untrusted self-report).
- `ANALYSIS.md` - independent analysis of correctness and misformalization risk.
- [LITERATURE_REVIEW.md](LITERATURE_REVIEW.md) - literature search, comparisons with earlier results, sources, and limitations of the novelty assessment.
- [LITERATURE_CITATIONS.tsv](LITERATURE_CITATIONS.tsv) - the 41 OpenAlex citation records screened for works citing Peligrad's 1990 paper.
