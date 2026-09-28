# Roadmap

Versión 1.0. Formato de la sección `## Roadmap` de `PROJECT.md` y cómo se obtiene el estado de cada fase.

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
