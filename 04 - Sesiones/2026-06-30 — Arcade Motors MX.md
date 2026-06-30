---
tags: [sesion, ArcadeMotorsMX, 2026-06]
fecha: 2026-06-30 01:07
proyecto: Arcade Motors MX
---

# Sesión 2026-06-30 — Arcade Motors MX

Proyectos: [[Arcade Motors MX]]

## Qué se hizo
- Recuperé la BD autosalv borrada (restaurada del backup diario de hPanel del 06-27). Confirmé que NO fue hackeo (SSH, archivos, logins limpios).
- Zoom/lupa en el lightbox de los anuncios; tipografía del blog ajustada a VT323.
- SEO Tier 1: robots.txt, sitemap dinámico, schema Vehicle/Offer, canonical, Google Search Console (verificado + sitemap enviado).
- SEO Tier 2: páginas de categoría y marca optimizadas, IndexNow (Bing), URLs limpias id-based para auto, categoria y marca.
- Blog (Tier 3): base de blog + artículos, 3 guías con FAQPage schema, enlaces contextuales guía-anuncio (1 guía por anuncio, estable por id).
- Automatización: endpoint blog-publish + adapté el Agente de Blogs (sitio arcademotors, prompt de autos) + ARCADE_API_KEY en Railway. Ya publicó su 1a guía sola (REPUVE).
- Fix: el JSON-LD grande deslogueaba en los anuncios (session_start tarde); se arregló arrancando la sesión al inicio de header.php.

## Decisiones técnicas
- Restaurar del backup 06-27 (el del 06-28 ya no tenía la BD).
- URLs limpias id-based a prueba de choques de slug; slug malo o viejo redirige 301.
- Agente de Blogs vía endpoint propio (Arcade es PHP, no WordPress); el notify_seo_agent config-driven se auto-salta para Arcade.
- 1 guía por anuncio estable por id (mejor SEO que aleatoria por-carga).

## Problemas encontrados
- BD autosalv borrada (recuperada del backup; no fue hackeo).
- Bug de collation en el endpoint del blog; se calculó published_at en PHP.
- Bug de login en anuncios (session_start tarde); sesión iniciada antes de cualquier salida.
- Falsa alarma del FX del home (estático): era estado del navegador, se arregló reiniciando Chrome (código intacto).

## Próximos pasos
- [ ] SEO Tier 3 autoridad: backlinks, Google Business Profile, redes.
- [ ] Imagen de portada en los posts del blog.
- [ ] Igualar fuente VT323 en las páginas de categoría y marca.
- [ ] Secuencia avanzada de SEO (review entre generar y publicar).
- [ ] Crear correo de contacto; activar 2FA en hPanel.
