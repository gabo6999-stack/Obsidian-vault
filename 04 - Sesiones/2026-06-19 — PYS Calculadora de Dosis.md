---
tags: [sesion, pys, wordpress, elementor, calculadora, frontend, 2026-06]
fecha: 2026-06-19
proyecto: PYS Ecommerce
---

# Sesión 2026-06-19 — PYS: Calculadora de Dosis de Péptidos

Proyectos: [[PYS Ecommerce]] · [[Ecommerce Agent]]

## Contexto
Se construyó y publicó en **peptidosysuplementos.mx** una **Calculadora de Dosis de Péptidos** interactiva, replicando la UX de `particlepeptides.com/en/content/48-peptide-calculator` pero en español y con el branding de PYS. El diseño va SOLO en Elementor (regla del stack); por API solo se tocan title/meta/slug/rank_math.

## Qué se hizo
- **Página creada y PUBLICADA:** `https://peptidosysuplementos.mx/calculadora-de-dosis-de-peptidos/` (post **2040**, slug `calculadora-de-dosis-de-peptidos`).
- **Calculadora en widget HTML de Elementor** (autocontenida, CSS+JS namespaced `.pys-calc`, sin dependencias externas). Fuente en el repo `ecommerce-agent` rama `claude/practical-edison-3qhcvw`: `calculadora_dosis/calculadora_dosis_pys_v3.html`.
- **Réplica de Particle Peptides con ilustraciones SVG propias** (sin fotos/copyright):
  - Columna izq: selector de jeringa **0.3 / 0.5 / 1.0 ml** con jeringa dibujada + su escala (30/50/100 UI).
  - Columna der: viales dibujados (ámbar = péptido, transparente = agua) + pills.
  - Pills: vial 5/10/15 mg · agua 1/2/3/5 ml · dosis con **toggle mcg/mg** (mcg→50/100/250/500, mg→1/2.5/5/10) · "Otro".
  - Resultado: **regla horizontal numerada con marcador azul** ("jala la jeringa hasta X unidades") + tarjetas (concentración, volumen a extraer, dosis por vial) + advertencia si la dosis no cabe + acordeón de reconstitución + aviso médico + CTA.
- **Botón "Calculadora" en el menú del header**, entre Performance y Blog.
- **Fondo de la página igualado** al resto del sitio (estaba blanco) y **título de la página en magenta** (#ff007e).

## Decisiones / hallazgos clave
- **El menú del header NO es un menú de WordPress.** El header es el template de Elementor **#1099 ("Header PYS")**; el nav es un widget HTML (`data-id c442c6a`) con `<nav class="pys-nav">` y enlaces `<a>` directos. El "Menú principal" de WP (etiquetas SEO largas) NO se usa en el header. → Cambios de navegación se hacen editando el widget c442c6a del template 1099.
- **Fondo:** la página salía con fondo claro (`...biohacking.png`, default del kit Elementor) mientras el resto del sitio usa body con `uploads/2026/05/background-general.png` (oscuro). Se igualó con regla `body.page-id-2040{background-image:url(.../background-general.png)!important}` dentro del `<style>` del widget.
- **Título magenta:** el H1 `body.page-id-2040 h1.entry-title` se puso en **#ff007e** (= rgb 255,0,126, el color del chatbot del sitio) porque en gris quedaba invisible sobre el fondo oscuro.

## Técnicas (reutilizables) 🔧
- **Insertar/editar widgets de Elementor por código** (el drag&drop con la herramienta de browser NO engancha el sortable): usar la API interna
  `$e.run('document/elements/create', {container, model:{elType:'widget', widgetType:'html', settings:{html}}})` para crear, y
  `$e.run('document/elements/settings', {container, settings:{html}, options:{external:true}})` para editar.
- **Inyectar HTML grande (~20KB)** en el widget: dividir el base64 en trozos de ~7000 chars, acumular en `window.__pys` verificando **len + checksum (suma de charCodes)** por trozo, luego decodificar UTF-8 (`atob`→`Uint8Array`→`TextDecoder`).
- **Guardar:** el `Ctrl+S` por teclado a veces NO persiste en Elementor (queda `isDirty:true` aunque el botón parezca atenuado). Guardar con `await $e.run('document/save/default')` y verificar `elementor.documents.getCurrent().editor.isChanged`.
- **LiteSpeed Cache:** purga la URL al guardar el post; para ver cambios al instante añadir `?nc=<n>` a la URL (fuerza cache MISS).

## Pendientes
- [ ] Revisar el destino del botón CTA interno de la calculadora (`/contacto/`) — confirmar que esa página existe o apuntarlo a WhatsApp / página real.

## Referencias
- Fuente: `ecommerce-agent/calculadora_dosis/calculadora_dosis_pys_v3.html` (rama `claude/practical-edison-3qhcvw`)
- Página: https://peptidosysuplementos.mx/calculadora-de-dosis-de-peptidos/
- Relacionados: [[PYS Ecommerce]] · [[Ecommerce Agent]]
