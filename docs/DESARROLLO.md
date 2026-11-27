# UserManager — Bitácora pedagógica retrospectiva
Documento de consulta del alumnado. No es fuente de verdad técnica. Se amplía únicamente después de realizar y verificar un paso, mediante Build y la skill documentar, con aprobación humana. No contiene pasos futuros ni resultados simulados.

*Todavía no se han registrado pasos ejecutados.*

## 1. Gobierno: revisión y commit de las instrucciones del agente

**Objetivo.** Registrar el paso realmente ejecutado: revisar el estado del repositorio, detectar una edición accidental en la constitución, corregirla y crear un commit Conventional Commits con autorización humana previa.

**Fundamento teórico y pedagógico.** Un commit es una unidad atómica del historial; el formato `tipo(alcance): descripción breve` lo hace legible y trazable (principio P7). Revisar el diff antes de preparar y el index antes de confirmar evita incluir artefactos no deseados. Corregir un texto roto antes de confirmar muestra que verificar no es solo ejecutar comandos, sino interpretar su salida.

**Contexto.** SPEC: no consta (fase 1 de gobierno, aún no aprobada). Rama: `main`. Agente: OpenCode de esta sesión (ID no consta). `/new`: no se usó en este paso. Skills: `conventional-commits` y `documentar`. Artefactos consultados: estado de Git (`branch`, `status`, `diff`, `log`), `AGENTS.md`, `constitution.md`, `.devcontainer/devcontainer.json` y `.devcontainer/devcontainer-lock.json`. `docs/DESARROLLO.md` solo se consultó al final para la numeración.

**Prompts y aclaraciones.** Se usaron «ahora haz un conventional commit» y «carga la skill de documentar para documentar que hemos hecho el paso del agent». El humano resolvió tres dudas —corregir antes de commitear, excluir el lock file y hacer un solo commit— y concedió dos autorizaciones: `git add` selectivo y `git commit`.

**Proceso.** El agente propuso sin ejecutar: rama, archivos, diff y mensaje. Después pidió permiso para el stage selectivo, revisó `git diff --cached`, pidió permiso para el commit y verificó el resultado con `log` y `status`. Toda escritura se anunció con comando, destino y motivo. Propuesta no es decisión: nada se ejecutó hasta la respuesta humana.

**Decisiones humanas y alternativas.** Ante la eliminación accidental de §3 y el texto «ne» en `constitution.md`, se eligió corregir (restaurar §3) frente a commitear tal cual o excluir el archivo; al restaurarlo quedó sin diferencias y no entró en el commit. El lock file se excluyó y quedó sin seguimiento. Se prefirió un solo commit frente a dos atómicos (tooling y documentación separados). No se autorizó el `push`.

**Responsabilidad.** El agente propuso, revisó y ejecutó lo autorizado; el humano decidió y autorizó. Entraron en el commit `.devcontainer/devcontainer.json` (pin de `@opencode/cli@2`) y `AGENTS.md` (instrucciones definitivas, propuesta pendiente de aprobación).

**Comandos y resultados reales.**
```
git branch --show-current                     → main
git status --short                            → M devcontainer.json, M AGENTS.md, M constitution.md, ?? devcontainer-lock.json
git diff --stat                               → 3 archivos, 25 inserciones, 18 eliminaciones
git log --oneline -5                          → 85df809, 45e5ce5
git diff                                      → cambio de postCreateCommand y borrado de §3
(edición: restaurar §3 en constitution.md)
git diff -- constitution.md                   → sin salida
git add .devcontainer/devcontainer.json AGENTS.md → aviso LF→CRLF, sin error
git diff --cached --stat                      → 2 archivos, 24 inserciones, 14 eliminaciones
git diff --cached                             → solo los dos archivos autorizados
git commit -m "docs: finalize AGENTS instructions and pin opencode CLI v2"
                                              → [main c74ee83] 2 files changed, 24 insertions(+), 14 deletions(-)
git log --oneline -3                          → c74ee83, 85df809, 45e5ce5
git status --short                            → ?? .devcontainer/devcontainer-lock.json
```
El identificador `c74ee83` es un SHA de commit público, no una credencial. No se registran credenciales, hashes de claves ni salts.

**Errores y resolución.** `constitution.md` contenía «ne» en lugar de la sección §3, con referencias colgadas en §0 y §5; se restauró el texto original antes del commit. Git avisó de la conversión LF→CRLF en `AGENTS.md` y `constitution.md`: aviso informativo sin efecto en el resultado. No se ejecutaron pruebas: el paso es de gobierno, no de código.

**Conceptos aprendidos.** Área de preparación (index), `git diff --cached`, commit atómico, mensaje Conventional Commits y trazabilidad de decisiones. En 2.º DAW: `git add .` arrastra artefactos no deseados; en su lugar conviene `git add ruta/archivo`, y un mensaje como `docs(governance): approve EARS specification` dice qué cambió y por qué, frente a un `cambios varios` ilegible.

**Punto de control.** Commit `c74ee83` verificado en `main`; árbol de trabajo limpio salvo el lock file sin seguimiento; sin `push`; queda pendiente la aprobación humana de la propuesta `AGENTS.md`. Este registro no sustituye spec, plan, tareas ni verificación.
