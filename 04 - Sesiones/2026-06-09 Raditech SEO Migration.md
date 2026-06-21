# Raditech SEO Migration — Sesión 2026-06-09

## Estado al cerrar sesión

cambios hechos en [[Raditech]]

### ✅ Completado hoy

#### Migración de páginas (8/8 publicadas)
| ID | URL | Notas |
|----|-----|-------|
| 871 | https://raditech.mx/servicio-teleradiologia/ | Migrada de Elementor |
| 874 | https://raditech.mx/teleradiologia-alta-especialidad/ | Migrada de Elementor |
| 879 | https://raditech.mx/monitores-medicos-radiologia/ | Migrada de Elementor |
| 881 | https://raditech.mx/portal-x-card/ | Migrada de Elementor |
| 883 | https://raditech.mx/sistema-pacs-ris/ | Migrada de Elementor (diseño rt-pacs) |
| 890 | https://raditech.mx/sistema-his-medsi/ | Nueva (era 404 en Google) |
| 892 | https://raditech.mx/teleradiologia-resonancia-cardiovascular/ | Nueva (era 404) |
| 894 | https://raditech.mx/teleradiologia-tomografia-cardiaca/ | Nueva (era 404) |

#### SEO aplicado en todas las páginas
- Diseño rt-pacs-page (navy oscuro, naranja, hero animado, tabs, audience cards)
- Focus keywords configuradas en Rank Math
- FAQPage JSON-LD schema (6+ preguntas por página)
- Links internos (min 4) y externos de autoridad (min 3)
- Sección "Servicios relacionados" en todas
- Sin footer-links

#### Redirects 301 configurados en Rank Math
| Origen | Destino |
|--------|---------|
| /pacs-ris/ | /sistema-pacs-ris/ |
| /teleradiologia-de-alta-especialidad/ | /teleradiologia-alta-especialidad/ |
| /monitores-grado-medico/ | /monitores-medicos-radiologia/ |
| /x-card/ | /portal-x-card/ |

#### Agente Railway (web.py) actualizado
- SYSTEM prompt con diseño rt-pacs-page como estándar
- Protocolo SEO completo (keywords, FAQs, schema, links)
- **Fix crítico**: endpoint `/raditech-page-update` ahora respeta campo `status` y `slug`
  - Antes las páginas siempre quedaban en draft aunque se enviara `status: publish`
  - Commit: `d7301c3` — desplegado en Railway

---

## 🔴 Pendiente — requiere credenciales de la otra PC

### GSC Raditech — 403 Forbidden
**Problema**: El OAuth refresh token se renovó pero la cuenta de Google autenticada no tiene acceso a la propiedad `raditech.mx` en GSC.

**Para resolver en la otra PC:**

**Opción A** — Re-autenticar con la cuenta correcta:
1. Abrir en navegador: `https://web-production-3743c.up.railway.app/search-console/auth`
2. Hacer login con la cuenta de Google que administra raditech.mx en GSC
3. Copiar el Refresh Token que aparece
4. Railway → Variables → actualizar `GOOGLE_REFRESH_TOKEN` → Redeploy

**Opción B** — Agregar la cuenta actual como propietaria:
1. GSC → raditech.mx → Configuración → Usuarios y permisos
2. Agregar el email de la cuenta con la que se hizo auth
3. Rol: Propietario

**Service account** (para indexing): `gsc-indexing@tanus-498105.iam.gserviceaccount.com` — ya agregado como propietario en raditech.mx GSC ✅

---

## 🟡 Pendientes menores

### Rol usuario API de WordPress
El usuario del agente en WP (`enlacegrupoptm-com` o `gavitoa`) necesita rol **Editor** para publicar páginas sin intervención manual.
- WP Admin → Usuarios → cambiar rol a Editor

### Blog raditech.mx — sin artículos
Las 8 landings están listas pero el blog no tiene contenido de soporte.
Ideas de artículos de alto impacto:
- "¿Qué es el score de calcio CAC y para qué sirve?"
- "NOM-004 expediente electrónico: qué necesita tu hospital"
- "PACS vs RIS: diferencias y por qué necesitas ambos"
- "Teleradiología en México: marco legal y beneficios"

### Página 862 `/pacs-teleradiologia/`
Sigue publicada (Elementor viejo). Decidir: ¿dejar como está, redirigir a /sistema-pacs-ris/ o eliminar?

---

## Datos técnicos clave

### Railway
- URL agente: `https://web-production-3743c.up.railway.app`
- Repo: `C:\Users\Fam. Gavito Llernadi\ecommerce-agent`
- Variables clave: `RADITECH_WP_USER`, `RADITECH_WP_PASSWORD`, `GOOGLE_REFRESH_TOKEN`, `GOOGLE_SERVICE_ACCOUNT_JSON`, `RADITECH_GSC_SITE_URL`

### WordPress raditech.mx
- JWT auth (no Application Password)
- Usuarios: `gavitoa` (ID 6), `enlacegrupoptm-com` (ID 1), `rmena` (ID 4)
- Endpoint directo para subir HTML grande: `POST /raditech-page-update`
- Después de crear páginas via API: WP Admin → Páginas → seleccionar → Editar en lote → Estado: Publicada

### Diseño estándar
- Namespace CSS: `.rt-[page]` con prefijos únicos por página
- Paleta: `--rt-bg:#061423`, `--rt-orange:#f09a37`, `--rt-muted:#b8c6d8`
- Archivos HTML locales: `C:\Users\Fam. Gavito Llernadi\AppData\Local\Temp\rt_*.html`

### GSC PYS
- Funcionando ✅ después de renovar OAuth token
- Top keywords: "comprar mots-c mexico" (pos 30), "comprar peptidos mexico" (pos 44)
