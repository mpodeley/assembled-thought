# Qué sigue

El salto interesante es este: **pasar de "entender qué hay adentro" a usar el residual stream
como una interfaz de programación.** El paper muestra que — al menos para relaciones factuales —
se puede separar *qué entidad está siendo procesada* de *qué operación se le aplica*,
recombinarlas, y obtener conducta correcta. Si eso se sostiene y se extiende, se abren varias
puertas.

## 1 · Modelos modulares sin reentrenar

En vez de hacer fine-tuning para cada tarea, extraer operadores reutilizables —
`capital-de(x)`, `moneda-de(x)`, `clasificar-hábitat(x)`, quizás algún día `resumir(x)`,
`comparar(x)` — y aplicarlos sobre operandos nuevos durante la inferencia. Algo intermedio entre
prompting, adapters y programación tradicional: **una biblioteca de funciones latentes**.

## 2 · Model editing quirúrgico

Modificar una capacidad o relación sin alterar globalmente el modelo. El hallazgo de que
intervenir *solo el token de consulta* reproduce casi todo el efecto — a 60× menos costo fuera
de tarea — apunta directo acá. Cambiar una relación incorrecta; desactivar una asociación;
instalar temporalmente una política; actualizar una categoría sin fine-tuning completo. Más que
editar pesos, **editás el estado computacional en el momento correcto**.

## 3 · Diagnóstico causal de errores

Ante una respuesta incorrecta, distinguir: ¿*reconoció mal el operando*? ¿*seleccionó mal el
operador*? ¿*falló el binding* entre ambos? ¿o *tenía todo bien y generó mal*? Muy útil para
agentes y modelos locales. En un modelo técnico: operando = *yacimiento X*, operador = *estimar
recuperación* — si el resultado da mal, podrías ver si confundió el activo, la tarea o el
conocimiento.

## 4 · Agentes con control interno más limpio

Un agente podría mantener por separado *objeto actual*, *objetivo actual*, *operación actual* y
*restricciones actuales* — y cambiar exactamente uno: mismo documento + nuevo operador
*criticar*; mismo documento + *resumir*; mismo documento + *buscar inconsistencias*. Menos
dependencia de prompts largos; mejor memoria de trabajo, planificación y cambio de tarea.

## 5 · Transferencia entre idiomas y modalidades

Una posibilidad fuerte: un operador extraído **en inglés** + un operando leído **desde una
imagen** → una respuesta **en español**. (Imagen de Italia + *capital-de* → *Roma*.) Interfaces
latentes entre visión, texto, audio, sensores y modelos especializados — compartiendo operandos
y operadores internos en vez de obligar a todos los módulos a comunicarse por texto.

## 6 · Entrenamiento más composicional

Diseñar objetivos para que el modelo aprenda explícitamente `h(o,k) ≈ a(o) + b(k)`, penalizando
interacciones innecesarias. Posibles beneficios: mejor generalización a combinaciones nunca
vistas, menos datos, más facilidad para reutilizar habilidades, modelos más controlables, menos
interferencia entre tareas — enseñarle al modelo un álgebra interna más ordenada.

## 7 · Compresión y distillation

Si muchas tareas usan un conjunto chico de operadores, destilar *miles de prompts → decenas de
operadores latentes*. En un modelo local, eso podría volverse un **paquete compacto de
capacidades** en vez de una pila de LoRAs. Más especulativo — pero atractivo.

## 8 · Seguridad y control

Detectar qué operación está *intentando* ejecutar el modelo; bloquear operadores peligrosos;
permitir capacidades solo bajo condiciones; separar conocimiento de intención; identificar
cuándo el modelo está en modo *recuperar*, *persuadir*, *ocultar* o *ejecutar*. **El contrapunto
honesto:** una dirección controlable también es manipulable. Interpretabilidad y control tienen
un lado ofensivo inevitable.

## 9 · Ciencia cognitiva artificial

El hilo filosófico: ¿tienen los LLMs equivalentes funcionales de *verbos*, *casos gramaticales*,
*roles semánticos*, *variables*, *funciones*, *memoria de trabajo*, *composicionalidad
simbólica*? No porque "piensen como humanos" — sino porque pueden converger hacia soluciones
estructurales parecidas.

---

## Un ranking realista

Lo más cercano y útil primero:

1. **Debugging causal** de agentes y modelos locales;
2. **model editing temporal y localizado**;
3. **bibliotecas de operadores latentes**;
4. **transferencia multilingüe/multimodal**;
5. **nuevas arquitecturas entrenadas para factorizar**.

El paso de investigación inmediato es mecanístico: usar herramientas de attribution graphs
(Circuit Tracer) para encontrar *qué features, cabezas de atención y MLPs implementan la
recombinación* — hipótesis a validar con la misma disciplina intervencional del paper.

## La gran cautela

Esto funciona notablemente bien para **recuperación relacional** — y **no** apareció para
aritmética ni lógica bajo el pipeline idéntico. No es un "CPU universal latente". Es más parecido
a descubrir que cierta clase de conocimiento factual está organizada como **datos más operaciones
reutilizables**. Y eso ya es bastante grande.
