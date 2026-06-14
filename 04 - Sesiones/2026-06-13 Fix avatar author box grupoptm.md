Sesion 2026-06-13 23:50 - Fix avatar author box grupoptm

Proyectos: [[MOC SEO]] [[grupoptm.com]]

## Que se hizo
- Fix bug avatar en plugin PTM Custom: esc_js() cambiado a wp_json_encode()
- Plugin actualizado a v2.4 - avatar ahora renderiza como imagen correctamente
- Cache LiteSpeed purgado y verificado en post de prueba
- Pestaña Chrome MCP reconnectada (tab group habia expirado)

## Decisiones tecnicas
- wp_json_encode() en lugar de esc_js() porque esc_js escapa comillas dobles con backslash, rompiendo innerHTML

## Problemas encontrados
- Tab group de Chrome MCP habia expirado - resuelto con tabs_context_mcp createIfEmpty:true

## Proximos pasos
- [ ] Eliminar scripts temp del root WP: ptm-diag.php, ptm-fix2.php, ptm-dump.php
- [ ] Quitar firma Radiologo/PACS-RIS en posts de Raditech
- [ ] Desactivar noindex en /author/GavitoA (Rank Math)