---
tags: [proyecto, ptm-novo, wordpress, seo, marketing]
estado: En configuración
ruta: Remoto (grupoptm.com / Hostinger)
---

# grupoptm.com — Sitio Marketing PTM Novo

> Sitio WordPress de marketing y SEO para el ecosistema PTM Novo.
> URL: grupoptm.com

## Rol en el ecosistema

grupoptm.com atrae tráfico orgánico y dirige al usuario a la app.
Cada landing termina con CTA → [[PTM Novo]] en Railway.

**NO** es donde el cliente actúa — la app PTM Novo en Railway es el destino final.

> [!important] Cambio de estrategia 2026-06-19 — Sitio 100% legítimo (Meta Andromeda)
> grupoptm.com pasa a ser un sitio **100% legítimo de telemedicina**, **SIN funnel ni enlaces públicos a peptidosysuplementos.mx (PYS)**, para correr campañas de Meta sin penalización.
> - El sitio público **nunca** enlaza a la tienda.
> - La derivación a PYS (WooCommerce) la hace el **médico durante la consulta**.
> - CTAs apuntan a **evaluación/consulta** en la app, no a "comprar".
> - **Fase 2:** agregar Terapia de Reemplazo Hormonal (RHT).
> Runbook: [[grupoptm.com — Legitimización (retiro funnel PYS)]]

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
- [x] ~~Activar cross-links PTM ↔ PYS~~ → **revertido**: retirar funnel PYS (Meta) — ver runbook

## Cross-linking PTM ↔ PYS — ⛔ RETIRADO (2026-06-19)

> Estrategia anterior (revertida): grupoptm.com → PYS con links a productos.
> **Ahora:** grupoptm.com **NO** enlaza a PYS. La derivación a la tienda la hace el médico en consulta.
> - PYS → grupoptm.com: el sentido PYS→PTM (consulta médica) **sí puede conservarse** (no afecta a Meta en grupoptm.com).
> - Ver runbook: [[grupoptm.com — Legitimización (retiro funnel PYS)]]

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

## Sesión 2026-06-19 — Legitimización para Meta (retiro funnel PYS)
Decisión estratégica: grupoptm.com será **100% legítimo de telemedicina**, sin funnel ni enlaces públicos a peptidosysuplementos.mx, para correr Meta Andromeda sin penalización. La derivación a la tienda la hará el médico en consulta. RHT queda como Fase 2.
- Creado runbook completo: [[grupoptm.com — Legitimización (retiro funnel PYS)]]
- Landing `peptidos-rendimiento-recuperacion-mexico` (ID 42887) limpiada como plantilla de referencia: 8 referencias PYS retiradas (bloque comercial, botón CTA, xlinks, links a producto, FAQ schema, FAQ "¿dónde encuentro productos?", nota interna) → **0 menciones**. Guardada en `05 - grupoptm legit/`.
- Pendiente (requiere WP/Railway): aplicar el runbook a las 16 páginas + blogs, desactivar cross-linking en `ecommerce-agent` y enlaces a PYS en `agente-blogs`, limpiar menú/footer/sitemap.