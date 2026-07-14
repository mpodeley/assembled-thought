---
hide:
  - navigation
  - toc
---

<div class="hero" markdown>

# The thought is made of parts

<p class="hero-sub">We took a language model's thought — <em>"the capital of Italy"</em> — apart
into three averaged ingredients, put them back together, wrote the assembled state into a single
spot inside the model… and it said <b>"Rome"</b>. Swap the Italy part for France and it says
<b>"Paris"</b>. Push the same knob 4× too hard and it babbles — yet still <em>knows</em> the
answer.</p>

<div class="hero-buttons" markdown>
[:material-play-circle: Watch it assemble](assembly.md){ .md-button .md-button--primary }
[:material-school: Explained simply](explained.md){ .md-button }
[:material-file-document: The paper (PDF)](assets/paper.pdf){ .md-button }
[:material-flask: Evidence & controls](evidence.md){ .md-button }
[:material-github: Code](https://github.com/mpodeley/jspace-qwen){ .md-button }
</div>

<div class="hero-gif" markdown>
[![A thought assembled from three averaged parts flies across the model's map of thoughts, lands beside the real measured state — and the console types the model's actual output: "Rome, and the currency of the United"](figs/assembly.gif)](assembly.md)
</div>

## The numbers

<div class="stats" markdown>
<div class="stat"><span class="n accent-blue">52% ≈ 53%</span><span class="d">a thought assembled
from averaged parts makes the model say the answer at its own accuracy ceiling (8B: 62% vs
68%)</span></div>
<div class="stat"><span class="n accent-orange">20/20 × 3</span><span class="d">every ordered
relation swap flips the answer margin in Qwen3-1.7B, Qwen3-8B and Gemma-2-9B; permuted-label
nulls ≈ 0</span></div>
<div class="stat"><span class="n accent-gold">80%</span><span class="d">even at a 4× overdose —
where fluent speech collapses into token loops — the right answer still wins a forced
choice</span></div>
<div class="stat"><span class="n accent-green">3 × 2</span><span class="d">three models, two
domains (geography, animal taxonomy) where it works — and two (arithmetic, logic) where it
provably doesn't</span></div>
</div>

!!! note "The honest null that started this"
    This project began as a replication of a "readable subspace" claim — which did **not**
    replicate under matched controls. The structure reported here is *causal* organization,
    validated against nulls that preserve every statistic of the method while destroying its
    meaning. Every claim sits next to the control that could have killed it in
    [Evidence & controls](evidence.md).

---

**Paper:** *Operator–Operand Factorization in LLM Residual Streams: Causal Influence and
Compositional Sufficiency* — [PDF](assets/paper.pdf) · [abstract & BibTeX](paper.md) ·
[code & reproducibility](https://github.com/mpodeley/jspace-qwen) ·
[what's next](future.md)

Matias Podeley · independent researcher · <mpodeley@gmail.com> · MIT license ·
learn the field in Spanish: [Interpretabilidad Mecanicista](https://mpodeley.github.io/interpretabilidad-mecanicista/)
