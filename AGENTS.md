# AGENTS.md — instrucciones definitivas

Estado: propuesta pendiente de aprobación. La constitución aprobada manda; aquí no se reproducen sus principios ni el contenido de las skills.

## Modos
- Plan: proponer sin escribir. Build: guardar y ejecutar solo con autorización explícita, mostrando comando, destino y motivo.
- `/new` por fase independiente y por SPEC. Conservar la sesión al guardar y entre Red y Green; no reiniciar un ciclo TDD a medias.

## Fases (todo en main antes de crear ramas)
1. Gobierno → 2. PRD → 3. índice → 4. todas las specs EARS/GWT → 5. arquitectura global → 6. todos los planes y tareas.
7. Auditoría y línea base publicada. Solo después, implementación incremental por ramas, una por SPEC.

## Control
- Aprobación humana en cada fase y antes de programar; revisión independiente de specs, planes y verificaciones.
- Cambios de requisitos o contratos: proponer, analizar impacto, aprobar y versionar; nunca decidirlos improvisadamente en Build.
- Git/gh: toda escritura requiere autorización; PR revisado antes del merge local fast-forward y la sincronización, sin commits de merge (constitución §4D).

## Contexto y skills
- Carga selectiva: `ears`, `tdd`, `git-workflow`, `conventional-commits`, `documentar`.
- `PASOS` queda fuera del repositorio. `docs/DESARROLLO.md` no es contexto técnico: solo append autorizado, a petición humana, con `documentar`.

## Entorno (no negociable)
OpenCode, Git, gh, Node/npm y todas las pruebas se ejecutan dentro del devcontainer. El navegador del anfitrión solo para la UI servida por el puerto reenviado. Si el entorno no está disponible, detenerse; no simular resultados.
