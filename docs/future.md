# What's next

The interesting leap is this: **from "understanding what's inside" to using the residual stream
as a programming interface.** The paper shows that — at least for factual relations — you can
separate *which entity is being processed* from *which operation is applied to it*, recombine
them, and get correct behavior. If that holds up and extends, several doors open.

## 1 · Modular models without retraining

Instead of fine-tuning for each task, extract reusable operators — `capital-of(x)`,
`currency-of(x)`, `classify-habitat(x)`, perhaps one day `summarize(x)`, `compare(x)` — and
apply them to new operands at inference time. Something between prompting, adapters, and
traditional programming: **a library of latent functions**.

## 2 · Surgical model editing

Modify one capability or relation without touching the model globally. The paper's finding that
intervening on the *query token alone* reproduces nearly the full effect — at 60× lower off-task
cost — points straight at this. Fix a wrong relation; disable an association; install a
temporary policy; update a category without a full fine-tune. Rather than editing weights, you
**edit the computational state at the right moment**.

## 3 · Causal error diagnosis

When a model answers wrongly, distinguish: did it *misrecognize the operand*? *select the wrong
operator*? *fail the binding* between them? or *have everything right and generate badly*?
Immensely useful for agents and local models. In a technical model: operand = *oil field X*,
operator = *estimate recovery* — if the result is wrong, you could see whether it confused the
asset, the task, or the knowledge.

## 4 · Agents with cleaner internal control

An agent could hold *current object*, *current goal*, *current operation*, and *current
constraints* separately — and change exactly one: same document + new operator *critique*; same
document + *summarize*; same document + *find inconsistencies*. Less dependence on long prompts;
better working memory, planning, and task switching.

## 5 · Transfer across languages and modalities

A strong possibility: an operator extracted **in English** + an operand read **from an image** →
an answer **in Spanish**. (Picture of Italy + *capital-of* → *Roma*.) Latent interfaces between
vision, text, audio, sensors, and specialist models — sharing internal operands and operators
instead of forcing every module to talk through text.

## 6 · More compositional training

Design objectives so the model explicitly learns `h(o,k) ≈ a(o) + b(k)`, penalizing unnecessary
interaction. Possible payoffs: better generalization to never-seen combinations, less data,
easier skill reuse, more controllable models, less task interference — teaching the model a
tidier internal algebra.

## 7 · Compression and distillation

If many tasks share a small set of operators, distill *thousands of prompts → dozens of latent
operators*. On a local model, that could become a compact **capability pack** instead of a stack
of LoRAs. More speculative — but attractive.

## 8 · Safety and control

Detect which operation the model is *trying* to execute; gate dangerous operators; allow
capabilities only under conditions; separate knowledge from intention; recognize when the model
is in *retrieve*, *persuade*, *conceal*, or *execute* mode. **The honest counterpoint:** a
controllable direction is also a manipulable one. Interpretability and control have an
unavoidable offensive side.

## 9 · Artificial cognitive science

The philosophical thread: do LLMs have functional equivalents of *verbs*, *grammatical cases*,
*semantic roles*, *variables*, *functions*, *working memory*, *symbolic compositionality*? Not
because they "think like humans" — but because they may converge on similar structural
solutions.

---

## A realistic ranking

Closest and most useful first:

1. **Causal debugging** of agents and local models;
2. **temporary, localized model editing**;
3. **libraries of latent operators**;
4. **multilingual / multimodal transfer**;
5. **new architectures trained to factorize**.

The immediate research step is mechanistic: use attribution-graph tooling (Circuit Tracer) to
find *which features, attention heads, and MLPs implement the recombination* — hypotheses to be
validated with the same interventional discipline as the paper.

## The big caveat

This works remarkably well for **relational retrieval** — and did **not** appear for arithmetic
or logic under the identical pipeline. It is not a "universal latent CPU". It is more like
discovering that a certain class of factual knowledge is organized as **data plus reusable
operations**. And that is already quite a lot.
