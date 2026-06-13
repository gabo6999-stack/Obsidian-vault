---
tags: [sesion, raditech, ecommerce-agent, 2026-06]
fecha: 2026-06-13 13:17
proyecto: Raditech SEO
---

# Sesión 2026-06-13 — Raditech SEO Redirects

## Qué se hizo
- Confirmado que Rank Math REST API (updateRedirection) crea entradas en DB pero NO las ejecuta en v1.0.271.1
- Descubierto que los 2 redirects que funcionaban (pacs, teleradiologia) usan X-Redirect-By: WordPress via _wp_old_slug mecanismo nativo
- Configurado _wp_old_slug en 8 páginas destino via JWT REST API (todos respondieron exitosamente)
- Intentado purge de LiteSpeed Cache via GET con nonce (retornó 200, efecto no confirmado)
- Probado y descartado: WPCode admin-ajax (400), WP File Manager (403), objectID 0 en RM (400)

## Decisiones técnicas
- **_wp_old_slug via REST API**: Mecanismo nativo de WordPress que sí funciona. Se escribe via POST a wp-json/wp/v2/pages/{id} con meta: {_wp_old_slug: 'slug-antiguo'}. Elegido sobre Rank Math porque es el mismo mecanismo que usan los redirects que ya funcionan.
- **Cookie + X-WP-Nonce**: Para endpoints REST de Rank Math que requieren autenticación por cookie (no JWT bearer). Nonce se obtiene via heartbeat admin-ajax.
- **Purge LiteSpeed**: LiteSpeed cachea 404s, bloqueando que WordPress procese la petición. El purge es prerequisito para que _wp_old_slug funcione en URLs que ya estaban cacheadas como 404.

## Problemas encontrados
- LiteSpeed Cache intercepta antes de que WordPress procese — los 7 redirects nuevos siguen como 404 porque LiteSpeed sirvió la versión cacheada
- La URL teleradiologia redirige al destino INCORRECTO (teleradiologia-alta-especialidad en vez de servicio-teleradiologia) — otro page tiene ese _wp_old_slug
- WPCode: los nombres de action para admin-ajax son desconocidos (no están en el HTML, la UI usa React)
- Rank Math 'security' nonce para admin-ajax tampoco funcionó con las acciones probadas

## Próximos pasos
- [ ] Verificar los 8 redirects — si siguen 404, pedir purge manual de LiteSpeed al usuario
- [ ] Corregir teleradiologia — encontrar qué page tiene _wp_old_slug=teleradiologia y sobrescribirlo con el correcto
- [ ] Correr auditoría SEO completa con graficas (fue solicitada, no completada aún)
- [ ] Revisar 2 imágenes sin alt en homepage (son widgets Elementor, no en media library)
