# Visibilidad de repositorios

Versión 1.0. Criterio para decidir si un repo de la organización `adminnexoru` es público o privado.

## Regla

| Visibilidad | Cuándo |
|---|---|
| **Privado** | Repos de clientes (`tipo: producto-cliente`), o con estrategia comercial sensible. Ante la duda, privado. |
| **Público** | Repos sin datos sensibles: estándares, plantillas, herramientas genéricas y productos cuyo contenido no revela estrategia comercial. |

Cambiar de privado a público es una decisión del Dueño, previa revisión del **historial completo** de git, no solo del árbol actual: lo que se borró de un archivo sigue siendo visible en commits anteriores.

## Nunca en un repo público

- Precios a clientes, cotizaciones, propuestas o condiciones comerciales.
- Contactos: nombres, correos, teléfonos de clientes o proveedores.
- Márgenes, costos negociados o costos reales de compra.
- Qué productos, mercados o proveedores evalúa o eligió Nexoru, cuando eso revela estrategia comercial (p. ej. productos candidatos, proveedores seleccionados, resultados de análisis).
- Datos de clientes o de usuarios finales.
- Secretos de cualquier tipo. Esto aplica también a repos privados (ver [project-standard.md §6](project-standard.md#6-repositorio-ci-y-secretos)).

Si un proyecto público necesita documentar evidencia de validación con datos así, la evidencia va en un repo o almacenamiento privado y `PROJECT.md` la referencia sin reproducirla.

## Este repo

`nexoru-governance` solo contiene el estándar y plantillas, sin datos comerciales. Los ejemplos usan un proyecto ficticio (Proyecto Demo) y no nombran proyectos reales. Puede ser público mientras respete esta regla.
