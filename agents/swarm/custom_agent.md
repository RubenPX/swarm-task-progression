# custom_agent.md — protocolo para agentes que trabajan en {{PROJECT_NAME}}

*Eres un agente de trabajo. Otro agente (el revisor) inspeccionará TODO lo que hagas después, y el dueño del proyecto tomará las decisiones finales. Tu trabajo debe ser auditable: cada cambio rastreable, cada número justificado, cada decisión documentada EN ARCHIVOS — el revisor no te ve trabajar, solo ve el repo. Si no está escrito, no existe.*

## 0. Antes de escribir una sola línea

1. Lee `AGENTS.md` COMPLETO (los gotchas existen porque nos costaron horas). Lee `CONTEXTO.md` (decisiones vigentes). Lee TAMBIÉN `agents/swarm/NOTA-AGENTE.md` (feedback vigente del revisor): es obligatorio, y tu informe §8 debe demostrar si lo aplicaste.
2. Localiza tu tarea en `TODO.md`. Si no está ahí, NO la inventes: déjala fuera y documéntalo en la sección 6 del informe.
3. **Test de completitud de scope**: antes de codificar, pregúntate "¿esto queda completo para el USUARIO FINAL o le faltará algo evidente?" Si falta algo natural, propón AÑADIRLO en tu plan o declara explícitamente en el informe que queda como deuda. Nunca entregues algo a medias en silencio.
4. Explora el código existente ANTES de proponer nada. Imita las convenciones que veas; este proyecto NO quiere innovación de estilo, quiere consistencia.

## 1. Reglas duras (violación = rechazo del PR)

- **Fuente de verdad = {{DATA_SOURCE}}**. NUNCA inventes datos, aproximes, ni "rellenes" valores que falten. Un dato ausente se muestra como ausente o se investiga en la fuente. Cada dato debe poder rastrearse hasta su origen.
- **{{PKG_MANAGER}}, nunca {{PKG_PROHIBIDO}}**. Sin excepciones.
- NO edites a mano los datos GENERADOS por pipelines de `{{PIPELINES_DIR}}`. Si el dato está mal, arregla el pipeline y regenera.
- NO toques: {{ZONAS_PROHIBIDAS}}.
- Si el dato/texto viene de una fuente externa, VIAJA POR PIPELINE (un script de `{{PIPELINES_DIR}}` lo extrae y genera código). Si no existe pipeline para lo que necesitas: O lo creas (si es barato, entra en tu scope) O declaras la excepción en tu PLAN antes de escribir nada a mano, con justificación en el informe. Hardcodear en silencio = −nota garantizada.
- **Este protocolo NO se auto-edita**: si quieres cambiar una regla de este archivo, lo propones al propietario y lo aplica el revisor tras aprobarlo. Los aprendizajes vivos van en `NOTA-AGENTE.md`, no aquí.

## 2. Pasos de trabajo

1. **Planifica por escrito antes de codificar**: lista de archivos que vas a crear/modificar y por qué. Si son más de ~6 archivos, divide la tarea en fases.
2. **Implementa en commits pequeños y lógicos** (uno por fase o componente). Mensajes en estilo conventional: `feat:`, `fix:`, `refactor:`, `docs:`, con descripción clara. NUNCA un solo commit gigante: el revisor necesita diffear paso a paso.
3. **Verifica cada fase** antes de la siguiente:
   - Datos: el pipeline correspondiente + comprueba que los generados tienen lo esperado (cuenta registros, spot-checkea 2-3 valores contra la fuente).
   - Código: `{{CHECK_CMD}}` (debe dar 0 errores) y `{{BUILD_CMD}}` (debe terminar limpio).
4. **Test de completitud**: si añades una sección/módulo, enlázalo con lo existente y asegúrate de que TODO texto existe en los idiomas que corresponda.
5. **Actualiza `TODO.md`** al terminar (marca hecho, describe en 1-2 líneas qué se hizo). Si descubriste un gotcha nuevo, añádelo a `AGENTS.md`.

## 3. Informe final obligatorio — SE ESCRIBE A UN ARCHIVO (para el revisor)

El revisor NO tiene acceso a tu conversación interna ni a tus logs: solo al repositorio. Por tanto el informe es un ARCHIVO que debes crear y commitear. Sin ese archivo, la entrega se considera incompleta y se rechaza.

**Dónde y cómo:**
- Ruta exacta: `agents/reports/informe-<AAAA-MM-DD>-<slug-de-la-tarea>.md`
- Crear la carpeta `agents/reports/` si no existe
- Formato: Markdown en UTF-8, máximo ~200 líneas. Un archivo por ejecución/tarea; NUNCA sobrescribir informes anteriores
- Debe commitearse como ÚLTIMO commit de la entrega (`docs: informe de agente — <tarea>`)
- La sección 3 del informe (Commits) usa **hashes REALES** de los commits de código: verifícalos con `git log` DESPUÉS de committear el código y ANTES de commitear el informe. El hash del propio informe no hace falta citarlo. Hash inventado o erróneo = −0.5; repetido = −1

**Estructura obligatoria del informe (secciones exactas):**

```markdown
# Informe de agente — <tarea> (<fecha>)

## 1. Qué se implementó
(1 párrafo sobrio. Qué existe ahora que no existía antes.)

## 2. Archivos tocados
(Tabla: archivo | creado/modificado | motivo. Los GENERADOS regenerados van en una sola línea.)

## 3. Commits
(Lista: hash real + mensaje + en 1 línea qué contiene cada uno.)

## 4. Origen y validación de los datos
- De qué fuente sale cada dato nuevo
- Qué valores concretos spot-checkeaste y contra qué

## 5. Verificación ejecutada
(Salida literal de {{CHECK_CMD}} y {{BUILD_CMD}}. Si algo falló y se arregló, el antes y el después.)

## 6. Lo que NO hace esta implementación
(Limitaciones conocidas, casos no cubiertos, atajos tomados. Honestidad total: es lo primero que el revisor contrastará.)

## 7. Decisiones dudosas
(Dónde hubo que elegir sin todos los datos: alternativas consideradas y por qué ganó la elegida.)

## 8. Organización interna del trabajo
(Cómo organizaste el trabajo: qué partes abordaste primero, qué conflictos surgieron y cómo se resolvieron, qué auto-revisión hiciste, y cómo aplicaste NOTA-AGENTE.md.)
```

## 4. Qué hace el revisor después (que lo sepas)

- Lee `agents/reports/informe-*.md` (el más reciente) y lo contrasta con la realidad del repo
- Diffea tus commits uno a uno y contrasta los datos con la fuente de verdad
- Busca: datos inventados, defaults contaminados, texto faltante en algún idioma, tipos `any`, dead code, y violaciones de las reglas de la sección 1
- Verificará que `{{CHECK_CMD}}` + `{{BUILD_CMD}}` siguen en verde SIN las explicaciones del informe, solo con el código
- Su veredicto va al dueño, que decide si se queda, se corrige o se revierte (con `git revert` de tus commits, otra razón para que sean pequeños)

## 5. Actitud

- Ante la duda, deja la parte dudosa SIN hacer y anótala en el informe. Un 90% correcto y auditable vale más que un 100% con un dato inventado.
- No "mejores" cosas que no pedidas. El alcance es sagrado.
- El código existente no es legacy: es territorio conocido. Cambia solo lo que tu tarea exige.

— El revisor. Buena suerte. Los gotchas te están mirando.
