# Roadmap

Versión 1.3. Formato de la sección `## Roadmap` de `PROJECT.md` y cómo se obtiene el estado de cada fase.

## Formato de la tabla

Encabezado exacto, con columnas en este orden:

```markdown
| Fase | Objetivo | Specs | Fecha objetivo | Estado manual |
|---|---|---|---|---|
```

| Columna | Contenido | Formato |
|---|---|---|
| `Fase` | Identificador de la fase | Número o número con decimal (`0`, `1`, `1.5`, `5.2`), único en la tabla y en orden ascendente |
| `Objetivo` | Qué entrega la fase, en una línea | Texto libre |
| `Specs` | Carpetas de `specs/` vinculadas | Nombres de carpeta exactos (`002-catalogo`) separados por coma y espacio, o `—` si no hay spec |
| `Fecha objetivo` | Fecha comprometida para cerrar la fase | `AAAA-MM-DD`, o `—` si la fase ya cerró o no tiene fecha comprometida |
| `Estado manual` | Estado capturado a mano, solo cuando no se puede derivar | Valor permitido (abajo) o vacío |

Antes de la tabla va un párrafo que diga cómo se obtiene el estado y que explique cualquier caso particular (p. ej. specs retroactivas o fases con spec sin `tasks.md`).

## Estado derivado

El estado de una fase **se deriva** de los `tasks.md` de sus specs vinculadas, contando las casillas de tarea (`- [ ]` pendiente, `- [x]` o `- [X]` hecha) de todas sus specs juntas:

| Condición | Estado derivado |
|---|---|
| Todas las tareas marcadas | `completa` |
| Al menos una marcada y al menos una pendiente | `en-curso` |
| Ninguna marcada | `pendiente` |

Una fase solo tiene estado derivado si **todas** sus specs vinculadas tienen `tasks.md`.

Un pendiente no bloqueante que aparece después de cerrar una fase no se agrega como casilla abierta al `tasks.md` de esa fase, porque la regresaría a `en-curso`. Va en `## Pendientes conocidos` de `PROJECT.md` o en una spec nueva.

## Estado manual

`Estado manual` **solo** se llena en fases sin estado derivado:
- fases con `Specs` = `—`; o
- fases con specs donde al menos una no tiene `tasks.md`.

En cualquier otra fase, `Estado manual` va vacío: el dato capturado a mano no puede contradecir al derivado (regla central a).

Valores permitidos:

| Valor | Significado |
|---|---|
| `completa` | Entregada y validada. |
| `implementada-sin-validar` | El código está en la rama principal, pero sus criterios de aceptación no están verificados con pruebas reales. |
| `en-curso` | Se está trabajando. |
| `bloqueada` | No puede avanzar por una dependencia externa, que debe aparecer como **Bloqueo** en `## Riesgos, bloqueos y dependencias`. |
| `pendiente` | Planeada, sin iniciar. |

Toda fase sin estado derivado debe tener `Estado manual` no vacío.

## Fase y roadmap concluidos

Una **fase del roadmap está concluida** si:
- tiene estado derivado y el 100% de sus tareas está marcado (estado derivado `completa`); o
- no tiene estado derivado y su `Estado manual` es `completa`.

Cualquier otro estado (`en-curso`, `implementada-sin-validar`, `bloqueada`, `pendiente`) significa que la fase no está concluida.

El **roadmap está concluido** si tiene al menos una fase y todas sus fases están concluidas. Las filas de `## Incrementos planeados` ([project-standard.md](project-standard.md#sección-opcional--incrementos-planeados)) no son fases y no cuentan. Qué pasa con la `fase` del proyecto cuando el roadmap concluye, y cuando se abre un incremento, se define en [lifecycle.md](lifecycle.md#cierre-y-apertura-de-incrementos).

## Apertura de un incremento

Al abrir un incremento, en el mismo commit:
1. Su fila **sale** de `## Incrementos planeados` (si estaba ahí).
2. **Entra** al roadmap como la siguiente fase: identificador mayor que el de la última fase, `Specs` con la carpeta de su spec en `specs/` (con al menos `spec.md`) y `Fecha objetivo` con su fecha comprometida.

Los cambios de `fase`, `fase_desde` y mapa funcional que acompañan la apertura están en [lifecycle.md](lifecycle.md#cierre-y-apertura-de-incrementos).

## Ejemplo

```markdown
| Fase | Objetivo | Specs | Fecha objetivo | Estado manual |
|---|---|---|---|---|
| 0 | Arquitectura, hosting, dominio, base de datos | — | — | completa |
| 1 | Registro de usuarios | 001-registro-usuarios | — | |
| 2 | Catálogo y búsqueda | 002-catalogo, 003-busqueda | — | implementada-sin-validar |
| 3 | Pagos en línea | — | 2026-12-15 | bloqueada |
| 4 | Reportes | — | 2026-12-15 | pendiente |
```

Con un incremento planeado en `## Incrementos planeados`:

```markdown
| Incremento | Objetivo | Prioridad | Referencia |
|---|---|---|---|
| Programa de lealtad | Puntos por compra y canje en el catálogo | media | B-007 |
```

Cuando el Dueño lo abre, la fila sale de esa tabla y entra al roadmap como fase 5, p. ej. `| 5 | Programa de lealtad | 004-programa-lealtad | 2027-03-31 | |`.
