# Conformidad

Versión 1.0. Define los niveles de conformidad de un proyecto. Cada verificación está escrita para que un programa la ejecute sobre el repo sin interpretación humana.

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

Todas las rutas son relativas a la raíz del repo. "Sección X" significa una línea que coincide con `^## (\d+\.\s+)?X\s*$`, sin distinguir mayúsculas.

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
| 1.8 | Existen las 9 secciones H2 de [project-standard.md §2](project-standard.md#2-projectmd-portada-ejecutiva), en ese orden. |
| 1.9 | `## Resumen ejecutivo` contiene una línea que empieza con `**Métricas de éxito:**`. |
| 1.10 | `## Costo mensual` contiene una tabla con encabezado `Servicio \| USD/mes \| Nota`, una fila por cada elemento de `servicios` y una fila **Total**. |
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
| 3.2 | La última ejecución de CI en la rama principal terminó con éxito (consulta a la API de GitHub). |
| 3.3 | `## Roadmap` contiene una tabla con el encabezado exacto de [roadmap.md](roadmap.md). |
| 3.4 | Cada valor de la columna `Specs` es `—` o una lista de carpetas que existen en `specs/`. |
| 3.5 | Toda carpeta de `specs/` aparece vinculada en al menos una fase. |
| 3.6 | Las fases con estado derivado (todas sus specs tienen `tasks.md`) tienen `Estado manual` vacío. |
| 3.7 | Las fases sin estado derivado tienen `Estado manual` con un valor permitido. |
| 3.8 | Toda fase con `Estado manual` = `bloqueada` tiene una entrada **Bloqueo** en `## Riesgos, bloqueos y dependencias`. |

## Hallazgos fuera de nivel

Estas verificaciones son obligatorias según [project-standard.md §6](project-standard.md#6-repositorio-ci-y-secretos). No cambian el nivel, pero el programa las reporta siempre:

| Severidad | Verificación |
|---|---|
| **Crítico** | Un archivo versionado coincide con `.env*` (salvo `.env.example`), o un escáner de secretos (p. ej. gitleaks) encuentra credenciales en el historial. |
| Alto | La visibilidad del repo contradice [repo-visibility.md](repo-visibility.md) (p. ej. `producto-cliente` en un repo público). |
| Medio | No existe `.env.example`, o le falta alguna variable que el código lee. |
| Bajo | El frontmatter tiene comentarios YAML. |

## Salida esperada del programa

```
proyecto: <id>
nivel: <0-3>
fallas:       lista de verificaciones fallidas del siguiente nivel, con número y detalle
advertencias: specs sin plan.md/tasks.md, comentarios YAML, etc.
hallazgos:    lista con severidad
```
