---
tags: [moc, seo]
actualizado: 2026-06-14
---

# MOC SEO

MOC transversal de técnicas y sesiones SEO aplicadas en todos los proyectos.
Raditech, PYS y PTM comparten stack SEO similar (WordPress + Rank Math).

> Las técnicas SEO son reutilizables entre sitios, por eso este MOC no pertenece a un solo proyecto. Es el índice central; al iniciar cualquier trabajo de SEO, revisar aquí primero.

## Sesiones — Raditech
- [[2026-06-09 - Raditech GSC]] — Google Search Console separado con token OAuth independiente
- [[2026-06-09 — Raditech Agentes y Blog Setup]]
- [[2026-06-10 — Raditech Landing Tomografia Cardiaca]]
- [[2026-06-13 - Raditech SEO Redirects]] — 8/8 redirects 301 vía slug cycling
- [[2026-06-13 - Raditech SEO Score 100]]

## Sesiones — PTM / PYS
- [[2026-06-08 — Agente SEO PTM Cross-linking]]
- [[2026-06-08 — Agente Blogs PTM]]

## Sesiones — grupoptm.com
- [[2026-06-13 SEO grupoptm Raditech]]
- [[2026-06-13 Fix avatar author box grupoptm]]
- 2026-06-14 — Limpieza de scripts temporales (bitácora abajo)

## Técnicas documentadas
- **Slug cycling** para redirects 301 (Rank Math)
- **Cookie + X-WP-Nonce** para Rank Math REST API
- `_wp_old_slug` vía REST API
- **Purge LiteSpeed** vía nonce del admin panel
- **Plugin Editor de WP (CodeMirror):** usar `cmObj.setValue()` (instancia CM), NO `textarea.value`, para que el form envíe el contenido actualizado. La primera edición vía `textarea.value` no funciona: CodeMirror ignora el cambio y el form envía el contenido original.
- **Hook `admin_init` temporal** en un plugin para operaciones de filesystem cuando WP File Manager da error de permisos: agregar el hook vía Plugin Editor, ejecutar una vez y eliminarlo.

## Bitácora — 2026-06-14 · grupoptm.com (limpieza)

**Qué se hizo**
- Eliminados scripts temporales de diagnóstico de `/public_html/`: `ptm-diag.php`, `ptm-fix2.php`, `ptm-dump.php`
- Técnica usada: hook `admin_init` temporal en `ptm-custom.php` vía Plugin Editor, ejecutado una vez y luego eliminado
- `ptm-custom.php` quedó limpio en **v2.4** (2377 chars, sin código temporal)
- Confirmado que la firma en posts de Raditech ('Médico Radiólogo / PACS-RIS') se queda — usuario decidió mantenerla

**Decisiones técnicas**
- Plugin Editor de WP usa CodeMirror: hay que usar `cmObj.setValue()` (instancia CM) en lugar de `textarea.value` para que el form envíe el contenido actualizado
- WP File Manager da error de permisos al navegar directo — workaround: usar Plugin Editor + hook temporal para operaciones de filesystem
- Firma Raditech: origen en `agente-blogs/prompts/system.py` — usuario decidió NO tocarla

**Problemas encontrados**
- Primera edición vía `textarea.value` no funcionó: CodeMirror ignoró el cambio y el form envió el contenido original
- WP File Manager (`/wp-admin/admin.php?page=wp-file-manager`) da 'No tienes permisos' al navegar directo desde nueva sesión
- elFinder connector vía `admin-ajax.php` no respondió sin nonce apropiado

**Estado / próximos pasos**
- Firma 'Médico Radiólogo / PACS-RIS' en Raditech: CANCELADA por usuario — se mantiene
- No hay pendientes activos en grupoptm.com — plugin PTM Custom v2.4 completo y limpio

## MOCs de proyecto
[[MOC - Raditech]] | [[MOC - Ecosistema PTM-PYS]]

## Proyectos
[[Raditech]] | [[PYS Ecommerce]] | [[PTM Novo]] | [[Agente SEO GSC]]
