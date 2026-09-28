# Nexoru Governance: instrucciones para Claude Code

Este repo define el Estándar de Proyecto Nexoru. Es solo documentación en Markdown, en español. No agregues código, scripts ni dependencias.

## Cómo mantener el estándar

- **Fuente de verdad por tema:** cada regla vive en un solo archivo de `standard/`. Si otro archivo la necesita, enlázala en vez de copiarla. Las plantillas de `templates/` son la excepción: deben reflejar exactamente lo que dicen `standard/project-standard.md`, `standard/project-manifest.md` y `standard/roadmap.md`.
- **Consistencia al cambiar algo:** un cambio en una regla obliga a revisar, en el mismo commit:
  - las plantillas de `templates/`;
  - las verificaciones de `standard/conformance.md` (toda regla obligatoria debe poder verificarse con un programa, o quedar marcada explícitamente como revisión humana);
  - el prompt de `migration/guide.md`.
- **Proyectos conformes:** antes de cambiar una estructura, revisa si deja fuera de conformidad a proyectos que hoy cumplen. Si es así, es un cambio MAJOR y se dice explícitamente en el CHANGELOG.
- **Nada comercial sensible:** este repo no contiene precios a clientes, contactos, márgenes ni datos de clientes. Los ejemplos usan un proyecto ficticio, "Proyecto Demo", con datos genéricos; no se nombran proyectos reales ni se incluyen sus datos (ver `standard/repo-visibility.md`). Las evaluaciones de conformidad de cada proyecto no van aquí: las calcula el dashboard.

## Versionado (semver)

El estándar se versiona con semver (`MAJOR.MINOR.PATCH`), y cada cambio se registra en `CHANGELOG.md`:

| Cambio | Versión | Ejemplos |
|---|---|---|
| **MAJOR** | Deja fuera de conformidad a proyectos que hoy cumplen | Campo obligatorio nuevo, valor permitido eliminado, sección obligatoria nueva, verificación más estricta |
| **MINOR** | Agrega algo sin romper a los proyectos conformes | Campo opcional nuevo, valor permitido nuevo, plantilla o guía nueva |
| **PATCH** | No cambia ninguna regla | Redacción, erratas, ejemplos, enlaces |

Al publicar una versión:
1. Agrega la entrada al inicio de `CHANGELOG.md`, con fecha (AAAA-MM-DD) y cambios agrupados en **Agregado**, **Cambiado**, **Eliminado** y **Corregido**.
2. Actualiza la versión en `README.md`.
3. Si cambió MAJOR o MINOR, actualiza `version_estandar` en `templates/PROJECT.md` y en `templates/docs/mapa-funcional.md`. `version_estandar` guarda solo MAJOR.MINOR.
4. En un cambio MAJOR, agrega a `migration/guide.md` una sección con los pasos para pasar de la versión anterior a la nueva.
5. Crea un tag git `vMAJOR.MINOR.PATCH`.

Los mensajes de commit van en inglés. La documentación va en español.
