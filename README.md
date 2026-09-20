# TaskProgressionSwarm — plantilla de trabajo con agentes

*Plantilla extraída de un sistema real que evolucionó de nota 8 a 10 en 6 entregas. La idea: un algoritmo swarm hace las tareas, un agente revisor independiente las audita contra la realidad, y la nota que emite alimenta el aprendizaje del swarm. Cada rol tiene su carpeta, cada documento su dueño.*

## Qué incluye

```
├── README.md                      <- este archivo (instrucciones de montaje)
├── TODO.md                        <- estado del proyecto: hecho / pendiente / ideas
├── AGENTS.md                      <- memoria del proyecto (gotchas, geografía, pipeline)
├── CONTEXTO.md                    <- decisiones finas que no caben en AGENTS.md
├── .gitignore
└── agents/
    ├── README.md                  <- mapa de roles + diagrama del bucle
    ├── owner/boot_swarm.md        <- arranque automático del swarm (systemd-style)
    ├── swarm/custom_agent.md      <- protocolo de trabajo del swarm
    ├── swarm/NOTA-AGENTE.md       <- feedback de evolución (lo escribe el revisor)
    ├── revisor/REVISOR.md         <- protocolo y memoria del evaluador
    └── reports/                   <- informes de entrega del swarm (uno por tarea)
```

## Montaje en un proyecto nuevo (10 minutos)

**Opción recomendada — asistida:** carga `agents/revisor/first_boot_revisor_builder.md` en tu agente revisor. Te entrevistará por bloques (identidad, stack, fuente de datos, zonas prohibidas, tareas), rellenará la plantilla con tus respuestas, verificará que quedan 0 placeholders y commiteará. Tú solo respondes preguntas.

**Opción manual:**

1. **Copia el contenido de esta carpeta** a la raíz de tu proyecto (no como subcarpeta: TODO.md y AGENTS.md deben estar en la raíz).
2. **Sustituye los `{{PLACEHOLDER}}`** en todos los .md — búscalos con `Select-String "{{" -Path *.md -Recurse`. Los principales:
   - `{{PROJECT_NAME}}` — nombre del proyecto
   - `{{PKG_MANAGER}}` — gestor de paquetes (bun/npm/pnpm/cargo/pip…)
   - `{{CHECK_CMD}}` — comando de verificación estática (tsc, svelte-check, clippy…)
   - `{{BUILD_CMD}}` — comando de build
   - `{{DATA_SOURCE}}` — tu fuente de verdad de datos (API, BD, dumps, ficheros…)
   - `{{REPORT_PATH}}` — dónde viven los informes (`agents/reports/` por defecto)
3. **Rellena `TODO.md`** con tus tareas reales (hecho/pendiente). El swarm SOLO trabaja tareas que estén ahí.
4. **Rellena `AGENTS.md`** con lo que ya sabes del proyecto — cada gotcha que escribas es una hora que tu swarm no perderá redescubriendo.
5. **Inicializa git y haz el primer commit.** El sistema necesita commits pequeños: la revisión y el revert dependen de ello.
6. **Ejecuta el boot**: carga `agents/owner/boot_swarm.md` en tu swarm al iniciar cualquier tarea. El swarm preguntará qué tarea se quiere, trabajará según el protocolo, y entregará informe en `agents/reports/`.
7. **Cuando termine una tarea, pide la revisión** a tu agente revisor cargando `agents/revisor/REVISOR.md`. La nota 1-10 es tu recompensa para el aprendizaje del swarm.

## Los 5 principios que hacen funcionar el sistema

1. **Fuente de verdad explícita**: todo dato debe poder rastrearse a su origen. Nada inventado, nada "aproximado". Un 90% auditable vale más que un 100% con un dato falso.
2. **Texto/datos viajan por pipeline**: si el dato viene de una fuente externa, existe un script que lo extrae y genera código. Los datos a mano mueren con el primer patch.
3. **Informe obligatorio a archivo**: el swarm escribe un informe de 8 secciones y lo commitea. Sin informe, la entrega está incompleta. Es lo que hace la revisión posible.
4. **Separación de capas**: aprendizaje (NOTA-AGENTE, cambia cada entrega) ≠ contrato (custom_agent.md, solo cambia por propuesta aprobada) ≠ memoria del proyecto (AGENTS.md). El evaluado jamás reescribe sus propias reglas.
5. **Commits pequeños y atómicos**: la revisión diffea paso a paso y los errores se revierten con precisión quirúrgica.

## Lecciones aprendidas en el sistema original (léelas antes de modificar nada)

- El crash más tonto (un `m[3]` de un regex de 2 grupos) lo detectó el swarm, no el humano ni el revisor: la doble verificación funciona en ambas direcciones.
- Un dato "stale" (fuente más nueva que el dato generado) es un bug silencioso: la frescura se comprueba siempre.
- El feedback de evolución (NOTA-AGENTE) es infraestructura: si se borra, el bucle muere. Restaurarlo es lo primero.
- Los tests del propietario (texto oculto, commits anómalos) son legítimos; el protocolo del revisor los trata igual que una amenaza real hasta que el propietario confirma.
