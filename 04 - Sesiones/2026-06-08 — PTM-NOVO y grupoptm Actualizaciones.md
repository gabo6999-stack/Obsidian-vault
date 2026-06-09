# 2026-06-08 — PTM-NOVO y grupoptm.com Actualizaciones

## Resumen
Sesión de implementación completa cubriendo SEO en grupoptm.com y múltiples cambios en el app PTM-NOVO (precio, nomenclatura, nuevo programa Rendimiento).

---

## grupoptm.com — SEO

### Páginas actualizadas
| ID | Página | Slug |
|----|--------|------|
| 42877 | Pérdida de Peso | `perdida-de-peso-con-peptidos-en-mexico` |
| 42883 | Longevidad | `peptidos-longevidad-antiaging-mexico` |
| 42887 | Rendimiento | `peptidos-rendimiento-recuperacion-mexico` |

### Cambios SEO aplicados (3 páginas)
- FAQPage schema JSON-LD ✅
- Links externos PubMed/NCBI ✅
- Cross-links entre programas ✅
- Links a PYS (peptidosysuplementos.mx) ✅
- Rank Math titles y meta descriptions (seteados manualmente en WP Admin) ✅

### Página Rendimiento (42887) — adiciones
- **MK-677 (Ibutamoren)** agregado como 4ta tarjeta de péptido
- Slug de Longevidad corregido en widget inferior
- CTAs del app actualizados a `/#quiz`

### Nota técnica
- Rank Math NO se puede actualizar vía REST API en Hostinger/LiteSpeed — debe hacerse manualmente en WP Admin
- Nav de Elementor tiene link antiguo de Rendimiento pero 301 redirect activo — bajo impacto

---

## PTM-NOVO — App de telemedicina

**Repo:** https://github.com/gabo6999-stack/PTM-NOVO  
**Deploy:** https://ptm-novo-production.up.railway.app/

### Precio actualizado
- $500 MXN → **$1,500 MXN** en todo el app
- Split: PTM 80% ($1,200) / médico 20% ($300)
- Verificado en Mercado Pago producción ✅

### Nomenclatura
- "consulta" / "videoconsulta" → **"Orientación Médica"** en toda la UI (~20 archivos)
- Identificadores de código no cambiados (consultationId, /patient/consultas, etc.)

### Nuevo programa: Rendimiento & Recuperación
- Agregado como tercer programa (`PERFORMANCE` en DB)
- Péptidos: BPC-157, IGF-1 LR3, MK-677, TB-500, GHK-Cu
- Quiz ahora distingue 3 programas:
  - `weight_loss` → Pérdida de Peso
  - `performance` → Rendimiento & Recuperación
  - `longevity` / `sleep_hormones` → Péptidos & Longevidad

### Longevidad actualizada
- GHK-Cu agregado al resultado y formularios del médico
- Stack: Epithalon, GHK-Cu, CJC-1295/Ipamorelin, NAD+/Selank

### Flujo de pago verificado
- Checkout → MP muestra "Orientación médica — $1,500" ✅
- Token MP_ACCESS_TOKEN en Railway es **producción** (APP_USR-)
- Post-pago: /pago-exitoso → activación de cuenta → portal paciente

---

## Commits PTM-NOVO (2026-06-08)
1. `fix: precio $500 → $1500 y consulta → orientacion medica`
2. `fix: corregir ocurrencias restantes en resultado, admin, doctor, patient`
3. `fix: corregir ocurrencias restantes de consulta y videoconsulta`
4. `fix: corregir ocurrencias restantes en paginas de paciente`
5. `fix: corregir texto ilegible (negro sobre fondo oscuro) en resultado y quiz`
6. `feat: agregar programa Rendimiento y GHK-Cu a Longevidad`

---

## Tags
#PTM #PTM-NOVO #grupoptm #SEO #telemedicina #peptidos
