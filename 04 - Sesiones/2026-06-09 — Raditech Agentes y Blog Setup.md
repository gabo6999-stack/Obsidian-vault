# 2026-06-09 — Raditech: Agentes, Blog Setup y Fixes

## Lo que se hizo

### Agente SEO (ecommerce-agent — 3743c)
- Agregadas 11 herramientas para raditech.mx: 8 WP CRUD + 3 GSC
- Endpoint `/optimize-raditech-blog`: links internos, externos (RSNA/ACR/HIMSS/PubMed), FAQ, CTA
- FAQ Schema JSON-LD automático en los 3 optimizadores (PYS, PTM, Raditech)
- `get_raditech_post_content` ahora devuelve meta + categories + featured_media (context=edit)
- `update_raditech_post` expone campo `categories`
- `get_or_create_raditech_category` tool agregado

### JWT Auth — raditech.mx
- Plugin JWT Auth instalado y activo
- wp-config.php: `JWT_AUTH_SECRET_KEY` y `JWT_AUTH_CORS_ENABLE` agregados (línea 103)

### agente-blogs-raditech (2809e)
- Fork de agente-blogs para contenido B2B PACS/teleradiología
- Repo: gabo6999-stack/agente-blogs-raditech
- Fix Pydantic v2: `Optional[str]` en campos nullables
- Fix PORT Railway: `int(os.getenv("PORT", 8000))`
- Auto-categoría: Claude detecta tema y elige entre 5 categorías en el JSON
- Fix rank_math: PATCH separado post-creación (Rank Math no acepta meta en POST inicial)
- Llama a `/optimize-raditech-blog` después de publicar (no `/optimize-blog`)

### Categorías creadas en raditech.mx
| ID | Nombre |
|----|--------|
| 27 | Teleradiología |
| 28 | Diagnóstico por Imagen |
| 29 | Gestión Hospitalaria |
| 30 | Tecnología Médica |
| 31 | Medicina General |

### Posts organizados
| ID | Título | Categoría |
|----|--------|-----------|
| 819 | Teleradiología en México Guía 2026 | Teleradiología (27) |
| 622 | Teleradiografía de Tórax | Teleradiología (27) |
| 667 | Eco 4D | Diagnóstico por Imagen (28) |
| 636 | Gestión Hospitalaria Digitalización | Gestión Hospitalaria (29) |
| 830 | VIRA PACS Solución Integral | Diagnóstico por Imagen (28) |
| 842 | IA en Radiología Diagnóstica 2026 | Tecnología Médica (30) |
| 742 | Analgésicos Musculares | Medicina General (31) |
| 706 | Hipotensión Ortostática | Medicina General (31) |

### CSS fix
- Títulos del blog en negro `#1a1a1a` vía Apariencia → Personalizar → CSS adicional
- Hover en naranja para mantener identidad de marca

### Stop Hook corregido
- `settings.json`: PowerShell escribe en daily note de Obsidian al final de cada respuesta

## Pendientes
- Desplegar `social-video-raditech` en Railway
- Agregar Service Account GSC a raditech.mx como Owner
- Solicitar indexación Google de nuevos posts
- Verificar rank_math fix publicando nuevo blog
