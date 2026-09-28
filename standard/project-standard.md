# Estándar de Proyecto Nexoru

Versión 1.0. Define qué debe tener todo proyecto que vive en `/proyectos/<nombre>` y cómo se relacionan sus documentos.

## 1. Artefactos obligatorios

| Artefacto | Ruta | Qué es | Detalle |
|---|---|---|---|
| Portada ejecutiva | `PROJECT.md` | Frontmatter legible por máquina + resumen para quien decide | Sección 2 y [project-manifest.md](project-manifest.md) |
| Mapa funcional | `docs/mapa-funcional.md` | Diseño funcional del sistema, sin estados de avance | Sección 3 |
| Instrucciones para agentes | `CLAUDE.md` | Debe incluir la sección "Documentación de proyecto" | Sección 4 |
| Spec Kit | `.specify/` y `specs/NNN-feature/` | Fuente de verdad técnica: requisitos, plan y tareas | Sección 5 |
| Repositorio | GitHub, organización `adminnexoru` | Con CI | Sección 6 |
| Plantilla de entorno | `.env.example` | Nombres de todas las variables de entorno, sin valores | Sección 6 |

## 2. `PROJECT.md`: portada ejecutiva

Empieza con el frontmatter YAML definido en [project-manifest.md](project-manifest.md). Después vienen estas secciones H2, con estos títulos exactos y en este orden:

| Sección | Contenido |
|---|---|
| `## Resumen ejecutivo` | Misión, problema, qué es, para quién y **métricas de éxito** (línea `**Métricas de éxito:**`). |
| `## Alcance` | **Incluye** y **Fuera de alcance**, con el motivo de cada exclusión. |
| `## Roadmap` | Tabla de fases según [roadmap.md](roadmap.md). |
| `## Decisiones clave` | Tabla `Decisión \| Razón`. Solo decisiones que condicionan el diseño o el negocio. |
| `## Costo mensual` | Tabla `Servicio \| USD/mes \| Nota`, una fila por servicio de `servicios` más las plataformas del `stack` que se pagan, y una fila **Total** igual a `costo_mensual_usd`. Un servicio sin costo lleva `—` y la nota "Sin costo". |
| `## Riesgos, bloqueos y dependencias` | Lista con prefijo en negritas: **Bloqueo**, **Dependencia**, **Riesgo de costo**, **Riesgo de calidad**, **Riesgo de calendario**, etc. |
| `## Pendientes conocidos` | Trabajo pendiente no bloqueante, con referencia a la tarea en `specs/` cuando exista. |
| `## Evidencia de validación` | Tabla `Qué \| Evidencia`: pruebas reales (IDs de registros, corridas, capturas), no afirmaciones. Indica si se validó en producción o en local. |
| `## Siguiente hito` | Qué sigue y de qué depende. Debe coincidir con `siguiente_hito` del frontmatter. |

Debajo del título H1 va una nota que remite al mapa funcional y a `specs/`, y declara la regla de precedencia (regla central b).

## 3. `docs/mapa-funcional.md`: diseño funcional

Describe **cómo funciona** el sistema, no **cuánto falta**. No lleva estados de avance: ni porcentajes, ni "completa", "en curso", "pendiente" o fechas objetivo. El avance vive solo en el roadmap de `PROJECT.md`. Sí puede declarar límites de diseño permanentes de una versión, p. ej. "en v1 no hay integración de pagos".

Frontmatter:

```yaml
---
proyecto: <id del proyecto>
tipo_documento: mapa-funcional
version_estandar: "1.0"
---
```

Secciones H2 obligatorias, en este orden. Pueden llevar numeración (`## 1. Misión`) y sumarse secciones propias del proyecto entre ellas o al final:

| Sección | Contenido |
|---|---|
| `Misión` | Una frase y qué tipo de sistema es. |
| `Componentes` | Tabla de componentes (agentes, servicios, módulos): función y nivel de autonomía o responsabilidad. |
| `Flujo` | Diagrama Mermaid (bloque ` ```mermaid `) del flujo principal entre componentes. |
| `Reglas de negocio no negociables` | Reglas que ningún componente puede romper: autonomía, datos, dinero, seguridad. |
| `Datos y fuentes` | Tabla por dato o variable: fuente, disponibilidad y **nivel de confianza**, con su regla. |
| `Integraciones` | Servicios externos: qué se consume, con qué credencial (solo el nombre de la variable de entorno), límites y costo por uso. |

El diseño detallado de cada componente puede ir en subsecciones propias (p. ej. `## Diseño por componente`).

## 4. `CLAUDE.md`: sección "Documentación de proyecto"

El `CLAUDE.md` del proyecto debe tener una sección H2 cuyo título empiece con `## Documentación de proyecto`. Su contenido es el de [templates/CLAUDE-seccion.md](../templates/CLAUDE-seccion.md): al cerrar cada fase se actualizan `PROJECT.md` y `docs/mapa-funcional.md`, y se aplica la regla de precedencia.

Si el proyecto usa `AGENTS.md` y su `CLAUDE.md` solo lo importa (`@AGENTS.md`), la sección se agrega igual en `CLAUDE.md`, debajo del import.

## 5. Spec Kit

- `.specify/` inicializado, con la constitución del proyecto en `.specify/memory/constitution.md`.
- Una carpeta por feature o fase en `specs/NNN-nombre-corto/` (`NNN` con tres dígitos, consecutivo), con:
  - `spec.md`: qué y por qué, requisitos funcionales, fuera de alcance, criterios de aceptación.
  - `plan.md`: stack, decisiones y su razón.
  - `tasks.md`: tareas con casillas `- [ ]` / `- [x]`, que alimentan el estado del roadmap.
- Una spec escrita después del código (retroactiva) es válida; en `PROJECT.md` se declara como tal.

## 6. Repositorio, CI y secretos

- Repo en GitHub, en la organización `adminnexoru`, con la visibilidad que indica [repo-visibility.md](repo-visibility.md).
- CI en GitHub Actions (`.github/workflows/*.yml`) que corre, como mínimo, en cada push y cada pull request a la rama principal. Si el proyecto tiene código, la CI incluye el chequeo de tipos o la compilación, y el lint.
- `.env.example` en la raíz, con **todas** las variables que usa el código y un comentario de para qué sirve cada una, sin valores reales.
- **Ningún secreto versionado:** `.env*` en `.gitignore` (salvo `.env.example`), sin llaves, tokens ni contraseñas en el historial de git. Si un secreto llegó a versionarse, se rota; borrarlo del árbol no basta.

## 7. Reglas centrales

**(a) Lo derivable no se captura a mano.** Lo que se puede obtener de git, GitHub o Spec Kit no se escribe en `PROJECT.md`, y si se escribe, se considera derivado y el dato de origen manda. Ejemplos:

| Dato | Se deriva de |
|---|---|
| Fecha del primer commit, último commit, autores | git |
| Visibilidad del repo, estado de la CI, PRs abiertos | GitHub |
| Estado de cada fase con specs | `tasks.md` de las specs vinculadas ([roadmap.md](roadmap.md)) |
| Lista de specs existentes | carpetas de `specs/` |

Lo que no es derivable, y por eso sí se captura, son las decisiones del Dueño del proyecto: `fase`, `estado`, fechas objetivo, `fecha_inicio`, costos y métricas de éxito.

**(b) Precedencia: manda `specs/`.** Si `PROJECT.md` o el mapa funcional no coinciden con `specs/`, se corrigen para que coincidan con `specs/`. **Excepción:** si el código y la documentación técnica del repo (`AGENTS.md`, README técnico, comentarios de esquema) coinciden entre sí y la spec es la que quedó atrás, se corrige la spec. Toda corrección se reporta: qué se cambió, dónde y por cuál de las dos vías.

**(c) Lo desconocido se marca `CONFIRMAR`.** Un dato que no se conoce y no es derivable se escribe como `CONFIRMAR` (en el frontmatter, como valor del campo; en el cuerpo, en el lugar del dato). Nunca se inventa ni se deja vacío en silencio. **Un proyecto con al menos un `CONFIRMAR` en `PROJECT.md` o en `docs/mapa-funcional.md` no es conforme** (ver [conformance.md](conformance.md)).

## 8. Idioma

Documentación de proyecto (`PROJECT.md`, `docs/`, specs) en español. Código, nombres de variables y mensajes de commit en inglés. Los valores del frontmatter van en minúsculas, sin acentos y con guiones, como se definen en [project-manifest.md](project-manifest.md).
