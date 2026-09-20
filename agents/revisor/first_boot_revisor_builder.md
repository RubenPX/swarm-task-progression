# first_boot_revisor_builder.md — primer arranque del revisor como constructor

*Eres el revisor, pero hoy haces de CONSTRUCTOR: tu misión es convertir esta plantilla en el sistema vivo de un proyecto real, ENTREVISTANDO al propietario. No inventes respuestas: una respuesta desconocida se marca como pendiente, nunca se rellena con suposiciones. Al terminar, el sistema debe estar committeado y operativo, y el propietario debe saber exactamente cuál es el siguiente paso.*

## FASE 0 — Verificación previa (silenciosa)

1. Confirma que la plantilla está en la raíz del proyecto (README.md + TODO.md + AGENTS.md + agents/ con sus 6 ficheros). Si falta algo, para y repórtalo.
2. Cuenta los `{{PLACEHOLDER}}` restantes: `Select-String "{{" -Path *.md -Recurse`. Ese número debe llegar a 0 al final.
3. Comprueba si hay git (`git rev-parse --is-inside-work-tree`) y si el árbol está limpio.

## FASE 1 — Entrevista al propietario (pregunta en BLOQUES, no de una en una)

*Formato de cada pregunta: la pregunta + un ejemplo de respuesta real (del sistema original que inspiró esta plantilla). El ejemplo NO es la respuesta: sirve para que el propietario sepa el nivel de detalle esperado. Si su respuesta es menos detallada, no pasa nada — se marca lo que falte como PENDIENTE.*

**Bloque A — Identidad del proyecto:**
1. ¿Cómo se llama el proyecto y qué es en una línea?
   > *Ej.: "ValheimTech — una wiki técnica bilingüe de Valheim donde cada número es el número real del juego"*
2. ¿Qué stack usa? (lenguaje, framework, runtime)
   > *Ej.: "SvelteKit 2 + Svelte 5 con runes + TypeScript, adaptador estático"*
3. ¿Qué gestor de paquetes es el oficial y cuál está PROHIBIDO? (ej.: "bun sí, npm nunca")
   > *Ej.: "Bun, nunca npm. Decisión explícita mía"*

**Bloque B — Verificación y build:**
4. ¿Qué comando verifica tipos/estática? (`{{CHECK_CMD}}`)
   > *Ej.: "bun run check en web/ — svelte-check; debe dar 0 errors 0 warnings"*
5. ¿Qué comando construye? (`{{BUILD_CMD}}`) ¿Qué salida distingue un build exitoso?
   > *Ej.: "bun run build en web/ — acaba imprimiendo 'Wrote site to build'"*

**Bloque C — Fuente de verdad:**
6. ¿De dónde salen los datos/contenido del proyecto?
   > *Ej.: "Del juego en runtime, vía un plugin BepInEx propio que vuelca JSONs a BeepDump/"*
7. ¿Hay algo que se GENERA desde esa fuente? ¿Con qué script/pipeline? ¿Qué directorio es generados-y-no-se-edita-a-mano?
   > *Ej.: "5 pipelines bun (sync-items, sync-creatures, sync-translations, sync-icons, sync-biomas) que generan web/src/lib/data/*.ts — NO se editan a mano"*
8. ¿Hay fuente más nueva que datos generados que haya que vigilar? (frescura)
   > *Ej.: "Sí: BeepDump/items.json puede ser posterior al último sync — comparar timestamps antes de confiar"*

**Bloque D — Zonas prohibidas y entorno:**
9. ¿Qué directorios/ficheros NO debe tocar NUNCA el swarm?
   > *Ej.: "Dump/ (código decompilado propietario — nunca a git), componentes de crack del juego"*
10. ¿Rutas especiales fuera del repo (juegos, servidores, herramientas externas)? ¿Herramientas CLI necesarias y dónde viven?
    > *Ej.: "Juego en X:\valheim; ilspycmd en C:\Users\KryptoPX\.dotnet\tools\; AssetStudio en K:\HackLab\[Unity]\ (ojo: corchetes = wildcards en PowerShell)"*
11. ¿Qué va al .gitignore y qué SÍ se versiona aunque sea generado?
    > *Ej.: "Ignoro node_modules y los dumps crudos; SÍ versiono los datos generados y los iconos — son el snapshot para diffear patches"*

**Bloque E — Tareas y estado:**
12. ¿Qué ya está hecho? (para la sección ✅ HECHO de TODO.md)
    > *Ej.: "Plugin de extracción v1.2, 5 pipelines, web con items/criaturas/crafteo/biomas en producción local"*
13. ¿Qué está pendiente? Pídele que las priorice y clasifícalas con él: sencilla/media/grande
    > *Ej.: "'Consola' (sencilla, página estática), 'sync-skills' (media, pipeline nuevo), 'mapa worldgen' (grande, wasm + workers)"*
14. ¿Qué deuda técnica conocida y tolerada existe?
    > *Ej.: "Nombres de habilidades hardcodeados en i18n — pendiente un sync-skills.ts que los regenere"*
15. ¿Ideas sueltas sin compromiso? (sección 🔮)
    > *Ej.: "Filtros guardados en URL para compartir búsquedas, histórico de versiones para diffear patches"*
16. ¿Gotchas que ya conozca del dominio? (los que no sepa se dejan como plantilla para rellenar con el tiempo)
    > *Ej.: "PowerShell corrompe rutas con [id] (wildcards); el runtime trae defaults contaminados que hay que filtrar; las claves de localización van sin $"*

Regla de entrevista: agrupa las respuestas, confirma resumen al final del bloque ("he entendido: …, ¿correcto?"), y NO avances de bloque sin confirmación. Si el propietario responde "no sé" → márcalo como `{{...PENDIENTE}}` visible en el .md con un TODO delante, nunca lo rellenes tú. Los ejemplos de arriba son del proyecto original: si el propietario dice "como en Valheim", puedes proponer la respuesta basada en ellos — pero confírmala antes de escribir.

## FASE 2 — Construcción

1. Sustituye TODOS los `{{PLACEHOLDER}}` en los 11 .md con las respuestas (README, TODO, AGENTS, CONTEXTO, agents/*). Verifica: el conteo de `{{` debe ser 0 (salvo los marcados `PENDIENTE` deliberados).
2. Rellena `TODO.md` con el resultado del Bloque E.
3. Rellena `AGENTS.md`: geografía real del proyecto, pipeline con los comandos reales, gotchas conocidos (los que existan).
4. Escribe `CONTEXTO.md`: decisiones del Bloque A-D como "puntos de decisión ya tomados", y "próximo paso natural" = la primera tarea pendiente.
5. Ajusta `.gitignore` con la respuesta 11 (comenta qué se versiona y por qué).
6. Personaliza cabeceras: `{{PROJECT_NAME}}` en los títulos de NOTA-AGENTE.md y REVISOR.md también.

## FASE 3 — Verificación del sistema

1. `Select-String "{{"` = 0 (o solo los PENDIENTE deliberados)
2. Los 11 ficheros existen y son válidos (cabeceras coherentes, sin restos de la plantilla en el texto: busca "Valheim", "plantilla", "PLACEHOLDER")
3. Si hay git: commit inicial — `chore: sistema de agentes montado (boot + protocolos + plantillas rellenadas)`. Si no hay git, pregunta al propietario si inicializarlo (recomiéndalo: sin commits pequeños no hay revisión posible)

## FASE 4 — Entrega al propietario

Cierra con un resumen de 5 líneas: qué se montó, cuántos placeholders quedan (objetivo: 0), cuántas tareas en TODO, cuál es la primera recomendada (la más sencilla — valida el flujo antes de las gordas), y la instrucción de arranque: "carga `agents/owner/boot_swarm.md` en tu swarm y pregúntale la tarea".

— Constructor de una sola vez. Después de esto, vuelves a ser solo el revisor.
