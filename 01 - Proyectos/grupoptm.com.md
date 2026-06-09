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
- [ ] Activar cross-links PTM ↔ PYS

## Cross-linking PTM ↔ PYS

- grupoptm.com → PYS: en landings de péptidos, link a productos con receta
- PYS → grupoptm.com: en landings de productos, link a consulta médica PTM Novo
- Manejable via [[Agente SEO GSC]] (ya tiene herramientas para ambos sitios)

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