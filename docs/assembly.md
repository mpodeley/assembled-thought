# Assemble a thought

This is the paper's headline result, live. A language model's thought about *"the capital of
Italy"* can be **built from three averaged ingredients** — a generic base, an "Italy" part, and
a "capital-of" part. Write the sum into a single position inside the model while it reads a
*different* question, and it says **"Rome"** — as often as it says anything correctly at all.

**How to drive it.** Pick an **entity** and a **question**, then press **assemble & speak**:
watch the dot travel the model's map of thoughts (base → +entity → +question), land next to the
real measured thought, and read the model's *actual, persisted* output in the console. Then tick
**overdose ×4**: the same direction pushed too hard leaves the map and the output collapses into
token loops — yet forced to choose among the possible answers, the model still picks the right
one 80% of the time. Every output shown is a real k=8 generation from the experiments; nothing
is simulated.

<iframe src="../interactive/assembly.html" title="Assemble a thought"
        style="width:100%;height:980px;border:1px solid #ddd;border-radius:12px"
        loading="lazy"></iframe>

> Full-page: [open the scene directly](interactive/assembly.html). Prefer the geometry view
> (re-marking, injection, syncretism)? It's [here](geometry.md).
