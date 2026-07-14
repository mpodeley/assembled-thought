# Notas de laboratorio: ¿qué hay adentro del vector de operador?

*Experimentos de seguimiento, julio 2026 — mismas reglas que el paper: cada número
recomputable desde artefactos persistidos, cada claim junto al control que podría matarlo.*

El paper mostró que un pensamiento factual se separa en piezas — la cosa, y la pregunta que se
le hace — y que la **pieza de la pregunta** (el *vector de operador*) se puede extraer
promediando, mover, y volver a escribir adentro. La pregunta siguiente era obvia: **¿qué ES
ese vector?** ¿*Significa* algo — mapea a palabras, estilo rey − hombre + mujer? Le apuntamos
un logit lens (leer cualquier vector interno como "¿qué palabras empuja hacia arriba?"), lo
partimos, e inyectamos las partes por separado.

## 1 · Se lee — y cambia de contenido con la profundidad

Temprano en el modelo (≈40% de profundidad), el vector *capital-de* empuja **palabras del
tema**: "ubicado en", "sede de". En la capa donde tiene palanca causal (≈89%), empuja
**respuestas de ejemplo**: ' Tokyo', ' Paris' — y también ' London' y ' Riyadh', ciudades que
**no son respuestas de nuestro dataset**. El vector codifica la *categoría*, no una tabla de
búsqueda. La misma subida monótona aparece en los tres modelos (score de legibilidad +1.8 →
+8.5 en Qwen3-1.7B, +3.0 → +10.1 en Qwen3-8B, +5.1 → +12.0 en Gemma-2-9B). Una lente de control
hecha de proyecciones aleatorias no lee nada a ninguna profundidad, y los vectores construidos
con etiquetas permutadas se leen como ruido.

## 2 · La pregunta no es su palabra — ni sus respuestas

Dos disociaciones que no esperábamos tan limpias:

- El vector *capital-de* **nunca se alinea con la palabra " capital"** (z ≈ 0 en todas las
  capas), mientras se alinea fuerte con los tokens de ciudades tarde (z ≈ 14–18). *Moneda-de*
  arranca alineado con la palabra " currency" y le pasa la posta a los tokens de respuesta con
  la profundidad.
- Vuelve el par sincrético del paper: *idioma-de* y *gentilicio-de* comparten sus palabras de
  respuesta ("Italian"), y sin embargo **el vector de gentilicio nunca se alinea con esas
  respuestas compartidas** mientras el de idioma sí. Dos preguntas, mismas palabras, geometría
  distinta.

## 3 · La parte legible no es la palanca

El experimento central. Partimos cada vector de operador en la componente exacta que un logit
lens puede ver — su proyección sobre las direcciones de lectura de los tokens de respuesta,
que contiene **3–7% de la energía del vector** — y el resto. Después inyectamos cada parte
sola, a la dosis calibrada del paper, en los tres modelos:

- la **parte legible sola no hace casi nada** (corrimiento de margen +0.9 en Qwen3-1.7B, +0.8
  en Qwen3-8B, +1.1 en Gemma-2-9B; intervalos de confianza pegados a cero; **0% de pares dados
  vuelta**);
- el **resto no-legible carga el efecto en los 20/20 pares de relaciones en los tres modelos**
  (mediana 87% del efecto completo en Qwen3-8B, 93% en Gemma-2-9B; mínimo 79%).

Así que sí — el vector se decodifica a palabras con sentido (contra un claim reciente de que
estas proyecciones son universalmente incoherentes), pero la parte decodificable es causalmente
**inerte**: la identidad de la operación viaja en el ~93% del vector que ningún logit lens
puede leer. *Decodificable, pero no es la palanca.*

## 4 · Una cara nueva del artefacto de dosis

A la sobredosis 4× del paper, inyectar solo la parte legible infla el margen de respuesta
enormemente (+66 en Qwen3-8B, +65 en Gemma-2-9B — el doble del efecto del vector completo). La
trampa: empujar un vector del mismo tamaño construido con las respuestas de las **relaciones
equivocadas** también lo infla (+24 en ambos), igual que vectores aleatorios dentro del span de
tokens de respuesta.
A dosis alta, los márgenes en direcciones alineadas al vocabulario son **mecánicamente
inflables** — la tercera aparición del artefacto de dosis del paper, y una razón más para que
la dosis calibrada sea la lectura honesta.

## 5 · El test más puro: un marcador sin respuestas adentro

Construí la diferencia *vector-idioma − vector-gentilicio* usando **solo** los ocho países
donde las dos preguntas comparten la misma respuesta — así el marcador resultante carga
comprobablemente casi cero información de tokens de respuesta (1–4% de su energía). Inyectalo
en los cuatro países donde las respuestas difieren (Brasil, Egipto, India, México): mueve la
preferencia idioma-vs-gentilicio en la dirección predicha en **24 de 24 celdas** en
Qwen3-1.7B — y de nuevo 24/24 en Qwen3-8B, y de nuevo 24/24 en Gemma-2-9B. **72 de 72 celdas,
tres modelos, dos empresas.** Se puede dirigir *qué pregunta se está haciendo* sin tocar *qué
palabra va a salir*.

## Letra chica honesta

- La generación es heterogénea por relación: para *moneda*, el resto no-legible solo alcanza
  para que el modelo diga la respuesta (94%); para *capital*, hacen falta las dos mitades
  (85% juntas, ~0% cada una sola).
- El aporte de la parte legible a la *generación* es idiosincrático del modelo, no una ley de
  escala: parcheada sola llega a 29.5% de exact match en Qwen3-8B pero solo 8.9% en Qwen3-1.7B
  y 4.9% en Gemma-2-9B — el 8B es el raro, y no tenemos explicación.
- El test del marcador tiene n=4 países. Las 72 celdas entre los tres modelos apuntan igual,
  pero es un n chico y lo decimos.

**Reproducilo:** `scripts/op_vocab_portrait.py` y `scripts/op_vocab_causal.py` en
[jspace-qwen](https://github.com/mpodeley/jspace-qwen), con los JSONs de resumen commiteados;
`scripts/verify_numbers.py` recomputa cada número de arriba. Contexto: esto dialoga con
[Todd et al. (los function vectors decodifican a ejemplares de respuesta)](https://arxiv.org/abs/2310.15213)
y [Nadaf (steerable but not decodable)](https://arxiv.org/abs/2604.02608) — ambos proyecciones
correlacionales; el split de arriba es la versión causal, y el marcador sincrético es la pieza
que ninguno podía construir.

*¿Querés el contexto primero? [El paper, explicado fácil](explained.md) ·
[la escena interactiva](assembly.md) · [qué sigue](future.md).*
