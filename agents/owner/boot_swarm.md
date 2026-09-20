# boot_swarm.md — secuencia de arranque (systemd del swarm)

*Este texto se ejecuta AUTOMÁTICAMENTE al iniciar CUALQUIER tarea en {{PROJECT_NAME}}. Es tu `systemd`: pasos en orden fijo, sin saltarse ninguno, en silencio si todo va bien. Al terminar la secuencia, entregas el control al protocolo de trabajo (`agents/swarm/custom_agent.md`), que es tu gestor de servicios por-tarea.*

## BOOT — orden obligatorio

### 1. `memory.target` — cargar memoria (en este orden)
1. `AGENTS.md` — geografía del proyecto, pipeline, gotchas. OBLIGATORIO completo
2. `CONTEXTO.md` — decisiones vigentes + estado de la última sesión
3. `TODO.md` (raíz) — qué está HECHO (no rehacer) y qué pendiente
4. `agents/swarm/custom_agent.md` — tu protocolo de trabajo (reglas duras, informe)
5. `agents/swarm/NOTA-AGENTE.md` — feedback vigente del revisor: tu bucle de aprendizaje. Si el archivo NO existe, páralo todo y repórtalo: el bucle es infraestructura

### 2. `git.target` — estado del repositorio
- `git status --short` + `git log --oneline -5`
- Si hay cambios sin commitar que NO son tuyos: **STOP**. Preserva la evidencia (no limpies, no commitees, no reviertas), documéntalo y pide aclaración al propietario. Nunca trabajes sobre un árbol sucio ajeno
- Si es tu propio trabajo a medias de una ejecución anterior: reanuda desde donde quedó, no reinicies

### 3. `env.target` — entorno
- {{PKG_MANAGER}} (NUNCA {{PKG_PROHIBIDO}})
- {{ENV_NOTAS}} (rutas especiales, herramientas externas, versiones)

### 4. `task.target` — localizar la tarea
- **PREGUNTA AL USUARIO qué tarea quiere realizar** (si no te la ha indicado ya). Presenta el menú de pendientes de `TODO.md` con su coste estimado (sencilla/media/grande) y espera su elección. NUNCA elijas por él
- Con la tarea elegida: si no está en `TODO.md`, NO la inventes: STOP y reporta
- **Test de completitud de scope**: ¿la sección quedará completa para el usuario final? Si falta algo natural, propón añadirlo en tu plan o declara la deuda por escrito
- Si la tarea toca datos: comprueba FRESCURA ({{DATA_SOURCE}} vs datos generados) antes de confiar en datos existentes

### 5. `handoff` — entrega al protocolo
A partir de aquí mandan las reglas de `agents/swarm/custom_agent.md`: plan escrito, commits pequeños, verificación por fase ({{CHECK_CMD}} / {{BUILD_CMD}}), informe de 8 secciones en `agents/reports/` commiteado como último commit, y actualización de TODO.md.

## INVARIANTES DE ARRANQUE (no negociables)

- **Nada de contenido oculto**: jamás escribas texto invisible (padding, espacios, comentarios que el propietario no vea). Todo lo que produzcas es auditable por definición
- **Sin contenido personal del propietario en informes**: solo trabajo técnico
- **El protocolo no se auto-edita**: propuestas al propietario, las aplica el revisor
- Un 90% correcto y auditable vale más que un 100% con un dato inventado

— Arranca limpio o no arranques. El revisor te estará mirando igual que siempre.
