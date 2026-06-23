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

## Bitácora — 2026-06-23 · Sesión SEO grande (PYS + Raditech)

Operando el agente SEO (GSC vía Railway, edición vía navegador wp-admin). Cubre 3 sitios.

### peptidosysuplementos.mx (PYS)
- **Análisis GSC de caídas** (método: searchAnalytics API 2 períodos + URL Inspection, credenciales en Railway proyecto Agente-SEO). Causa raíz: reestructuración de URLs en curso, NO una página.
- 🔴 **Bomba de tiempo desactivada**: `/precio-de-retatruida-en-mexico/` (página #1, 7 de 13 clicks) hacía 301 al PRODUCTO. Cambiado a redirigir al artículo de contenido **1647** `/retatrutide-precio-guia-completa-costos-beneficios/` (plugin **Redirection**, reglas id 2 e id 3). Consolidada la canibalización de "retatrutida".
- **Titles/meta optimizados** (vía Rank Math `rankmath/v1/updateMeta`): art. 1647 ("Retatrutida en México: Precio, Beneficios y Guía 2026") y **home page 21** ("Comprar Péptidos en México | Alta Pureza y Trazabilidad | PyS") — antes el title de la home no mencionaba México/comprar aunque el H1 sí.
- Indexación solicitada (manual, usuario). Investigación home/productos: están sanas, la "caída" de /productos/ es ruido estadístico (long-tail geográfico marginal). tirzepatida: sin mismatch real (página correcta ya pos 1.4).
- **Conclusión**: on-page de PYS esencialmente completo; el cuello de botella es autoridad/backlinks (sitio nuevo + ~70 backlinks perdidos en la desvinculación).

### raditech.mx
- **GEO (tarea 4.2)**: reescritos primeros párrafos a "respuesta directa" (AI Overviews) en 4 blogs del nicho (#819, #842, #941, #636). Edición vía REST wp/v2/posts con nonce (posts son HTML clásico, no Gutenberg).
- **Off-topic**: #706 hipotensión (11,403 imp) y #742 analgésicos (5,675 imp) son las páginas de más tráfico PERO es **tráfico vanidad** (long-tail salud-consumidor, audiencia equivocada, sin AdSense) → NO eliminar, no invertir. Solo 9.9% de impresiones del sitio son nicho B2B real.
- **E-E-A-T confirmado**: posts firmados Dr. Antonio Gavito Hernández - Médico Radiólogo (schema author + meta + byline).
- **Mapeo de nicho**: NO hay canibalización (slugs viejos ya consolidados con 301 vía snippet WPCode **932**). 🔴→✅ **Fix crítico**: `/teleradiologia-de-alta-especialidad/` daba **404** siendo la URL de mejor rendimiento de nicho ("pacs ptm", "teleradiologia mexico"); agregada al mapa del snippet 932 → 301 a /teleradiologia-alta-especialidad/. Gotcha: WPCode requiere 2º Update + purgar LiteSpeed (cachea el 404).
- Pendiente: CTR de nicho ~1% (mejorar titles/metas landings core); arrastre de marca "pacs ptm" → reforzar identidad Raditech.

### grupoptm.com
- Pendiente detectado: la página `/contacto/` muestra datos de Raditech (email info@raditech.mx, dirección Venustiano Carranza, texto teleradiología).

### Técnicas/gotchas nuevos de la sesión
- **Rank Math meta** no está en REST wp/v2 estándar → usar `POST {root}rankmath/v1/updateMeta` con X-WP-Nonce=window.rankMath.restNonce (en el editor del post). Setear rank_math_title literal omite el template de sitename.
- **Theme File Editor** (grupoptm): NO persiste con form.submit() → usar `wp.ajax.post('edit-theme-plugin-file', {nonce,file,theme,newcontent})`.
- **Indexing API** (request-indexing) bloqueada: el SA gsc-indexing@tanus-498105 no es propietario en las propiedades GSC → 403. Solicitar indexación es manual en la UI con la cuenta dueña.

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

## Bitácora — 2026-06-22 · Schema MedicalOrganization (grupoptm + Raditech)

**Qué se hizo**
- **grupoptm.com**: el `MedicalOrganization` que vivía en `functions.php` (Astra) se **migró al plugin hefo** (`options[head]`, site-wide) y se **enriqueció** con `@id`, `logo` (300x300) e `image`. Quitada la función `grupoptm_organization_schema` de functions.php; conservada `grupoptm_page_schema_and_og` (MedicalWebPage+OG). Sin contacto/dirección/sameAs por decisión del usuario.
- **raditech.mx**: creado `MedicalOrganization` nuevo vía **snippet WPCode HTML id 948** (site_wide_header). Adaptado a teleradiología: `medicalSpecialty: Radiology`, email/tel/address (Tamaulipas 150A, Hipódromo Condesa, CDMX 06140), servicios Teleradiología 24/7 + Alta Especialidad.
- Verificado en vivo: 1 solo MedicalOrganization por sitio, JSON válido.

**Decisiones técnicas / gotchas**
- 🔴 **Theme File Editor de grupoptm NO guarda con `form.submit()`** (WP moderno usa AJAX). Usar `wp.ajax.post('edit-theme-plugin-file', {nonce, file, theme:'astra', newcontent})`. Verificar releyendo el editor fresh (lee disco). Corrige el gotcha viejo del submit.
- Para schema en `<head>`: grupoptm → plugin **hefo**; raditech → **WPCode**. Ambos sobreviven a updates del tema (mejor que functions.php).

**Raditech — BreadcrumbList 3 niveles**
- Las 9 landings de producto ya tenían BreadcrumbList de Rank Math (2 niveles, "Home > título con | Raditech", sin migas visibles en Elementor).
- Solución sin duplicar: **snippet WPCode PHP id 949** con filtro `rank_math/json_ld` que reescribe el itemListElement a 3 niveles `Inicio > Productos > [nombre corto]`. Verificado en las 9 (canónica + cache-bust). Caché LiteSpeed purgada.
- Técnica reutilizable: **modificar el JSON-LD de Rank Math vía su filtro `rank_math/json_ld`** en vez de crear schema nuevo → evita duplicados y mantiene consistencia.

**Pendiente**
- 🐛 La página `/contacto/` de **grupoptm.com** muestra datos de **Raditech** (email info@raditech.mx, dirección Venustiano Carranza, texto teleradiología). Corregir con datos reales de PTM.
- Búsqueda Console del agente SEO ya funciona para PYS/Raditech/PTM.

## MOCs de proyecto
[[MOC - Raditech]] | [[MOC - Ecosistema PTM-PYS]]

## Proyectos
[[Raditech]] | [[PYS Ecommerce]] | [[PTM Novo]] | [[Agente SEO GSC]]
