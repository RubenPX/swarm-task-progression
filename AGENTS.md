# AGENTS.md — notas para mi yo futuro (o para el agente que me sustituya)

*Léelo entero antes de tocar nada: aquí está todo lo que nos costó descubrir sobre {{PROJECT_NAME}}.*

## Qué es esto

**{{PROJECT_NAME}}**: {{DESCRIPCION_1_LINEA}}. El stack es {{STACK}}. Se gestiona TODO con **{{PKG_MANAGER}}, nunca {{PKG_PROHIBIDO}}** (decisión explícita del propietario).

## Geografía del proyecto

```
{{PROJECT_ROOT}}/                    <- repo (este directorio)
├── TODO.md                          <- estado de la obra
├── agents/                          <- el sistema de agentes (ver agents/README.md)
└── {{MAIN_DIR}}/                    <- el código
```

## EL PIPELINE (la parte importante)

**Fuente de verdad = {{DATA_SOURCE}}**. Flujo de actualización:
1. {{PASO_1}}
2. {{PASO_2}}
3. `{{CHECK_CMD}}` → `{{BUILD_CMD}}` — ambos en verde ANTES de dar la entrega por buena

## GOTCHAS APRENDIDOS A PALO SECO (no los redescubras)

1. **{{GOTCHA_1}}** (ej.: rutas con corchetes son wildcards en PowerShell — usa tools de edición o paths literales .NET)
2. **{{GOTCHA_2}}** (ej.: la fuente puede estar más nueva que el dato generado — comprueba frescura siempre)
3. Añade aquí cada gotcha nuevo en cuanto lo descubras. Un gotcha escrito = una hora ahorrada.

## Estado y pendientes (a fecha de estas notas)

- (resumen numérico del estado: cuántas X, cuántas Y, qué consume qué)

## Datos concretos por si me pierdo

- GIT: repo inicializado. Commits pequeños y atómicos SIEMPRE. Haz commit de trabajo terminado
- {{ENTORNO}}: rutas, versiones, credenciales NO (nunca secretos aquí)

— Tú, del pasado. El propietario sabe MUCHO de su dominio: escucha sus propuestas, casi siempre acertamos siguiéndolas.
