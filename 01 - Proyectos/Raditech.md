---
tags: [proyecto, raditech, seo, wordpress]
estado: Activo
sitio: https://raditech.mx
agente-seo: https://web-production-3743c.up.railway.app
agente-blogs: https://web-production-2809e.up.railway.app
---

# Raditech

> Estado: 🟢 Activo
> Nicho B2B: Sistemas PACS/RIS, Teleradiología, Monitores médicos para hospitales/centros de diagnóstico

## Stack
- WordPress en Hostinger
- Elementor + tema Astra
- Rank Math SEO
- Agente SEO (ecommerce-agent) + Agente de Blogs (agente-blogs-raditech)

## Páginas clave (landings de servicios)

| ID | Slug | Estado SEO |
|----|------|------------|
| 883 | /sistema-pacs-ris/ | ✅ Optimizado |
| 874 | /teleradiologia-alta-especialidad/ | ✅ Optimizado |
| 890 | /sistema-his-medsi/ | ✅ Optimizado |
| 879 | /monitores-medicos-radiologia/ | ⏳ Pendiente |
| 871 | /servicio-teleradiologia/ | ⏳ Pendiente |
| 881 | /portal-x-card/ | ⏳ Pendiente |
| 894 | /teleradiologia-tomografia-cardiaca/ | ⏳ Pendiente SEO (contenido OK) |
| 892 | /teleradiologia-resonancia-cardiovascular/ | ⏳ Pendiente |
| 10 | / (home) | ⏳ Pendiente — remover referencia Grupo PTM |

## Flujo correcto para editar landings con HTML grande (>30KB)

El chat del agente SEO timoueó con HTML de 30KB+. Flujo alternativo validado:
1. `GET https://raditech.mx/wp-json/wp/v2/pages/{id}` — API pública sin auth
2. Editar HTML localmente en PowerShell
3. `POST https://web-production-3743c.up.railway.app/raditech-page-update`
4. Verificar con la misma API (no WebFetch — caché 15 min)

---

## Sesión 2026-06-10
Eliminadas 3 secciones de la landing 894 (teleradiología tomografía cardiaca): CTA "Sin costo de instalación", y 2 FAQs (tiempo de reporte + post-procesamiento 3D). JSON-LD schema también limpiado. HTML: 32,777 → 29,355 chars.
