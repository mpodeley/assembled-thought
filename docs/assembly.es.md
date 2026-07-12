# Ensamblar un pensamiento

Este es el resultado central del paper, en vivo. El pensamiento de un modelo de lenguaje sobre
*"la capital de Italia"* puede **construirse desde tres ingredientes promediados** — una base
genérica, una pieza "Italia" y una pieza "capital-de". Escribí la suma en una sola posición
adentro del modelo mientras lee una pregunta *distinta*, y dice **"Rome"** — tan seguido como
acierta cualquier cosa.

**Cómo manejarlo.** Elegí una **entidad** y una **pregunta**, y apretá **assemble & speak**:
mirá el punto viajar por el mapa de pensamientos del modelo (base → +entidad → +pregunta),
aterrizar al lado del pensamiento real medido, y leé en la consola la salida *real y persistida*
del modelo. Después activá **overdose ×4**: la misma dirección empujada de más se sale del mapa
y la salida colapsa en loops de tokens — pero forzado a elegir entre las respuestas posibles, el
modelo igual acierta el 80% de las veces. Cada salida que ves es una generación real (k=8) de
los experimentos; nada está simulado. *(La interfaz del explorador está en inglés.)*

<iframe src="../../interactive/assembly.html" title="Ensamblar un pensamiento"
        style="width:100%;height:980px;border:1px solid #ddd;border-radius:12px"
        loading="lazy"></iframe>

> Pantalla completa: [abrir la escena directamente](interactive/assembly.html). ¿Preferís
> la vista geométrica (re-marcado, inyección, sincretismo)? Está [acá](geometry.md).
