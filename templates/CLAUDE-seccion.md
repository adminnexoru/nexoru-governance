## Documentación de proyecto (Estándar Nexoru)

Al cerrar cada fase, actualiza `PROJECT.md` (roadmap, specs vinculadas, decisiones clave,
costo mensual, riesgos, pendientes, evidencia de validación y siguiente hito) y
`docs/mapa-funcional.md` (componentes, flujo, reglas, datos y fuentes, integraciones) para
que reflejen lo construido. El mapa funcional no lleva estados de avance.

- Si algo no coincide con `specs/`, `specs/` es la fuente de verdad. Excepción: si el
  código y la documentación técnica coinciden entre sí y la spec quedó desactualizada, se
  corrige la spec. Reporta siempre qué cambiaste y por qué.
- No captures a mano lo que se deriva de git, GitHub o Spec Kit (fecha del primer commit,
  estado de fases con `tasks.md`, estado de la CI).
- Lo que no sepas y no puedas derivar va como `CONFIRMAR` para que lo responda el Dueño;
  nunca lo inventes. Un proyecto con `CONFIRMAR` no es conforme.
- Estándar: https://github.com/adminnexoru/nexoru-governance (versión en
  `version_estandar` de `PROJECT.md`).
