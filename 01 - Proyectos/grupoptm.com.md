---
aliases: [grupoptm, GrupoPTM, grupoptm.com marketing]
tags: [proyecto, ptm-novo, wordpress, seo, marketing]
estado: Operando (4 verticales activas)
ruta: Remoto (grupoptm.com / Hostinger)
---

# grupoptm.com — Sitio Marketing PTM Novo

> Sitio WordPress de marketing y SEO de **PTM Novo** (telemedicina).
> URL: grupoptm.com
> Nota 2026-06-19: PTM y [[PYS Ecommerce]] son **empresas independientes** — sin cross-linking ni embudo entre ambas. Ver [[Ecosistema PTM-PYS]].
> **Pivote 2026-06-19:** el negocio pasó de "solo péptidos" a **4 verticales de telemedicina** (estilo Medvi). Ver [[2026-06-19 — grupoptm 4 Verticales y Fix Clicks Fantasma Móvil]].

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

## Las 4 Verticales (estructura actual — 2026-06-19)

El sitio se reorganizó en 4 verticales de telemedicina. Home (id **23324**) con hero + 4 tarjetas de categoría + sección por vertical + cómo funciona + CTA (backup del contenido viejo en `.home_BACKUP_23324.html`).

| Vertical | Hub / Landing | ID |
|----------|---------------|----|
| Pérdida de peso | (landings existentes: semaglutida, tirzepatida…) | — |
| Péptidos y Longevidad | (landings existentes) | — |
| Salud para Hombres (TRH/sexual/hormonal) | hub `/salud-para-hombres/` | 43096 |
|  | landing TRH hombres `terapia-reemplazo-hormonal-hombres-mexico` | 43100 |
| Salud para Mujeres (menopausia/libido/SOP) | hub `/salud-para-mujeres/` | 43097 |
|  | landing Menopausia | 43101 |
|  | landing Libido femenino | 43102 |
|  | landing Equilibrio hormonal / SOP | 43103 |

- **Menú header + footer** con dropdowns a los 4 verticales. SEO Rank Math en 7 páginas + schema `MedicalOrganization` ampliado.
- **TRT → TRH** corregido en todo el sitio (es "Terapia de Reemplazo Hormonal"; TRT es inglés). Slug viejo redirige 301 automático.
- `salud-sexual-con-peptidos-mexico` (42983): genérica hombres+mujeres bajo hub de Hombres — **en pausa** (decidir qué hacer con ella).

### Landings históricas (12 SEO + 4 generales, desde 2026-06-09)
Aún vigentes bajo los verticales: telemedicina péptidos (`/`), pérdida de peso, longevidad, cómo funciona, médicos, semaglutida, tirzepatida, etc.

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


---

## Sesión 2026-06-19 — 4 Verticales + Fix Clicks Fantasma Móvil
Pivote a **4 verticales** (Pérdida de peso, Péptidos/Longevidad, Salud Hombres TRH, Salud Mujeres): Home rediseñada (23324), 2 hubs nuevos (43096, 43097) + 4 landings nuevas (43100-43103), menús con dropdowns, Rank Math + schema ampliado, TRT→TRH en todo el sitio.

🐛 **Fix bug "clicks fantasma" en móvil** (varias horas): dos causas combinadas — (1) el menú oculto del tema **Astra** (`.ast-header-break-point`) se re-mostraba invisible pero **clickeable** encima del contenido; (2) la **caché de LiteSpeed nunca se purgaba** desde el navegador automatizado (el botón dispara `confirm()`). Resuelto con CSS de mayor especificidad (plugin hefo) + purga directa por URL del Toolbox (`LSCWP_CTRL=PURGE_EMPTYCACHE`). Verificación correcta = pedir URL plana sin cache-buster y revisar header `x-litespeed-cache`. Detalle completo en [[2026-06-19 — grupoptm 4 Verticales y Fix Clicks Fantasma Móvil]].

### ⏳ Pendientes (al 2026-06-19)
- [ ] Desactivar el header/menú de **Astra en el Customizer** (raíz del bug de clicks fantasma, no solo taparlo con CSS).
- [ ] Migrar el código custom (schema, header/footer) de `functions.php` del parent Astra a un **child theme**.
- [ ] Opcional: extender las 4 landings nuevas a ~2000 palabras para SEO.
- [ ] Definir qué hacer con `salud-sexual-con-peptidos-mexico` (42983) — en pausa.
- [ ] ⚠️ **Crítico:** dictamen de abogado COFEPRIS (péptidos no registrados) — bloqueante legal.
- [ ] **Reconciliar** el modelo de monetización nuevo ($1,500, médico factura) vs el viejo de [[PTM Novo]] ($500, producto vía PYS).
- [ ] **Desvinculación PTM ↔ PYS:** quitar reglas de CTA bidireccional del system prompt de [[Ecommerce Agent]] y los links cruzados en las landings.