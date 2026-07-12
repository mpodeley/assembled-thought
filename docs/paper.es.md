# El paper

**Operator–Operand Factorization in LLM Residual Streams: Causal Influence and Compositional
Sufficiency** — Matias Podeley, investigador independiente. Enviado a **BlackboxNLP 2026**
(co-ubicado con EMNLP, Budapest). El paper está en inglés.

[:material-file-document: Descargar el PDF](assets/paper.pdf){ .md-button .md-button--primary }
[:material-github: Código y reproducibilidad](https://github.com/mpodeley/jspace-qwen){ .md-button }
[:material-flask: Evidencia y controles](evidence.md){ .md-button }

## Resumen (traducido)

¿Cómo representa un modelo de lenguaje una operación relacional — *moneda-de*, *capital-de* —
como algo distinto de la entidad a la que se aplica? Separamos tres niveles de evidencia.
**Representación:** una descomposición de efectos de dos vías del estado en la posición de
consulta es ~90% aditiva en operando y operador en Qwen3-1.7B/8B y Gemma-2-9B, y el vector de
steering es *idénticamente* el efecto principal del operador. **Influencia causal:** sumar la
diferencia de operadores da vuelta el margen objetivo-vs-fuente para cada par ordenado de
relaciones en los tres modelos; los nulos decisivos — direcciones reconstruidas con etiquetas de
relación permutadas, y direcciones aleatorias dentro del subespacio de operadores — eliminan el
efecto. **Suficiencia conductual:** una trayectoria residual por-capa *compuesta* desde las
partes de la descomposición, μ + operando + operador, parcheada en la posición de consulta, hace
que el modelo *produzca* la respuesta objetivo a su propio nivel de precisión; componentes
construidos sin la celda objetivo también generan, y cambiar el componente de operando redirige
la respuesta. El término de interacción no agrega nada detectable — y la falla anterior del
steering aditivo para generar era un artefacto de dosis: una dosis calibrada fuera de muestra
genera al mismo nivel, mientras que la sobredosis infla el margen a la vez que empuja el estado
fuera del manifold. La estructura generaliza (operandos held-out, prompts re-redactados, un
segundo dominio), separa la operación de su palabra superficial, y no emerge para aritmética ni
lógica bajo el pipeline idéntico.

## Leerlo

<object data="../../assets/paper.pdf" type="application/pdf" width="100%" height="780">
  <p>Tu navegador no puede mostrar el PDF —
  <a href="../../assets/paper.pdf">descargalo acá</a>.</p>
</object>

## Citarlo

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

Cada número del paper se regenera desde los artefactos persistidos del
[repo de código](https://github.com/mpodeley/jspace-qwen) — ver su
[guía de reproducibilidad](https://mpodeley.github.io/jspace-qwen/reproduce/). La bitácora de
investigación, con cada callejón sin salida y cada control, vive en el
[sitio de reproducibilidad](https://mpodeley.github.io/jspace-qwen/).
