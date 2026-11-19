# Desarrollo con OpenSpec

- Usa las skills del repositorio en `.agents/skills/` para gestionar cambios con el esquema `spec-driven`. Lee el `SKILL.md` correspondiente antes de ejecutar el flujo.
- En la raíz del repositorio, ejecuta `npm exec -- openspec <comando>` para usar la versión local de la CLI. Si aún no está instalada, ejecuta `npm ci --include=dev --cache "${TMPDIR:-/tmp}/codex-openspec-npm-cache"`.
- Usa `$openspec-propose` para preparar la propuesta, las especificaciones y las tareas. Usa `$openspec-apply-change` para implementar el cambio y `$openspec-archive-change` para cerrarlo.
