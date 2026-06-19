---
tags: [proyecto, grupoptm, meta-ads, compliance, seo, runbook]
estado: En ejecución
fecha: 2026-06-19
proyecto: grupoptm.com
---

# grupoptm.com — Legitimización (retiro funnel PYS)

> **Objetivo:** convertir grupoptm.com en un sitio **100% legítimo de telemedicina de péptidos**, sin ningún funnel ni enlace comercial hacia **peptidosysuplementos.mx (PYS)**, para poder correr campañas de **Meta (Andromeda)** sin penalización por venta de sustancias/farmacia.
>
> **Regla de negocio nueva:** el sitio educa y capta → el destino legítimo es la **app PTM Novo** (evaluación/consulta). El **médico**, durante la consulta, es quien deriva al paciente a la tienda WooCommerce (PYS). **El sitio público nunca enlaza a la tienda.**
>
> **Fase 2 (después):** agregar Terapia de Reemplazo Hormonal (RHT).

---

## Política de reemplazo (cómo se trata cada referencia)

| Tipo de referencia a PYS | Acción | Reemplazo |
|---|---|---|
| Botón CTA de compra (`ptm-btn` → PYS) | **Eliminar** | El CTA legítimo "Iniciar evaluación / Agenda tu consulta" → app PTM Novo ya cubre el lugar |
| Cross-link `ptm-xlink` "Ver catálogo / Ver X en PYS" | **Eliminar** (con su `<div>` contenedor si queda vacío) | — |
| Links a producto/categoría (`/producto/…`, `/categoria-producto/…`) | **Eliminar** el `<a>` (y el `<li>` si aplica) | Conservar solo las citas PubMed `[n]` |
| Sección comercial completa (`<div class="ptm-pys-links">…Compra tus … en PYS</div>`) | **Eliminar bloque entero** | — |
| Texto FAQ "¿Dónde encuentro los productos?" → tienda | **Reescribir** en clave médica | "¿Cómo obtengo mi tratamiento?" → el médico define protocolo y orienta tras la consulta |
| FAQ Schema JSON-LD que nombra la tienda | **Reescribir** texto del `Answer` | Versión neutral sin nombre de tienda |
| Nota/comentario interno `App: … · PYS: …` | **Quitar** la parte PYS | Dejar solo la URL de la app |
| Menciones de marca en texto ("Péptidos y Suplementos (PYS)", "en PYS") | **Eliminar/neutralizar** | Lenguaje médico ("tu médico tratante…") |

**App PTM Novo (destino legítimo):** `https://ptm-novo-production.up.railway.app/`

---

## Alcance (dónde hay que aplicarlo)

1. **Las páginas/landings de grupoptm.com** (≈16: 12 landings SEO + 4 generales).
2. **Los blogs** publicados en grupoptm.com (revisar cuerpo + enlaces internos).
3. **El código del agente** `ecommerce-agent` (Railway) → desactivar la inyección automática de cross-links PTM→PYS para que las páginas/blogs nuevos no vuelvan a meter el funnel.
4. **Generador de blogs** (`agente-blogs`) → quitar prompt/plantilla que inserta enlaces a PYS.
5. **Sitemap / Rank Math / menús / footer / header** (Customizer del tema) → quitar enlaces a la tienda.

---

## Runbook de ejecución

### Paso 1 — Enumerar todas las páginas y blogs
```
# desde el Agente SEO / WP REST
get_ptm_pages()          # lista las 16 páginas (id, slug, link)
# blogs:
GET https://grupoptm.com/wp-json/wp/v2/posts?per_page=100&_fields=id,slug,link
```

### Paso 2 — Detectar referencias en TODO el sitio
Para cada page/post, traer el `content.rendered` y buscar:
```
peptidosysuplementos      (URLs y menciones)
"en PYS"  /  "(PYS)"  /  "Péptidos y Suplementos"
/producto/  /  /categoria-producto/
ptm-pys-links             (la clase del bloque comercial)
```

### Paso 3 — Aplicar las transformaciones (ver tabla de política)
Patrón de referencia ya validado en `peptidos-rendimiento-recuperacion-mexico` (ID 42887).
Transformaciones probadas (regex sobre el `content`):
```python
# 1) bloque comercial completo
re.sub(r'<div class="ptm-pys-links".*?</div>\s*', '', c, flags=re.S)
# 2) botón CTA de compra
re.sub(r'\n?<a class="ptm-btn[^>]*peptidosysuplementos[^>]*>.*?</a>', '', c, flags=re.S)
# 3) div del xlink "Ver catálogo…"
re.sub(r'<div style="margin-top:26px;"><a class="ptm-xlink"[^>]*peptidosysuplementos.*?</a></div>', '', c, flags=re.S)
# 4) barrido de cualquier <a> a PYS restante
re.sub(r'\n?<a\b[^>]*peptidosysuplementos[^>]*>.*?</a>', '', c, flags=re.S)
# 5) FAQ schema / texto: reemplazos de cadena (ver política)
# 6) nota interna:  ' · PYS: https://peptidosysuplementos.mx/'  ->  ''
```
Subir con `update_ptm_page(id, content=...)`.

> **Plantilla de referencia ya limpia:** `05 - grupoptm legit/peptidos-rendimiento-recuperacion-mexico.clean.html` (0 menciones a PYS, verificado).

### Paso 4 — Código del agente (que no vuelva a inyectar el funnel)
- `ecommerce-agent`: localizar la función de cross-linking PTM→PYS (mergeada a main el 2026-06-08) y **desactivarla** para grupoptm.com.
- `agente-blogs`: quitar del prompt/plantilla la instrucción de enlazar a `peptidosysuplementos.mx`.

### Paso 5 — Tema/menús/sitemap
- Header/Footer (Customizer o Elementor): quitar cualquier botón/enlace a la tienda.
- Rank Math: regenerar sitemap; confirmar que no indexa rutas hacia la tienda.

### Paso 6 — Verificación final (criterio de aceptación)
Recorrer TODAS las páginas y posts y confirmar **0 coincidencias** de:
`peptidosysuplementos`, `/producto/`, `/categoria-producto/`, `ptm-pys-links`, `(PYS)`, `en PYS`.
```
# pseudo: por cada page/post -> assert "peptidosysuplementos" not in content
```

---

## Checklist de páginas (marcar al limpiar)

- [x] `peptidos-rendimiento-recuperacion-mexico` (ID 42887) — **limpia** (plantilla de referencia)
- [ ] `/` (home)
- [ ] `perdida-de-peso`
- [ ] `semaglutida-mexico`
- [ ] `tirzepatida-mexico` (ID 42995)
- [ ] `longevidad`
- [ ] `como-funciona`
- [ ] `medicos` / `para-medicos`
- [ ] *(resto de landings hasta completar las 12)*
- [ ] Páginas generales (4): inicio, aviso-privacidad, etc.
- [ ] Todos los blogs (`/wp-json/wp/v2/posts`)
- [ ] `ecommerce-agent`: cross-link PTM→PYS desactivado
- [ ] `agente-blogs`: prompt sin enlaces a PYS
- [ ] Menú/footer/header del tema sin enlace a tienda
- [ ] Sitemap Rank Math regenerado
- [ ] Verificación final: 0 coincidencias en todo el sitio

---

## Notas de compliance Meta (Andromeda)
- El sitio **no vende** ni enlaza a venta de péptidos/sustancias → es plataforma de **telemedicina** (servicio médico).
- CTAs apuntan a **evaluación/consulta médica**, no a "comprar".
- Lenguaje: "valoración médica", "protocolo personalizado", "el médico define el tratamiento" — nunca "compra directa".
- La derivación a la farmacia ocurre **dentro de la consulta**, fuera del sitio público.

---

**Relacionado:** [[grupoptm.com]] · [[Ecosistema PTM-PYS]] · [[Agente SEO GSC]] · [[MOC - Ecosistema PTM-PYS]]
