# Nexoru Governance

Estándares de gobierno para los proyectos de Nexoru. Este repo contiene solo documentación en Markdown: no hay código.

## Propósito

Que todo proyecto de Nexoru, sea producto propio, producto para un cliente o herramienta interna, se pueda entender, auditar y comparar con la misma estructura:

- Una **portada ejecutiva** (`PROJECT.md`) con metadatos legibles por máquina, que un dashboard o Obsidian pueden leer.
- Un **mapa funcional** (`docs/mapa-funcional.md`) con el diseño del sistema, sin estados de avance.
- **Specs** (Spec Kit) como fuente de verdad técnica.
- Reglas verificables por un programa para decidir si un proyecto es conforme y en qué nivel.

## Estructura del repo

```
nexoru-governance/
├── README.md                     este archivo
├── CLAUDE.md                     cómo mantener y versionar el estándar
├── CHANGELOG.md                  historial de versiones del estándar
├── standard/
│   ├── project-standard.md       qué debe tener todo proyecto
│   ├── project-manifest.md       esquema del frontmatter de PROJECT.md
│   ├── roadmap.md                formato de la tabla de roadmap y cómo se deriva el estado
│   ├── lifecycle.md              ciclo de vida de un producto Nexoru
│   ├── conformance.md            niveles de conformidad evaluables por un programa
│   └── repo-visibility.md        criterio para repos públicos o privados
├── templates/
│   ├── PROJECT.md                plantilla de la portada ejecutiva
│   ├── CLAUDE-seccion.md         sección "Documentación de proyecto" para CLAUDE.md
│   └── docs/
│       └── mapa-funcional.md     plantilla del mapa funcional
└── migration/
    └── guide.md                  cómo adecuar un proyecto existente, con el prompt a usar
```

## Cómo se usa

**Proyecto nuevo:**
1. Copia `templates/PROJECT.md` a la raíz del proyecto y `templates/docs/mapa-funcional.md` a `docs/`.
2. Agrega a su `CLAUDE.md` el contenido de `templates/CLAUDE-seccion.md`.
3. Inicializa Spec Kit (`.specify/` y `specs/`).
4. Reemplaza cada `CONFIRMAR` con el dato real. Mientras quede alguno, el proyecto no es conforme.

**Proyecto existente:** sigue [migration/guide.md](migration/guide.md). Incluye el prompt exacto para hacer la migración con Claude Code.

**Evaluar un proyecto:** aplica las verificaciones de [standard/conformance.md](standard/conformance.md). Están escritas para que un programa las ejecute sin interpretación.

## Versión

Versión actual del estándar: **1.1.0** (ver [CHANGELOG.md](CHANGELOG.md)). En el frontmatter de cada proyecto, `version_estandar` registra la versión MAJOR.MINOR con la que cumple, p. ej. `"1.1"`.
