# La geometría detrás

Una vista interactiva, estilo 3Blue1Brown, del resultado operador/operando en Qwen3-1.7B —
leída en vivo del modelo, sin re-ejecutar nada. Tres escenas: el morph de **re-marcado**
(organizado-por-operando → organizado-por-operador a lo largo de la secuencia), la **inyección**
de una dirección de operador (mirá el margen de respuesta darse vuelta), y el **sincretismo** de
`language`/`demonym` con el marcador sin exponente. *(La interfaz está en inglés.)*

**Cómo manejarlo.** *Escena 1:* arrastrá el slider de "country token" a "query token" y mirá los
mismos 60 puntos reorganizarse — mezclados por país a la izquierda, cinco clusters limpios de
pregunta a la derecha; ▶ lo anima. *Escena 2 (Injection):* elegí un par from→to, apretá
**inject**, y mirá el estado deslizarse al cluster objetivo mientras el margen de respuesta se
da vuelta; después activá **random control** y mirá cómo una flecha aleatoria igual de grande no
hace nada. *Escena 3 (Syncretism):* dos preguntas que comparten su palabra de respuesta viven
separadas como direcciones; disparo del **marcador puro** y la relación se instala igual, con la
palabra cancelada.

¿Buscás el resultado central — ensamblar un pensamiento desde partes y escuchar al modelo
decirlo? Tiene [su propia escena](assembly.md).

<iframe src="../../interactive/declension.html" title="Explorador geométrico"
        style="width:100%;height:1180px;border:1px solid #ddd;border-radius:12px"
        loading="lazy"></iframe>

> Página completa: [abrir el explorador directo](interactive/declension.html). Enlaces
> directos: [re-marcado](interactive/declension.html#0) ·
> [inyección](interactive/declension.html#1) ·
> [sincretismo](interactive/declension.html#2).
