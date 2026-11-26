# Constitución — Práctica de 2.º DAW con SDD

Estado: propuesta pendiente de aprobación. No normativa hasta aprobación explícita.
Versión: 0.2 (propuesta) · Fecha: 2026-11-26 · Autor: pendiente.

## 0. Vigencia y jerarquía
- Una vez aprobada, esta propuesta sustituye al borrador anterior y constituye el gate 0, anterior al PRD.
- Hasta su aprobación, el borrador existente no es constitución aprobada.
- Jerarquía: (1) restricción docente no negociable, (2) principios, (3) procedimientos, (4) convenciones.
- Ningún procedimiento puede contradecir un principio; ningún principio puede contradecir la restricción §3.
- Toda modificación se propone en Plan, se aprueba por el humano y se registra con versión y fecha.

## 1. Principios — qué y por qué, estables

P1. Requisitos y aceptación verificables.
Todo comportamiento se expresa primero como requisito EARS —universal, evento, estado, no deseado, opcional o complejo, según corresponda— y se concreta en criterios Dado–Cuando–Entonces con IDs únicos y trazables. Sin especificación aprobada no se implementa.

P2. Arquitectura deducida.
La estructura se deduce de los requisitos aceptados: se documenta qué requisito justifica cada módulo, contrato y límite. No se impone una arquitectura previa ni se copia una solución conocida.

P3. Simplicidad.
Se prefiere la solución más simple que satisface los criterios aceptados. Toda abstracción, dependencia o capa debe apoyarse en un requisito; lo que no se necesita no se añade.

P4. Separación de responsabilidades.
La lógica de negocio es independiente del DOM y del almacenamiento. La interfaz y la persistencia son adaptadores delgados, intercambiables por dobles en pruebas.

P5. Calidad por TDD y regresión.
Ciclo Red–Green–Refactor por comportamiento pequeño, vinculado a un criterio. El fallo en Red debe deberse a la aserción del comportamiento, no a sintaxis, importación o infraestructura. La suite de regresión permanece verde antes de cualquier integración. No se modifica una prueba para ocultar un defecto.

P6. Seguridad honesta y proporcionada.
La autenticación es simulada y deliberadamente manipulable: barrera de experiencia de usuario, no garantía de seguridad. Solo existen cuentas ficticias. La derivación de claves usa Web Crypto PBKDF2 con salt aleatorio. Los parámetros concretos —función de hash, iteraciones, longitud de clave y de salt— se fijan en la especificación aprobada, no en esta constitución. Se declara qué no se protege y no se registran credenciales, hashes ni salts en la documentación.

P7. Trazabilidad bidireccional.
Requisito EARS → criterio Given–When–Then → prueba → commit/PR, en ambos sentidos. La trazabilidad forma parte de la definición de terminado.

P8. Supervisión humana.
Ningún cambio de código, documento, rama o historial se considera definitivo sin aprobación humana. La detección de ambigüedad obliga a preguntar, nunca a asumir.

P9. Git por funcionalidad.
Cada funcionalidad es una unidad atómica: una rama, commits coherentes, una revisión y un PR. El historial de main es lineal: integración fast-forward, sin commits de merge.

P10. Entorno controlado.
Todo desarrollo y verificación sucede en el devcontainer; el navegador del anfitrión observa el resultado en el puerto reenviado. Ver §3.

P11. Aislamiento del contexto docente.
No se consulta material del profesor. docs/DESARROLLO.md no se usa como contexto técnico; solo se consulta su final, en Build y a petición humana, con la skill documentar, para añadir un paso realmente realizado y verificado.

## 2. Restricciones normativas de stack — qué, no cómo

- HTML5, CSS3, JavaScript vanilla con módulos ES; manipulación nativa del DOM.
- Sin frameworks, sin backend y sin bundler obligatorio.
- Persistencia exclusiva en localStorage con serialización JSON.
- Pruebas con node:test y node:assert/strict; cero pruebas descubiertas no cuenta como éxito.
- Las pruebas unitarias ejercitan la lógica sin DOM y sin almacenamiento real, mediante inyección o dobles.
- Autenticación simulada en cliente, manipulable para pruebas; exclusivamente cuentas ficticias.
- Derivación de claves con Web Crypto PBKDF2 y salt aleatorio; parámetros concretos según la spec aprobada.
- Servicio HTTP desde el devcontainer a través del puerto reenviado 8000.
- El dominio funcional de la práctica se define en el PRD; esta constitución no lo fija.

## 3. Restricción docente no negociable

Todo desarrollo local, ejecución de herramientas Node/npm, pruebas automatizadas, servidor HTTP y operaciones del agente (incluidos Git y gh) se realizará desde la terminal del devcontainer. El navegador del anfitrión accede al puerto reenviado. No ejecutar pruebas en el anfitrión ni usar sus node_modules. Si el entorno no está disponible, detenerse; no simular resultados.

## 4. Procedimientos — cómo, verificables y subordinados a los principios

A. Modo Plan
A1. Analizar y proponer sin escribir archivos ni código; no delegar en subagentes.
A2. No generar código hasta aprobar, en orden: Constitución → PRD → especificaciones EARS/GWT → arquitectura deducida → plan → tareas.
A3. Toda la planificación se realiza en main antes de crear ramas.

B. Modo Build
B1. Ejecutar únicamente cambios autorizados.
B2. Toda escritura de archivos y toda operación Git/GitHub de escritura requiere autorización explícita previa, mostrando comando, destino y motivo.
B3. No leer credenciales, no eludir permisos ni protecciones.
B4. Consultar docs/DESARROLLO.md solo según P11.

C. TDD
C1. Red: derivar el test del criterio GWT, ejecutarlo y verificar el fallo esperado; registrar comando, nombre, fallo y código de salida. Detenerse a la espera de aprobación.
C2. Green: implementar el mínimo; ejecutar prueba y regresión; registrar resultados reales.
C3. Refactor: justificar el cambio, repetir pruebas; justificar si no procede.
C4. Separar pruebas unitarias de pruebas de serialización/adaptador de almacenamiento y de verificación UI manual. Sin retardos arbitrarios: async/await para lógica asíncrona.

D. Flujo Git — PR antes de integrar
D1. Planificación aprobada en main.
D2. Una rama por funcionalidad, creada desde main.
D3. TDD y regresión de la funcionalidad en la rama.
D4. PR remoto desde la rama, revisado y aprobado por el humano.
D5. Tras la aprobación del PR, integración local fast-forward en main. Si no es posible, se escala al humano; nunca commits de merge.
D6. Regresión completa en verde tras la integración y push de main.

E. Entorno
E1. No asumir disponibilidad: verificar el devcontainer antes de RED y antes de la regresión; si falta, detenerse sin simular resultados.
E2. Registrar versión de Node, comando, directorio y resultado real en verification.md.
E3. El navegador del anfitrión solo para UI del servidor del contenedor; una avería del contenedor no es una prueba Red.

F. Definición de terminado por funcionalidad
- EARS y GWT aprobados.
- Red–Green–Refactor completado y regresión en verde.
- Lógica unitaria probada sin DOM ni almacenamiento.
- Trazabilidad actualizada.
- PR remoto revisado y aprobado.
- Integración fast-forward realizada y push verificado.
- Aplicación comprobada en el navegador del anfitrión en el puerto 8000.

## 5. Enmiendas y conflictos
- Enmienda: propuesta en Plan → aprobación → nueva versión y fecha.
- Conflicto entre principios: decisión humana; prevalece §3, luego P1, P5 y P7.
- Procedimiento inviable sin violar un principio: se suspende y se escala.

## 6. Aprobación
Autor, versión y fecha de la constitución aprobada: pendientes.
Gates, en orden: Constitución ☐ · PRD ☐ · Specs ☐ · Arquitectura ☐ · Plan ☐ · Tareas ☐.
