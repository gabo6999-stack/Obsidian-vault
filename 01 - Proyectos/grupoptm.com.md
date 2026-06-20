---
tags: [proyecto, ptm-novo, wordpress, seo, marketing]
estado: En configuración
ruta: Remoto (grupoptm.com / Hostinger)
---

# grupoptm.com — Sitio Marketing PTM Novo

> Sitio WordPress de marketing y SEO de **PTM Novo** (telemedicina).
> URL: grupoptm.com
> Nota 2026-06-19: PTM y [[PYS Ecommerce]] son **empresas independientes** — sin cross-linking ni embudo entre ambas. Ver [[Ecosistema PTM-PYS]].

## Rol en el ecosistema

grupoptm.com atrae tráfico orgánico y dirige al usuario a la app.
Cada landing termina con CTA → [[PTM Novo]] en Railway.

**NO** es donde el cliente actúa — la app PTM Novo en Railway es el destino final.

## Stack

| Componente | Tecnología |
|-----------|-----------|
| Hosting | Hostinger |
| CMS | WordPress |
| Page Builder | Elementor Free + tema Astra / Hello Elementor |
| SEO | Rank Math (igual que PYS) |
| Auth agente | JWT Authentication for WP REST API |

## Las 5 Landings

| # | Slug | Keyword objetivo |
|---|------|-----------------|
| 1 | `/` | telemedicina péptidos México |
| 2 | `/perdida-de-peso` | semaglutide México |
| 3 | `/longevidad` | péptidos longevidad México |
| 4 | `/como-funciona` | consulta médica online péptidos |
| 5 | `/medicos` | médico especialista péptidos |

## Checklist de activación

- [x] Instalar JWT Auth for WP-API
- [x] Editar wp-config.php: JWT_AUTH_SECRET_KEY + JWT_AUTH_CORS_ENABLE=true
- [x] Instalar Rank Math
- [x] Crear Application Password en WP admin
- [x] Agregar en Railway (ecommerce-agent): PTM_WP_USER, PTM_WP_PASSWORD, PTM_URL=https://grupoptm.com
- [ ] Crear las 5 páginas via agente (create_ptm_page ya disponible en Railway)
- [ ] Verificar con get_ptm_pages() desde el agente SEO
- [ ] Diseñar cada landing en Elementor
- [ ] Optimizar SEO de las 5 landings via agente
- [x] ~~Activar cross-links PTM ↔ PYS~~ ❌ CANCELADO 2026-06-19 — PTM y PYS son independientes; NO debe haber cross-linking entre ambas (ver nota de separación)

## Cross-linking PTM ↔ PYS — ❌ DEPRECADO (2026-06-19)

> PTM y PYS son **empresas independientes**. El cross-linking que embudaba paciente PTM → producto PYS (y viceversa) **se elimina** — es justo la cadena de venta que rompe la defensa de "PTM es solo plataforma".
>
> **Pendiente operativo:** quitar las reglas de CTA bidireccional PYS ↔ PTM del system prompt del agente [[Ecommerce Agent]] y los links cruzados en las landings.

Modelo anterior (ya no aplicar):
- ~~grupoptm.com → PYS: link a productos con receta~~
- ~~PYS → grupoptm.com: link a consulta médica PTM Novo~~

## Estado (2026-06-08)

- Dominio grupoptm.com ✅
- WordPress + Hostinger instalado ✅
- Plugins JWT Auth + Rank Math instalados ✅
- Variables Railway configuradas: PTM_WP_USER, PTM_WP_PASSWORD, PTM_URL ✅
- Código cross-linking en ecommerce-agent mergeado a main ✅
- Pendiente: crear las 5 páginas y diseñar en Elementor

---

## Sesión 2026-06-08 (Activación)
Plugins instalados (JWT Auth, Rank Math), variables Railway configuradas, conexión verificada con get_ptm_pages(). Error de dominio .mx corregido a .com. Pendiente: crear las 5 páginas en WP.


---

## Sesión 2026-06-09
16 páginas totales confirmadas (12 landings SEO + 4 generales). Landing tirzepatida-mexico creada con HTML completo (mismo patrón que semaglutida). WhatsApp eliminado de todas las landings. Aviso de privacidad reescrito desde cero (LFPDPPP, telemedicina, 10 secciones). Nuevas herramientas en agente SEO: create_ptm_page y delete_ptm_page. Pendiente: razón social en aviso de privacidad, diseño Elementor, SEO y cross-links.

**Actualización 2026-06-09 (cierre):** Aviso de privacidad completo con razón social y domicilio fiscal. Cross-links PTM ↔ PYS activados. Pendiente: diseño Elementor y SEO de las 12 landings.


---

## Sesion 2026-06-13 (23:50)
Fix bug avatar en PTM Custom v2.4: esc_js() cambiado a wp_json_encode(). Author box completamente funcional en posts.


---

## Sesión 2026-06-14
Eliminados scripts temporales de diagnóstico (ptm-diag.php, ptm-fix2.php, ptm-dump.php) de /public_html/ via hook admin_init temporal en ptm-custom.php. Plugin limpio en v2.4. Firma Raditech se mantiene por decisión del usuario.


---

## Sesión 2026-06-19 — Modelo de Monetización
Definido el modelo para legitimar PTM como portal de telemedicina **fuera de la cadena de venta**: consulta $1,500 ($1,000 médico / $500 comisión fija PTM), pago con retención liberado al completar la consulta, sin reembolso por no-show del paciente, asignación automática de médico, Stripe Connect. PTM no factura al paciente (lo hace el médico). ⚠️ Contradice el modelo viejo de [[PTM Novo]] ($500, producto vía PYS) — reconciliar. Pendiente crítico: dictamen de abogado COFEPRIS (péptidos no registrados). Detalle completo en [[2026-06-19 — grupoptm Modelo Monetización]] y en `ecommerce-agent/MODELO_MONETIZACION_PTM.md` (commit 138b7f9).