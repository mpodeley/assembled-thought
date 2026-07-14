# Lab notes: what's inside the operator vector?

*Follow-up experiments, July 2026 — same rules as the paper: every number recomputable from
persisted artifacts, every claim next to the control that could kill it.*

The paper showed that a factual thought splits into parts — the thing, and the question being
asked of it — and that the **question part** (the *operator vector*) can be extracted by
averaging, moved around, and written back in. The obvious next question: **what IS that
vector?** Does it *mean* anything — does it map to words, king − man + woman style? We pointed
a logit lens at it (reading any internal vector as "which words does it push up?"), split it,
and injected the parts separately.

## 1 · It reads — and it changes content with depth

Early in the model (≈40% depth), the *capital-of* vector pushes up **topic words**: "located
at", "seat of". At the layer where it has causal leverage (≈89%), it pushes up **example
answers**: ' Tokyo', ' Paris' — and also ' London' and ' Riyadh', cities that are **not answers
in our dataset**. The vector encodes the *category*, not a lookup table. The same monotone rise
appears in all three models (readability score +1.8 → +8.5 in Qwen3-1.7B, +3.0 → +10.1 in
Qwen3-8B, +5.1 → +12.0 in Gemma-2-9B). A control lens built from random projections reads
nothing at any depth, and vectors built with permuted labels read as noise.

## 2 · The question is not its word — and not its answers either

Two dissociations we didn't expect to be this clean:

- The *capital-of* vector **never aligns with the word " capital"** (z ≈ 0 at every layer)
  while aligning strongly with the city tokens late (z ≈ 14–18). *Currency-of* starts aligned
  with the word " currency" and hands over to the answer tokens with depth.
- The paper's syncretic pair returns: *language-of* and *demonym-of* share their answer words
  ("Italian"), yet **the demonym vector never aligns with those shared answers** while the
  language vector does. Two questions, same words, different geometry.

## 3 · The readable part is not the lever

The headline experiment. We split each operator vector into the exact component a logit lens
can see — its projection onto the answer tokens' readout directions, holding **3–7% of the
vector's energy** — and the rest. Then we injected each part alone, at the paper's calibrated
dose, in all three models:

- the **readable part alone does almost nothing** (margin shift +0.9 in Qwen3-1.7B, +0.8 in
  Qwen3-8B, +1.1 in Gemma-2-9B; confidence intervals hugging zero; **0% of pairs flipped**);
- the **non-readable rest carries the effect in 20/20 relation pairs in all three models**
  (median 87% of the full effect in Qwen3-8B, 93% in Gemma-2-9B; minimum 79%).

So yes — the vector decodes to meaningful words (contra a recent claim that such projections
are universally incoherent), but the decodable part is causally **inert**: the operation's
identity travels in the ~93% of the vector that no logit lens can read. *Decodable, but not
the lever.*

## 4 · A new face of the dose artifact

At the paper's 4× overdose, injecting just the readable part inflates the answer margin
enormously (+66 in Qwen3-8B, +65 in Gemma-2-9B — twice the full vector's effect). Trap: pushing
a norm-matched vector built from the **wrong relations' answers** inflates it too (+24 in both),
as do random vectors inside the answer-token span. At high dose, margins in vocabulary-aligned
directions are **mechanically inflatable** — the third appearance of the dose artifact from the
paper, and one more reason calibrated doses are the honest readout.

## 5 · The purest test: a marker with no answers in it

Build the difference *language-vector − demonym-vector* using **only** the eight countries
where both questions share the same answer — so the resulting marker provably carries almost
no answer-token information (1–4% of its energy). Inject it on the four countries where the
answers differ (Brazil, Egypt, India, Mexico): it moves the language-vs-demonym preference in
the predicted direction in **24 of 24 cells** in Qwen3-1.7B — and again 24/24 in Qwen3-8B, and
again 24/24 in Gemma-2-9B. **72 of 72 cells, three models, two companies.** You can steer
*which question is being asked* without touching *which word comes out*.

## Honest print

- Generation is relation-heterogeneous: for *currency*, the non-readable rest alone makes the
  model speak the answer (94%); for *capital*, both halves are needed (85% together, ~0% each
  alone).
- The readable part's contribution to *generation* is model-specific, not a scale law: patching
  it alone reaches 29.5% exact match in Qwen3-8B but only 8.9% in Qwen3-1.7B and 4.9% in
  Gemma-2-9B — the 8B is the odd one out, and we have no account of why.
- The marker test has n=4 countries. All 72 cells across three models point the same way, but
  it is a small n and we say so.

**Reproduce it:** `scripts/op_vocab_portrait.py` and `scripts/op_vocab_causal.py` in
[jspace-qwen](https://github.com/mpodeley/jspace-qwen), with committed summary JSONs;
`scripts/verify_numbers.py` recomputes every number above. Context: this speaks to
[Todd et al. (function vectors decode to answer exemplars)](https://arxiv.org/abs/2310.15213)
and [Nadaf (steerable but not decodable)](https://arxiv.org/abs/2604.02608) — both
correlational projections; the split above is the causal version, and the syncretic marker is
the piece neither could build.

*Want the background first? [The paper, explained simply](explained.md) ·
[the interactive scene](assembly.md) · [what's next](future.md).*
