# agents/ — el equipo de agentes de {{PROJECT_NAME}}

*Aquí vive todo lo que define y alimenta a los agentes de trabajo. El código está fuera; este directorio es la fábrica y su control de calidad.*

## Roles

### 1. El swarm (rol: trabajo)
El algoritmo del propietario que ejecuta tareas del TODO.md.

| Archivo | Qué es |
|---|---|
| `swarm/custom_agent.md` | **SU PROTOCOLO**: pasos, reglas duras, informe obligatorio. Su punto de partida |
| `swarm/NOTA-AGENTE.md` | **Su feedback de evolución** (lo escribe el revisor tras cada entrega) |

### 2. El revisor (rol: verificación)
Agente evaluador independiente que diffea, contrasta con la fuente de verdad y emite la nota 1-10 que sirve de recompensa.

| Archivo | Qué es |
|---|---|
| `revisor/REVISOR.md` | **SU PROTOCOLO Y MEMORIA**: cómo revisar, histórico de notas, barómetro |

### 3. El propietario (rol: orquestación + decisiones finales)
| Archivo | Qué es |
|---|---|
| `owner/boot_swarm.md` | **Secuencia de arranque del swarm** (se inyecta al inicio de cualquier tarea) |

### Informes del swarm (salida auditable)
`reports/informe-<AAAA-MM-DD>-<slug>.md` — uno por entrega, commiteado por el swarm como último commit. Es lo primero que lee el revisor.

## El bucle

```
TODO.md ──> boot (owner/boot_swarm.md) ──> swarm (custom_agent.md + NOTA-AGENTE.md)
   │                                              │
   │                                              └──> agents/reports/informe-*.md + commits
   │                                                        │
   └── propietario pide revisión ───────────────────────────┴──> revisor (REVISOR.md)
                                                                      │
                                                                      ├──> nota 1-10 (recompensa)
                                                                      └──> actualiza NOTA-AGENTE.md + TODO.md + REVISOR.md
```

## Convención

- Los .md de la raíz (TODO.md, AGENTS.md, CONTEXTO.md) son memoria compartida del PROYECTO: los leen ambos roles y los humanos.
- Los nombres de archivo de los roles NO se cambian sin avisar: el swarm puede tener punteros duros a ellos.
- Cambios a los protocolos: propuestos por quien quiera, aplicados y commiteados por el revisor tras aprobación del propietario. El evaluado jamás edita sus propias reglas.
