# Manifiesto del proyecto: frontmatter de `PROJECT.md`

Versión 1.3. El frontmatter de `PROJECT.md` es el manifiesto del proyecto: lo leen el dashboard, los programas de conformidad y Obsidian (como propiedades).

## Reglas de formato

- Bloque YAML entre `---` al inicio del archivo.
- **Solo valores planos** (texto, número, fecha) y **listas simples de texto**. Sin objetos anidados, sin listas de objetos, sin YAML multilínea (`|`, `>`), sin anclas. Así lo exigen las propiedades de Obsidian.
- Fechas en formato `AAAA-MM-DD`, sin comillas.
- Los valores enumerados van exactamente como se listan aquí: minúsculas, sin acentos, con guion.
- Se desaconsejan los comentarios YAML (`# ...`): Obsidian los borra al editar propiedades. Si un dato necesita explicación, va en el cuerpo.
- Un dato desconocido se escribe `CONFIRMAR`. Un proyecto con `CONFIRMAR` en el frontmatter no es conforme.
- Los campos opcionales que no aplican se omiten o se dejan vacíos. No se llenan con `CONFIRMAR`. Excepción: la plantilla trae `visibilidad: CONFIRMAR` para que el Dueño la decida; si todavía no decide, la línea se borra.

## Campos

| Campo | Tipo | Obligatorio | Valores permitidos / formato | Descripción |
|---|---|---|---|---|
| `id` | texto | Sí | `^[a-z0-9]+(-[a-z0-9]+)*$` | Identificador corto y estable. No cambia aunque cambie el nombre. |
| `nombre` | texto | Sí | Libre | Nombre legible del proyecto. |
| `tipo` | texto | Sí | `producto-cliente`, `producto-nexoru`, `interno` | Para quién se construye: un cliente, Nexoru como producto, o uso interno. |
| `cliente` | texto | Sí | Libre | Nombre del cliente. En `producto-nexoru` e `interno` es `Nexoru`. |
| `fase` | texto | Sí | `idea`, `especificacion`, `construccion`, `pruebas`, `piloto`, `migracion`, `operacion`, `pausado`, `retirado` | Fase actual del ciclo de vida ([lifecycle.md](lifecycle.md)). |
| `fase_desde` | fecha | Sí | `AAAA-MM-DD` | Desde cuándo el proyecto está en la `fase` actual. |
| `estado` | texto | Sí | `verde`, `ambar`, `rojo` | Salud del proyecto frente a su `fecha_objetivo` (ver abajo). |
| `despliegue` | texto | Sí | `nexoru-subdominio`, `dominio-cliente`, `local`, `ninguno` | Dónde corre hoy. |
| `urls` | lista de texto | Si `despliegue` es `nexoru-subdominio` o `dominio-cliente` | URLs `https://...` | URLs públicas o de acceso del sistema. |
| `repo` | texto | Sí | `organizacion/nombre` | Repositorio en GitHub. |
| `visibilidad` | texto | No | `publico`, `privado` | Visibilidad del repo **decidida por el Dueño**, según [repo-visibility.md](repo-visibility.md). La razón se registra como fila en `## Decisiones clave` (revisión humana). Vacío equivale a no declarado. |
| `fecha_inicio` | fecha | Sí | `AAAA-MM-DD` | Inicio del proyecto, **decidido por el Dueño**. No es la fecha del primer commit, que se deriva de git y no se captura. |
| `fecha_objetivo` | fecha | Sí, salvo en `operacion`, `pausado` y `retirado` | `AAAA-MM-DD`, mayor o igual a `fecha_inicio` | Fecha comprometida para la próxima meta del proyecto. En `operacion` es opcional: cada incremento lleva su fecha en la columna `Fecha objetivo` del roadmap. |
| `stack` | lista de texto | Sí | Minúsculas con guion (`nextjs`, `postgres`, `vercel`) | Tecnologías y plataformas base. |
| `servicios` | lista de texto | Sí (puede ser vacía) | Minúsculas con guion (`api-pagos`, `api-correo`) | Servicios externos consumidos. Cada uno debe tener fila en `## Costo mensual` (ver [correspondencia fila–servicio](project-standard.md#tabla-de-costo-mensual)). |
| `costo_mensual_usd` | número | Sí | Número `>= 0`, sin símbolo ni comas | Suma de la tabla `## Costo mensual`. |
| `siguiente_hito` | texto | Sí | Libre, una línea | Resumen de `## Siguiente hito`. |
| `mapa_funcional` | texto | Sí | `docs/mapa-funcional.md` | Ruta del mapa funcional. |
| `version_estandar` | texto | Sí | `"MAJOR.MINOR"` entre comillas, p. ej. `"1.0"` | Versión del estándar con la que cumple el proyecto. |

## Criterio de `estado`

| Valor | Cuándo |
|---|---|
| `verde` | La `fecha_objetivo` es alcanzable sin cambios de alcance y no hay bloqueos sin plan. |
| `ambar` | Hay un riesgo concreto (listado en `## Riesgos, bloqueos y dependencias`) que puede mover la `fecha_objetivo` o el alcance si no se resuelve a tiempo. |
| `rojo` | La `fecha_objetivo` ya no es alcanzable tal cual, o hay un bloqueo sin plan de salida. |

Quien lo decide es el Dueño. Si un riesgo tiene fecha límite, se escribe en el riesgo cuándo cambiaría el estado (p. ej. "si X no avanza antes del 2026-11-30, pasa a `ambar`").

## Validaciones cruzadas

Un programa de conformidad verifica además que:
- `fase_desde` no sea posterior a la fecha de evaluación.
- `fecha_objetivo`, si existe, sea mayor o igual a `fecha_inicio`.
- `urls` no esté vacía si `despliegue` es `nexoru-subdominio` o `dominio-cliente`.
- `repo` coincida con el remoto `origin` de git.
- `costo_mensual_usd` sea igual a la fila **Total** de `## Costo mensual`.
- `mapa_funcional` apunte a un archivo existente.

## Ejemplo (Proyecto Demo, ficticio)

```yaml
---
id: demo
nombre: Proyecto Demo
tipo: producto-nexoru
cliente: Nexoru
fase: construccion
fase_desde: 2026-02-01
estado: verde
despliegue: nexoru-subdominio
urls:
  - https://demo.nexoru.ai
repo: adminnexoru/proyecto-demo
visibilidad: privado
fecha_inicio: 2026-01-15
fecha_objetivo: 2026-12-15
stack:
  - nextjs
  - postgres
  - vercel
servicios:
  - api-pagos
  - api-correo
costo_mensual_usd: 45
siguiente_hito: Fase 3, pagos en línea
mapa_funcional: docs/mapa-funcional.md
version_estandar: "1.3"
---
```

Con su fila en `## Decisiones clave`:

```markdown
| Decisión | Razón |
|---|---|
| Repo privado (`visibilidad: privado`) | La evidencia de validación del piloto incluye datos de usuarios reales |
```
