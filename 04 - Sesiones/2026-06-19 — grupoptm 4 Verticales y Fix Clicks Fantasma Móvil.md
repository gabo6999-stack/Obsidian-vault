---
tags: [sesion, grupoptm, PTM-Novo, 2026-06]
fecha: 2026-06-19
proyecto: grupoptm.com
---

# Sesión 2026-06-19 — grupoptm.com: 4 Verticales + Fix Clicks Fantasma Móvil

## Qué se hizo

### Ampliación a 4 verticales de telemedicina (estilo Medvi)
- El negocio pasó de "solo péptidos" a **4 verticales**: **Pérdida de peso**, **Péptidos y Longevidad**, **Salud para Hombres** (TRH/sexual/hormonal), **Salud para Mujeres** (menopausia/libido/SOP).
- **Home (id 23324) rediseñada**: hero + 4 tarjetas de categoría (con fotos distintas a las secciones) + sección por vertical + cómo funciona + CTA. Backup del contenido viejo en `.home_BACKUP_23324.html`.
- **2 hubs nuevos** (`/salud-para-hombres/` 43096, `/salud-para-mujeres/` 43097) + **4 landings nuevas** enriquecidas (~800-1450 palabras c/u): TRH hombres (43100), Menopausia (43101), Libido femenino (43102), Equilibrio hormonal/SOP (43103).
- **Menú header + footer** actualizado con submenús (dropdowns) a los 4 verticales. SEO Rank Math en 7 páginas + schema MedicalOrganization ampliado. Fotos Unsplash en secciones y tarjetas.

### Correcciones de contenido
- **TRT → TRH** en todo el sitio (es "Terapia de Reemplazo Hormonal", TRT es inglés). 7 páginas + schema. Slug cambiado a `terapia-reemplazo-hormonal-hombres-mexico` (301 auto).
- Desvinculación PYS reforzada (ver [[2026-06-19 — grupoptm Modelo Monetización]]): sameAs, footer, bug "farmacia→Tienda en línea".

### 🐛 Fix CLICKS FANTASMA en móvil (lo que más costó — varias horas)
Síntoma: en el celular (no en PC), al tocar zonas "vacías" se navegaba a páginas al azar (salud-para-mujeres, TRH-hombres…). **Dos causas combinadas y camufladas:**
1. **Menú oculto del tema Astra**: Astra activa en móvil una clase `.ast-header-break-point` en el `<body>` con reglas (`.ast-header-break-point .main-navigation{display:inline-block}`) que **re-mostraban su menú automático** (page-list de TODAS las páginas, `#ast-mobile-site-navigation` / `.main-navigation`), invisible pero **clickeable** encima del contenido. Mis intentos de ocultarlo perdían el cascade por **menor especificidad**.
2. **La caché de LiteSpeed NUNCA se purgaba** desde el navegador automatizado: el botón "Vaciar la caché entera" dispara un `confirm()` que el agente no puede aceptar → la purga no se ejecutaba. Resultado: todos los fixes correctos quedaban invisibles tras la versión vieja cacheada.

## Decisiones técnicas
- **Fix Astra**: regla con prefijo `html body.ast-header-break-point #masthead, … .main-navigation, … {display:none!important;visibility:hidden!important;pointer-events:none!important}` (mayor especificidad que Astra) en `options[body]` del plugin **hefo**.
- **Fix mi nav custom**: `@media(max-width:900px){.ptmh-nav-wrap{display:none!important}.ptmh-header.open .ptmh-nav-wrap{display:block!important}}` (no depende de `:not(.open)`).
- **Purga REAL**: navegar directo a `wp-admin/admin.php?page=litespeed-toolbox&LSCWP_CTRL=PURGE_EMPTYCACHE&LSCWP_NONCE=<nonce>` (salta el diálogo). El nonce se saca de los `<a>` de purga del Toolbox.
- **Verificación correcta**: pedir la URL **PLANA** sin cache-buster (`cache:"no-store"`) y revisar header `x-litespeed-cache`. Verificar con `?cb=` ENGAÑA (salta caché). Tras purgar: 1er request `miss` (regenera con fix), siguientes `hit` con fix.
- Astra debería tener su header/menú **desactivado en el Customizer** (se usa header custom); pendiente como deuda técnica.

## Problemas encontrados
- El navegador del agente (Claude-in-Chrome) **renderiza a 1280px fijo**; `resize_window` NO cambia el viewport real → imposible probar móvil desde el agente. Se diagnosticó con **2 screenshots del usuario** (zona azul del tap + página destino); el destino reveló que el enlace venía del page-list de Astra.
- Un detector JS de toques inyectado fue **contraproducente** (la caché vieja no lo servía). Removido al final.
- Truco útil: simular el estado móvil de Astra agregando `document.body.classList.add('ast-header-break-point')` en desktop (sus reglas dependen de la clase, no del @media).

## Próximos pasos
- [ ] Desactivar el header/menú de Astra en el Customizer (eliminar la causa de raíz, no solo taparla con CSS).
- [ ] Migrar el código custom (schema, header/footer) de `functions.php` del parent Astra a un **child theme**.
- [ ] Opcional: extender las 4 landings a ~2000 palabras para SEO.
- [ ] Definir qué hacer con `salud-sexual-con-peptidos-mexico` (42983): genérica hombres+mujeres bajo hub de Hombres; el usuario la dejó en pausa.

---

**MOC:** [[MOC - Ecosistema PTM-PYS]]
