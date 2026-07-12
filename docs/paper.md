# The paper

**Operator–Operand Factorization in LLM Residual Streams: Causal Influence and Compositional
Sufficiency** — Matias Podeley, independent researcher. Submitted to **BlackboxNLP 2026**
(co-located with EMNLP, Budapest).

[:material-file-document: Download the PDF](assets/paper.pdf){ .md-button .md-button--primary }
[:material-github: Code & reproducibility](https://github.com/mpodeley/jspace-qwen){ .md-button }
[:material-flask: Evidence & controls](evidence.md){ .md-button }

## Abstract

How do language models represent a relational operation — *currency-of*, *capital-of* — as
distinct from the entity it applies to? We separate three levels of evidence.
**Representation:** a two-way effects decomposition of the query-position state is ~90% additive
in operand and operator across Qwen3-1.7B/8B and Gemma-2-9B, and the steering vector is
*identically* the decomposition's operator main effect. **Causal influence:** adding the
operator difference flips the target-vs-source answer margin for every ordered relation pair in
all three models; the decisive nulls — directions rebuilt under permuted relation labels, and
random directions inside the operator subspace — abolish the effect. **Behavioral sufficiency:**
a per-layer residual trajectory composed from the decomposition's parts, μ + operand + operator,
patched at the query position, makes the model *produce* the target answer at its own accuracy
level; components built without the target cell still generate, and swapping the operand
component redirects the answer. The interaction term adds nothing detectable — and the earlier
failure of additive steering to generate was a dose artifact: an out-of-sample-calibrated dose
generates at the same level, while overdosing inflates the margin as it pushes the state
off-manifold. The structure generalizes (held-out operands, re-worded prompts, a second domain),
separates the operation from its surface word, and does not emerge for arithmetic or logic under
the identical pipeline.

## Read it

<object data="../assets/paper.pdf" type="application/pdf" width="100%" height="780">
  <p>Your browser can't display the PDF inline —
  <a href="../assets/paper.pdf">download it here</a>.</p>
</object>

## Cite it

```bibtex
@misc{podeley2026operator,
  title  = {Operator--Operand Factorization in LLM Residual Streams:
            Causal Influence and Compositional Sufficiency},
  author = {Podeley, Matias},
  year   = {2026},
  url    = {https://mpodeley.github.io/assembled-thought/},
  note   = {Code: https://github.com/mpodeley/jspace-qwen}
}
```

Every number in the paper regenerates from the persisted artifacts in the
[code repository](https://github.com/mpodeley/jspace-qwen) — see its
[reproducibility guide](https://mpodeley.github.io/jspace-qwen/reproduce/). The research log,
with every dead end and control, lives on the
[reproducibility site](https://mpodeley.github.io/jspace-qwen/).
