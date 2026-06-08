---
tags: [sesion, obsidian, ptm-novo, pys, autodiag, 2026-06]
fecha: 2026-06-08 01:48
proyecto: Obsidian Vault PTM&PYS
---

# Sesión 2026-06-08 — Obsidian Vault Setup

## Qué se hizo
- Creación de bóveda Obsidian en `C:\Users\gabom\PTM&PYS\` desde cero
- 22 notas de proyectos con contexto migrado de carpetas reales (código, docs existentes)
- Configuración de Daily Notes + template de nota diaria en español
- Restructuración: PTM Novo y PYS unificados en carpeta `PTM Novo & PYS/`
- AutoDiag Pro agregado con visión general + servidor licencias
- Gloria IEM AI agregada y luego eliminada por el usuario
- CLAUDE.md creado para todos los proyectos (global `C:\Users\gabom\` + por proyecto)
- CLAUDE.md de `PTM Novo\ptm-novo\` actualizado con contexto completo del proyecto

## Decisiones técnicas
- Una sola bóveda para todos los proyectos (vs vaults separados) — menos fricción, una sola búsqueda
- Carpeta `PTM Novo & PYS/` unificada — PTM y PYS son ecosistema único (farmacia + clínica)
- Guión simple `-` en nombre de carpeta Gloria (em dash `—` causó problemas en PowerShell/Write)
- CLAUDE.md global en `C:\Users\gabom\CLAUDE.md` para contexto fresco en cualquier sesión

## Problemas encontrados
- Carpeta `Gloria — IEM AI` no se creó correctamente (em dash en nombre de carpeta)
- `UX PyQt6 — Patrones` apareció como nodo huérfano en root del vault (link no resuelto)

## Próximos pasos
- [ ] Deploy servidor licencias AutoDiag en Railway
- [ ] Prueba pago real Mercado Pago en PTM Novo producción
- [ ] WhatsApp API para confirmaciones post-pago en PTM Novo
- [ ] Renovar Google refresh token (WhatsApp bot Tanus)
- [ ] OpenAI API key para Whisper (WhatsApp bot Tanus)
