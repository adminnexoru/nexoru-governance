# Conformidad

Versión 1.3. Define los niveles de conformidad de un proyecto. Cada verificación está escrita para que un programa la ejecute sobre el repo sin interpretación humana.

## Niveles

Los niveles son acumulativos: un proyecto está en el nivel más alto cuyas verificaciones, **y las de todos los niveles anteriores**, pasan completas.

| Nivel | Nombre | Requisito |
|---|---|---|
| **0** | No conforme | No existe `PROJECT.md`, o existe pero no pasa el nivel 1. |
| **1** | Portada | `PROJECT.md` válido y sin `CONFIRMAR`. |
| **2** | Documentado | Nivel 1 + `docs/mapa-funcional.md` válido, `CLAUDE.md` con la sección "Documentación de proyecto" y Spec Kit. |
| **3** | Trazable | Nivel 2 + CI en GitHub y roadmap con specs vinculadas. |

Un proyecto que dice "cumple el estándar" está al menos en nivel 1.

## Verificaciones

Un evaluador del portafolio recorre las subcarpetas de `PROJECTS_ROOT` y omite las listadas en `.nexoruignore` ([project-standard.md §8](project-standard.md#8-portafolio-nexoruignore)). Todas las rutas son relativas a la raíz del repo. "Sección X" significa una línea que coincide con `^## (\d+\.\s+)?X\s*$`, sin distinguir mayúsculas.

### Nivel 1: `PROJECT.md` válido sin `CONFIRMAR`

| # | Verificación |
|---|---|
| 1.1 | Existe `PROJECT.md`. |
| 1.2 | Empieza con un bloque YAML entre `---` que se puede parsear. |
| 1.3 | Todos los campos obligatorios de [project-manifest.md](project-manifest.md) existen, con el tipo correcto. |
| 1.4 | Los campos enumerados (`tipo`, `fase`, `estado`, `despliegue`) tienen un valor permitido. |
| 1.5 | Los valores son planos o listas simples de texto (sin objetos anidados). |
| 1.6 | Las fechas tienen formato `AAAA-MM-DD`. |
| 1.7 | Se cumplen las validaciones cruzadas de [project-manifest.md](project-manifest.md#validaciones-cruzadas). |
| 1.8 | Existen las 9 secciones H2 de [project-standard.md §2](project-standard.md#2-projectmd-portada-ejecutiva), en ese orden. La sección opcional `## Incrementos planeados`, si existe, está entre `## Roadmap` y `## Decisiones clave` y contiene una tabla con el encabezado exacto `Incremento \| Objetivo \| Prioridad \| Referencia`, con `Prioridad` en `alta`, `media` o `baja`. |
| 1.9 | `## Resumen ejecutivo` contiene una línea que empieza con `**Métricas de éxito:**`. |
| 1.10 | `## Costo mensual` contiene una tabla con encabezado `Servicio \| USD/mes \| Nota`, una fila por cada elemento de `servicios` (correspondencia según [project-standard.md](project-standard.md#tabla-de-costo-mensual)) y una fila **Total**. |
| 1.11 | La cadena `CONFIRMAR` no aparece en ningún lugar de `PROJECT.md`. |

### Nivel 2: mapa funcional, `CLAUDE.md` y Spec Kit

| # | Verificación |
|---|---|
| 2.1 | Existe el archivo al que apunta `mapa_funcional` (`docs/mapa-funcional.md`). |
| 2.2 | Su frontmatter tiene `proyecto` igual al `id` de `PROJECT.md`, `tipo_documento: mapa-funcional` y `version_estandar`. |
| 2.3 | Tiene las secciones `Misión`, `Componentes`, `Flujo`, `Reglas de negocio no negociables`, `Datos y fuentes` e `Integraciones`, en ese orden relativo (con numeración opcional y con otras secciones intercaladas permitidas). |
| 2.4 | Contiene al menos un bloque ` ```mermaid `. |
| 2.5 | No contiene `CONFIRMAR`. |
| 2.6 | Ninguna celda de tabla tiene como valor completo un estado del roadmap (`completa`, `en-curso`, `implementada-sin-validar`, `bloqueada`, `pendiente`), y ninguna tabla tiene una columna titulada `Estado`, `Estado manual` o `Fecha objetivo`. |
| 2.7 | Existe `CLAUDE.md` con una línea que coincide con `^## Documentación de proyecto`. |
| 2.8 | Esa sección menciona `PROJECT.md`, `docs/mapa-funcional.md` y `specs/`. |
| 2.9 | Existen `.specify/` y `.specify/memory/constitution.md`. |
| 2.10 | Existe `specs/` con al menos una carpeta que coincide con `^\d{3}-[a-z0-9-]+$`, y cada una de esas carpetas tiene `spec.md`. |

Una carpeta de spec sin `plan.md` o sin `tasks.md` no rompe el nivel 2, pero el programa la reporta como advertencia. Sin `tasks.md`, su fase necesita `Estado manual` (ver nivel 3).

### Nivel 3: CI y roadmap con specs vinculadas

| # | Verificación |
|---|---|
| 3.1 | Existe al menos un archivo `.github/workflows/*.yml` o `*.yaml` que se dispara con `push` y `pull_request`. |
| 3.2 | En la rama principal, la ejecución terminada más reciente de **cada** workflow que satisface 3.1 terminó con éxito (consulta a la API de GitHub). Un workflow sin ejecuciones terminadas en la rama principal no cumple. |
| 3.3 | `## Roadmap` contiene una tabla con el encabezado exacto de [roadmap.md](roadmap.md). |
| 3.4 | Cada valor de la columna `Specs` es `—` o una lista de carpetas que existen en `specs/`. |
| 3.5 | Toda carpeta de `specs/` aparece vinculada en al menos una fase. |
| 3.6 | Las fases con estado derivado (todas sus specs tienen `tasks.md`) tienen `Estado manual` vacío. |
| 3.7 | Las fases sin estado derivado tienen `Estado manual` con un valor permitido. |
| 3.8 | Toda fase con `Estado manual` = `bloqueada` tiene una entrada **Bloqueo** en `## Riesgos, bloqueos y dependencias`. |

## Hallazgos fuera de nivel

Estas verificaciones vienen de [project-standard.md §6](project-standard.md#6-repositorio-ci-y-secretos) y de [lifecycle.md](lifecycle.md#cierre-y-apertura-de-incrementos). No cambian el nivel, pero el programa las reporta siempre. "Concluida" y "concluido" se definen en [roadmap.md](roadmap.md#fase-y-roadmap-concluidos); las filas de `## Incrementos planeados` no cuentan como fases:

| Severidad | Verificación |
|---|---|
| **Crítico** | Un archivo versionado coincide con `.env*` (salvo `.env.example`), o un escáner de secretos (p. ej. gitleaks) encuentra credenciales en el historial. |
| Alto | `tipo: producto-cliente` en un repo público, aunque tenga `visibilidad: publico` ([repo-visibility.md](repo-visibility.md)). |
| Alto | Requiere decisión del Dueño: `tipo` es `producto-nexoru` o `producto-cliente` y no declara `visibilidad` ([repo-visibility.md](repo-visibility.md#visibilidad-declarada)). |
| Alto | Discrepancia de visibilidad: `visibilidad` declarada no coincide con la visibilidad real del repo en GitHub. |
| Medio | No existe `.env.example`, o le falta alguna variable que el código lee. |
| Medio | `fase: operacion` con fases pendientes en el roadmap: al menos una fase no está concluida. |
| Medio | `fase: construccion` o `fase: especificacion` con el roadmap concluido. |
| Bajo | El frontmatter tiene comentarios YAML. |

Si `visibilidad` está declarada y coincide con la real, el programa la reporta como **aceptada**, sin hallazgo.

## Salida esperada del programa

```
proyecto: <id>
nivel: <0-3>
fallas:       lista de verificaciones fallidas del siguiente nivel, con número y detalle
advertencias: specs sin plan.md/tasks.md, comentarios YAML, etc.
hallazgos:    lista con severidad
visibilidad:  aceptada | requiere-decision | discrepancia | sin-declarar (interno)
```
