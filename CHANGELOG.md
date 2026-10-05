# Historial de cambios

Todas las versiones del Estándar de Proyecto Nexoru. Formato basado en [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/); versionado según [semver](https://semver.org/lang/es/) (ver `CLAUDE.md`).

## [1.3.0] - 2026-10-04

Versión MINOR: ningún proyecto conforme con 1.2 deja de serlo. Los proyectos pueden seguir declarando `version_estandar: "1.2"` hasta actualizarse.

### Agregado
- `standard/project-standard.md`: sección opcional `## Incrementos planeados` en `PROJECT.md`, entre `## Roadmap` y `## Decisiones clave`, con la tabla `Incremento | Objetivo | Prioridad | Referencia`. No forma parte del roadmap, no cuenta para el avance y no genera hallazgos de cierre.
- `standard/roadmap.md`: sección "Apertura de un incremento" (la fila sale de `## Incrementos planeados` y entra al roadmap como la siguiente fase, con su spec y fecha objetivo) y ejemplo del Proyecto Demo con un incremento planeado.
- `standard/lifecycle.md`: apertura y cierre de incrementos. Al abrir uno, `fase` pasa a `especificacion` o `construccion` con `fase_desde` nuevo y se actualiza `docs/mapa-funcional.md`; al concluir, el proyecto vuelve a `operacion` con `fase_desde` nuevo.
- `standard/conformance.md`, verificación 1.8: formato de `## Incrementos planeados` cuando existe. Las filas de esa sección no cuentan como fases en los hallazgos de cierre.
- `templates/PROJECT.md`: sección `## Incrementos planeados` con un ejemplo del Proyecto Demo.

### Cambiado
- `standard/lifecycle.md`: "Cierre y reactivación" pasa a "Cierre y apertura de incrementos".
- `standard/project-manifest.md`: se aclara que `fecha_objetivo` es opcional en `operacion` porque cada incremento lleva su fecha en el roadmap (ya era opcional desde 1.0).
- `version_estandar` de las plantillas a `"1.3"`.

## [1.2.0] - 2026-10-03

Versión MINOR: ningún proyecto conforme con 1.1 deja de serlo. Los proyectos pueden seguir declarando `version_estandar: "1.1"` hasta actualizarse.

### Agregado
- `standard/project-manifest.md`: campo opcional `visibilidad` (`publico` | `privado`), con el que el Dueño declara la visibilidad decidida del repo; la razón va en `## Decisiones clave`.
- `standard/repo-visibility.md`: sección "Visibilidad declarada", que compara el campo con la visibilidad real del repo en GitHub.
- `standard/conformance.md`: hallazgos altos "requiere decisión del Dueño" (`producto-nexoru` o `producto-cliente` sin `visibilidad`) y "discrepancia de visibilidad" (declarada distinta de la real). Si coincide, se reporta como aceptada. Ninguno cambia el nivel. La salida del programa incluye el resultado de visibilidad.
- `templates/PROJECT.md`: `visibilidad: CONFIRMAR` y una fila de ejemplo en `## Decisiones clave` con el Proyecto Demo.
- `migration/guide.md`: la visibilidad y su razón entran en las preguntas para el Dueño.

### Cambiado
- `standard/conformance.md`, verificación 3.2: se cumple cuando la ejecución terminada más reciente de cada workflow que satisface 3.1 terminó con éxito en la rama principal.
- `standard/conformance.md`: el hallazgo alto de visibilidad genérico queda como "`producto-cliente` en un repo público", aunque esté declarado.
- `version_estandar` de las plantillas a `"1.2"`.

## [1.1.0] - 2026-10-01

Versión MINOR: ningún proyecto conforme con 1.0 deja de serlo. Los proyectos pueden seguir declarando `version_estandar: "1.0"` hasta actualizarse.

### Agregado
- `standard/project-standard.md`: correspondencia entre filas de `## Costo mensual` y elementos de `servicios`. Una fila corresponde a un servicio si el identificador aparece en su nombre normalizado (minúsculas, cada tramo de espacios a un guion). Incluye un ejemplo con el Proyecto Demo.
- `standard/project-standard.md`: `.nexoruignore`, un archivo opcional en `PROJECTS_ROOT` con las carpetas que no son proyectos (una por línea). Los evaluadores las omiten.
- `standard/roadmap.md`: definiciones de fase concluida (100% de tareas derivadas, o `Estado manual` = `completa`) y de roadmap concluido (todas sus fases concluidas).
- `standard/lifecycle.md`: fase `retirado`, con criterios de entrada, y sección "Cierre y reactivación". Con el roadmap concluido, el proyecto va a `operacion` si está desplegado o a `retirado` si no. Un incremento (fase nueva en el roadmap) regresa el proyecto a `especificacion` o `construccion`, con `fase_desde` nuevo, y obliga a actualizar `docs/mapa-funcional.md`.
- `standard/project-manifest.md`: `retirado` como valor permitido de `fase`; `fecha_objetivo` no es obligatoria en `retirado`.
- `standard/conformance.md`: dos hallazgos que no cambian el nivel: `operacion` con fases pendientes en el roadmap, y `construccion` o `especificacion` con el roadmap concluido. Alcance del evaluador del portafolio según `.nexoruignore`.

### Cambiado
- `version_estandar` de las plantillas a `"1.1"`.
- `standard/lifecycle.md`: la entrada a `operacion` pide además el roadmap concluido y el sistema desplegado; de `operacion` también se sale a `retirado`.

## [1.0.0] - 2026-09-28

Primera versión.

### Agregado
- `standard/project-standard.md`: artefactos obligatorios de un proyecto y reglas centrales.
- `standard/project-manifest.md`: esquema del frontmatter de `PROJECT.md`.
- `standard/roadmap.md`: formato de la tabla de roadmap y cómo se deriva el estado desde Spec Kit.
- `standard/lifecycle.md`: ciclo de vida de un producto Nexoru, con criterios de entrada y salida.
- `standard/conformance.md`: niveles de conformidad 0 a 3 y verificaciones para un programa.
- `standard/repo-visibility.md`: criterio para repos públicos o privados.
- `templates/`: plantillas de `PROJECT.md`, `docs/mapa-funcional.md` y la sección de `CLAUDE.md`.
- `migration/guide.md`: guía y prompt para migrar un proyecto existente.
