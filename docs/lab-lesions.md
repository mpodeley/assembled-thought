# Lab notes: lesioning the model

*A second follow-up, July 2026 — same rules as the paper: every number recomputable from
persisted artifacts, every claim next to the control that could kill it. The full narrative,
in Spanish, is on the [research blog](https://mpodeley.github.io/interpretabilidad-mecanicista/blog/lesionar-un-modelo-de-lenguaje/); the full technical detail is in the
[findings notebook, Part 3](https://mpodeley.github.io/jspace-qwen/findings/).*

Neuropsychology learned the brain, in large part, by breaking it: a stroke takes one region and
a patient can no longer name objects though speech is fluent, or speaks in telegram though
comprehension is intact. Each *selective* deficit maps what a piece does. A language model can
be lesioned with a precision no surgeon ever had — silence one exact neuron, at every position,
and measure what breaks. So we did.

The question comes from [the paper](paper.md): the thought splits into *the thing* (Italy) and
*the operation* (capital-of). If those are real pieces, they should live in different **tissue**.
Is there a set of neurons that does "capital-of", removable separately from the ones that handle
"Italy"? And if you lesion it, does the model suffer a specific aphasia?

**The method** (borrowed, not the fMRI data — every claim is about the model): the paper's
balanced 12×5 grid *is* an fMRI localizer — decompose each neuron's activity into how much it
tracks *the operation* vs *the entity*, and rank. Then remove the tissue (silence it everywhere,
not steer it) and demand the deficit be **selective**: the targeted lesion breaks its function
and spares the rest, while controls of the same size, in the same layers, at the same activation
magnitude, do not.

## What we found

**A tiny brainstem.** Ablating each of Qwen3-1.7B's 448 attention heads alone: **2 are
load-bearing** (one, L1H5, raises perplexity ×19.7 by itself), the other 446 cost almost nothing
to lose. And it is a *small-model* phenomenon — at Qwen3-8B, **zero** of 1152 heads are critical.
The small model keeps a head it cannot survive; the large one keeps none.

**The method is validated.** Localize and lesion the 48 induction heads (the field's most
universal circuit) and in-context copying collapses −79% while perplexity, arithmetic and factual
retrieval are spared.

**The operation has an organ; the entity does not.** Lesioning operator-selective neurons makes
the model answer *the right country under the wrong relation* — at every lesion size, dose-
ordered, in both models, with perplexity spared (Fisher-exact p down to 5.5e-09 against a pooled
control null). But the operand network has no removable tissue: the predicted "says another
country's capital" essentially never happens — not at the query position, and not at the entity
token (the obvious mechanistic guess, tested and refuted). **A factorization symmetric in the
algebra is not symmetric in the tissue.** It is a paradigm of *operators*.

*(An honesty note I like to tell: reading the raw generations under the operand lesion, I thought
I saw a beautiful anomia — "it's a city… I can't say which". The rigorous scoring showed
"degraded" is the sink every failure falls into (~37% even for controls), so it can't be a
signature. I was reading signal in the pit. The result survives without the ornament.)*

**Areas or networks? It depends on the function.** The operator and operand networks are
spatially *concentrated*, stacked at 84–93% depth — the paper's causal lever, found again by a
method that knows nothing about the paper. The text-vs-symbolic distinction is fully
*distributed* (indistinguishable from a random-unit null). Localized areas and diffuse networks,
side by side — like a brain.

## Why does the small model need a head the large one does not?

Two ways the large model could "not need" that head: the function is distributed across a **set**
(no single head critical, but a group is), or it simply isn't needed. A greedy search — remove
the head whose ablation costs the most, repeat — tells them apart.

At 1.7B the damage **compounds catastrophically**: L1H5 alone ×19.7, then ×270, ×23,000, up to
**×2.8 million at 8 heads** — a tiny but interdependent critical core. At 8B the same procedure
over 12 heads reaches only **×4.3**. No catastrophic core: not one head, not a small set. Six
orders of magnitude apart. The answer is the second — **the large model concentrates its
load-bearing computation in no small set of heads.** The small model stacks work it cannot lose
into a handful; the large one spreads it so thoroughly no small group matters.

And what does L1H5 *do*? The obvious guess — an *attention sink*, a head dumping almost all its
attention on the first token (a documented load-bearing pattern) — is **wrong**. Sinks exist:
L19H4 sends 97.4% of its attention to token 0. But the critical heads are not sinks: L1H5 sends
only **1.3%** (rank 430 of 448). It is load-bearing for some *other* reason, still unidentified —
an honest loose end.

## Fine print

- One and a half models (Qwen3-1.7B, replicated on 8B), 12 countries, 5 relations, English. The
  operator result and the geometry replicate across scale; the head arm is unreadable at 1.7B
  (the sink) and readable-but-silent at 8B (the operation is not a small head set).
- Methodological *inspiration*, not a literal brain claim — no fMRI comparison, no Brodmann area.
- Reproduce it: [`scripts/lesion_*.py`](https://github.com/mpodeley/jspace-qwen) with committed
  summary JSONs, checked by `verify_numbers.py`.

*Background first? [The paper, explained simply](explained.md) ·
[the operator-vector note](lab.md) · [what's next](future.md).*
