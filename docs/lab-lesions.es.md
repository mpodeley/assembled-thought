# Notas de laboratorio: lesionar el modelo

*Un segundo seguimiento, julio 2026 — mismas reglas que el paper: cada número recomputable desde
artefactos persistidos, cada claim junto al control que podría matarlo. La narrativa completa
está en el [blog de investigación](https://mpodeley.github.io/interpretabilidad-mecanicista/blog/2026/07/17/lesionar-un-modelo-de-lenguaje/); el detalle técnico completo en el
[cuaderno de resultados, Parte 3](https://mpodeley.github.io/jspace-qwen/findings/).*

Los neuropsicólogos entendieron el cerebro, en buena parte, rompiéndolo: un ACV se lleva una
región y el paciente ya no puede nombrar objetos aunque habla fluido, o habla en telegrama aunque
entiende todo. Cada déficit *selectivo* mapea qué hace cada pieza. Un modelo de lenguaje se deja
lesionar con una precisión que ningún cirujano tuvo — apagás una neurona exacta, en todas las
posiciones, y medís qué se rompe. Así que lo hicimos.

La pregunta viene [del paper](paper.md): el pensamiento se separa en *la cosa* (Italia) y *la
operación* (capital-de). Si son piezas de verdad, deberían vivir en **tejido** distinto. ¿Hay un
grupo de neuronas que hace "capital-de", removible por separado de las que manejan "Italia"? Y si
lo lesionás, ¿el modelo sufre una afasia específica?

**El método** (prestado, sin los datos de fMRI — todos los claims son sobre el modelo): el diseño
factorial 12×5 del paper *es* un localizador de fMRI — descomponer la actividad de cada neurona
en cuánto sigue *la operación* versus *la entidad*, y rankear. Después remover el tejido (apagarlo
en todas las posiciones, no inclinarlo) y exigir que el déficit sea **selectivo**: la lesión
dirigida rompe su función y preserva el resto, mientras controles del mismo tamaño, en las mismas
capas, con la misma magnitud de activación, no.

## Lo que encontramos

**Un tronco encefálico diminuto.** Ablando cada una de las 448 cabezas de Qwen3-1.7B por
separado: **2 son intocables** (una, L1H5, multiplica la perplejidad ×19.7 sola), las otras 446
cuestan casi nada. Y es un fenómeno de *modelo chico* — en Qwen3-8B, **cero** de 1152 son
críticas. El chico guarda una cabeza que no puede perder; el grande, ninguna.

**El método está validado.** Localizar y lesionar las 48 induction heads (el circuito más
universal del campo) colapsa el copiado in-context −79% preservando perplejidad, aritmética y
recuperación de hechos.

**La operación tiene órgano; la entidad no.** Lesionar las neuronas selectivas de operador hace
que el modelo responda *sobre el país correcto con la relación equivocada* — en todos los
tamaños, ordenado por dosis, en los dos modelos, con perplejidad preservada (Fisher exact p hasta
5.5e-09 contra el pool de controles). Pero la red-operando no tiene tejido removible: la
predicción "dice la capital de otro país" casi nunca ocurre — ni en la consulta ni en el token de
la entidad (la hipótesis mecánica obvia, testeada y refutada). **Una factorización simétrica en
el álgebra no es simétrica en el tejido.** Es un paradigma de *operadores*.

*(Una nota de honestidad que me gusta contar: leyendo las generaciones crudas bajo la lesión de
operando, creí ver una anomia bellísima — "es una ciudad… no sé cuál". El scoring riguroso mostró
que "degradado" es el sumidero donde cae toda falla (~37% incluso en los controles), así que no
puede ser firma de nada. Estaba leyendo señal en el pozo. El resultado sobrevive sin el adorno.)*

**¿Áreas o redes? Depende de la función.** Las redes de operador y operando están
*concentradas*, apiladas al 84–93% de profundidad — la palanca causal del paper, hallada de nuevo
por un método que no sabe nada del paper. La distinción texto-vs-símbolo está totalmente
*distribuida* (indistinguible de un null de neuronas aleatorias). Áreas localizadas y redes
difusas conviviendo — como un cerebro.

## ¿Por qué el modelo chico necesita una cabeza que el grande no?

Dos maneras en que el grande podría "no necesitarla": la función se distribuyó en un **conjunto**
de cabezas (ninguna sola crítica, pero un grupo sí), o simplemente no la necesita. Una búsqueda
greedy las distingue — quitar la cabeza cuya ablación cuesta más, repetir.

En 1.7B el daño se **compone catastróficamente**: L1H5 sola ×19.7, después ×270, ×23.000, hasta
**×2.8 millones con 8 cabezas** — un núcleo crítico diminuto pero interdependiente. En 8B el mismo
procedimiento con 12 cabezas llega a **×4.3**. No hay núcleo catastrófico: ni una cabeza, ni un
conjunto chico. Seis órdenes de magnitud de diferencia. La respuesta es la segunda: **el grande
no concentra su cómputo crítico en ningún puñado de cabezas.** El chico apila trabajo que no
puede perder en unas pocas; el grande lo desparrama tanto que ningún grupo chico importa.

¿Y qué *hace* L1H5? La hipótesis obvia —que sea un *attention sink*, una cabeza que vuelca casi
toda su atención al primer token— **es falsa**. Sinks hay: L19H4 manda el 97.4% de su atención al
token 0. Pero las críticas no son sinks: L1H5 manda solo el **1.3%** (puesto 430 de 448). Es
intocable por *otra* razón, todavía sin identificar — un cabo suelto, honesto.

## La letra chica

- Un modelo y medio (Qwen3-1.7B, replicado en 8B), 12 países, 5 relaciones, en inglés. El
  resultado del operador y la geometría replican a escala; el brazo de cabezas es ilegible en
  1.7B (el sink) y legible-pero-mudo en 8B (la operación no es un puñado de cabezas).
- *Inspiración* metodológica, no una analogía literal con el cerebro — sin comparación con fMRI,
  sin áreas de Brodmann.
- Reproducilo: [`scripts/lesion_*.py`](https://github.com/mpodeley/jspace-qwen) con los JSONs de
  resumen commiteados, chequeado por `verify_numbers.py`.

*¿Contexto primero? [El paper, explicado fácil](explained.md) ·
[la nota del vector de operador](lab.md) · [qué sigue](future.md).*
