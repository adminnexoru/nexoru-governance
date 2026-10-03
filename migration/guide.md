# Guía de migración al Estándar de Proyecto Nexoru

Versión 1.2. Paso a paso para adecuar un proyecto existente en `/proyectos/<nombre>`.

## Resumen

La migración se hace en dos rondas, cada una con su commit:

1. **Ronda del agente:** copiar plantillas, llenar lo derivable, conciliar contra `specs/` y el código, y listar los `CONFIRMAR` para el Dueño. Commit en una rama, **sin push**, para que el Dueño revise.
2. **Ronda del Dueño:** el Dueño responde los `CONFIRMAR`, se verifica que no quede ninguno, commit, push y PR.

## Pasos

### 1. Preparar

- Trabaja en una rama nueva (p. ej. `docs/nexoru-standard`), nunca directo en la rama principal.
- Si el proyecto no tiene Spec Kit, inicialízalo antes (`specify init`). Si tiene fases ya construidas sin specs, se pueden formalizar como specs retroactivas.

### 2. Copiar las plantillas

| Plantilla | Destino en el proyecto |
|---|---|
| `templates/PROJECT.md` | `PROJECT.md` |
| `templates/docs/mapa-funcional.md` | `docs/mapa-funcional.md` |
| `templates/CLAUDE-seccion.md` | Se agrega al final de `CLAUDE.md` (debajo de `@AGENTS.md`, si existe) |

Si ya existe un documento de diseño (un mapa, un README largo, un documento de visión), su contenido se migra a `PROJECT.md` y al mapa funcional según la sección que corresponda, en vez de empezar de cero.

### 3. Llenar lo derivable

Desde el repo, sin preguntar al Dueño:

| Campo | De dónde sale |
|---|---|
| `repo` | `git remote get-url origin` |
| `id`, `nombre` | Nombre del repo y `README.md`/`AGENTS.md` (el Dueño puede corregirlo en la revisión) |
| `stack`, `servicios` | `package.json` (o equivalente), variables de entorno usadas en el código, `AGENTS.md` |
| `despliegue`, `urls` | Configuración de hosting (`.vercel/`, `vercel.json`/`vercel.ts`), dominio en la documentación |
| Columna `Specs` del roadmap | Carpetas de `specs/`, emparejadas con cada fase por su título |
| Decisiones clave, evidencia, pendientes | `specs/*/plan.md` ("Decisiones y su razón"), `tasks.md` (evidencia y tareas abiertas), `AGENTS.md` |

`fecha_inicio` **no** se deriva: es una decisión del Dueño. La fecha del primer commit se puede ofrecer como referencia en el reporte, pero no se escribe en el campo.

### 4. Conciliar contra `specs/` y el código

Compara `PROJECT.md` y el mapa funcional contra `specs/`, la documentación técnica (`AGENTS.md`, README) y el código. Para cada diferencia, aplica la regla de precedencia ([project-standard.md §7b](../standard/project-standard.md#7-reglas-centrales)):

- **Manda `specs/`:** corrige `PROJECT.md` o el mapa.
- **Excepción:** si el código y la documentación técnica coinciden entre sí y la spec es la desactualizada, corrige la spec.

Busca en particular:
- Afirmaciones sin respaldo en `specs/`: evidencia que no aparece en ningún `tasks.md`, funciones descritas como construidas que no existen.
- Valores de enumeraciones distintos a los reales (en Proyecto Demo: el documento de diseño decía `aprobado`/`rechazado` y el código usa `aprobado`/`rechazado`/`en_revision`).
- Reglas generalizadas de más (en Proyecto Demo: "toda validación ocurre en el servidor" solo era cierto para dos de los módulos).
- Fuentes de datos mal atribuidas (en Proyecto Demo: un dato atribuido a una API externa que en realidad calcula el propio sistema).
- Tareas abiertas en `tasks.md` que no aparecen en `## Pendientes conocidos`.
- Specs sin `tasks.md` y criterios de aceptación sin marcar.

**Reporta todas las diferencias**: qué decía, qué dice ahora, y en qué archivo y spec te basaste. Las diferencias que no te toca resolver (p. ej. una inconsistencia interna entre dos specs) se reportan sin cambiarlas.

### 5. Listar los `CONFIRMAR` para el Dueño

Lo que no es derivable queda como `CONFIRMAR`, y se entrega al Dueño como una lista de preguntas concretas. Normalmente son:
- `fase`, `fase_desde`, `estado`
- `fecha_inicio`, `fecha_objetivo` y las fechas objetivo de las fases del roadmap
- `costo_mensual_usd` y el desglose por servicio
- Métricas de éxito
- `Estado manual` de fases sin `tasks.md`
- `visibilidad` del repo (`publico` o `privado`) y su razón para `## Decisiones clave`

Si propones un valor (p. ej. métricas de éxito), déjalo escrito después de `CONFIRMAR` para que el Dueño solo lo acepte o lo corrija.

### 6. Commit de la ronda del agente, sin push

```
docs: add PROJECT.md and functional map (Nexoru standard draft)
```

No hagas push hasta que el Dueño revise.

### 7. Ronda del Dueño

1. Aplica las respuestas del Dueño y quita todas las marcas `CONFIRMAR`.
2. Verifica: `grep -n CONFIRMAR PROJECT.md docs/mapa-funcional.md` no devuelve nada.
3. Evalúa el nivel con [conformance.md](../standard/conformance.md) y repórtalo junto con lo que falta para el siguiente nivel.
4. Commit, push de la rama y PR a la rama principal:
   ```
   docs: complete PROJECT.md for Nexoru standard
   ```

## Prompt para la ronda del agente

Pega este prompt en Claude Code, desde la raíz del proyecto. Reemplaza `<nombre>` y la ruta del estándar si hace falta.

```text
Responde en español. Vas a migrar este proyecto al Estándar de Proyecto Nexoru v1.2.
El estándar está en /proyectos/nexoru-governance (si no existe, clónalo de
adminnexoru/nexoru-governance). Antes de escribir, lee standard/project-standard.md,
standard/project-manifest.md, standard/roadmap.md, standard/conformance.md y
migration/guide.md.

Trabaja en una rama nueva. Luego:

1. Copia las plantillas: templates/PROJECT.md a PROJECT.md, templates/docs/mapa-funcional.md
   a docs/mapa-funcional.md, y agrega templates/CLAUDE-seccion.md al final de CLAUDE.md.
   Si ya existe un documento de diseño del proyecto, migra su contenido a esos dos archivos.
2. Llena desde el repo todo lo derivable: repo (git remote), stack, servicios, despliegue,
   urls, la columna Specs del roadmap (según las carpetas de specs/), decisiones clave,
   evidencia de validación y pendientes conocidos (según plan.md, tasks.md y AGENTS.md).
   No llenes fecha_inicio con la fecha del primer commit: es decisión del Dueño.
3. Compara PROJECT.md y docs/mapa-funcional.md contra specs/, la documentación técnica
   (AGENTS.md, README) y el código. Si algo no coincide, manda specs/: corrige los dos
   archivos. Excepción: si el código y la documentación técnica coinciden entre sí y la
   spec está desactualizada, corrige la spec. Dime qué cambiaste, en qué te basaste, y qué
   inconsistencias encontraste que no te tocaba resolver.
4. Deja como CONFIRMAR todo lo que no puedas derivar y lístame las preguntas que solo yo
   puedo responder (fase, fase_desde, estado, fecha_inicio, fecha_objetivo y fechas del
   roadmap, costo mensual por servicio, métricas de éxito, estado manual de fases sin
   tasks.md, visibilidad del repo y su razón). Si tienes una propuesta, déjala escrita
   después del CONFIRMAR.
5. Evalúa el nivel de conformidad actual según standard/conformance.md y dime qué falta
   para el siguiente.
6. Haz commit con el mensaje "docs: add PROJECT.md and functional map (Nexoru standard
   draft)". No hagas push hasta que yo lo revise.
```

## Prompt para la ronda del Dueño

```text
Responde en español. Completa los CONFIRMAR de PROJECT.md y docs/mapa-funcional.md con mis
respuestas:
- fase: ...; fase_desde: ...; estado: ...
- fecha_inicio: ...; fecha_objetivo: ... Fechas objetivo del roadmap: ...
- Costo mensual por servicio: ... (total en costo_mensual_usd)
- Métricas de éxito: ...
- Estado manual de las fases sin tasks.md: ...
- visibilidad: publico | privado; razón: ... (o "sin decidir": borra la línea)
Verifica que no quede ningún CONFIRMAR en PROJECT.md ni en docs/mapa-funcional.md, evalúa
el nivel de conformidad según el estándar, haz commit con el mensaje "docs: complete
PROJECT.md for Nexoru standard", haz push de la rama y abre un PR. Al terminar, muéstrame
el frontmatter final de PROJECT.md y el nivel alcanzado.
```
