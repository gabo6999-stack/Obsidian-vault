---
tags: [sesion, grupoptm, meta-ads, compliance, 2026-06]
fecha: 2026-06-19
proyecto: grupoptm.com
---

# Sesión 2026-06-19 — grupoptm.com Legitimización (retiro funnel PYS)

## Objetivo
Convertir grupoptm.com en sitio **100% legítimo de telemedicina de péptidos**, sin funnel ni enlaces públicos a peptidosysuplementos.mx, para correr campañas de **Meta (Andromeda)** sin penalización. RHT como Fase 2.

## Qué se hizo (en el vault)
- Mapeadas todas las referencias a PYS en los artefactos disponibles. La landing exportada `peptidos-rendimiento-recuperacion-mexico` (ID 42887) tenía **8 referencias** en 7 patrones distintos:
  1. Bloque comercial completo `<div class="ptm-pys-links">…Compra tus péptidos…en PYS</div>`
  2. Botón CTA naranja "Ver productos en PYS"
  3. Cross-link `ptm-xlink` "Ver catálogo de péptidos en peptidosysuplementos.mx →"
  4. Links a producto `/producto/bpc-157…`, `/producto/igf-1-lr3…`, categoría `/categoria-producto/mk-677/`
  5. FAQ "¿Dónde encuentro los productos?" apuntando a la tienda
  6. FAQ Schema JSON-LD nombrando la tienda
  7. Nota interna `App: … · PYS: …`
- Generada versión **limpia y verificada (0 menciones PYS)**: `05 - grupoptm legit/peptidos-rendimiento-recuperacion-mexico.clean.html` + `.clean.json` (listo para `update_ptm_page`).
- Creado **runbook ejecutable**: [[grupoptm.com — Legitimización (retiro funnel PYS)]] con política de reemplazo, regex probadas, alcance, checklist y criterio de aceptación.
- Actualizadas notas de estrategia: [[grupoptm.com]] y [[Ecosistema PTM-PYS]].

## Decisiones
- CTAs de compra → se **eliminan**; el destino legítimo es la **app PTM Novo** (evaluación/consulta). La derivación a la tienda la hace el **médico en consulta**.
- FAQ "¿Dónde encuentro los productos?" reescrita a "¿Cómo obtengo mi tratamiento?" (clave médica, sin tienda).
- El sentido PYS → grupoptm.com (consulta) puede conservarse; lo que se retira es grupoptm.com → PYS.

## Restricción de la sesión
Entorno limitado al repo `obsidian-vault`. **No hay acceso** al WordPress vivo (Hostinger) ni a `ecommerce-agent`/`PTM-NOVO` (scope GitHub bloqueado). Por eso la limpieza del sitio vivo y del código del agente queda como ejecución del usuario vía el runbook.

## Próximos pasos
- [ ] Aplicar runbook a las 16 páginas + blogs vía Agente SEO / WP REST
- [ ] Desactivar cross-linking PTM→PYS en `ecommerce-agent`
- [ ] Quitar enlaces a PYS del prompt de `agente-blogs`
- [ ] Limpiar menú/footer/header (tema) y regenerar sitemap Rank Math
- [ ] Verificación final: 0 coincidencias de `peptidosysuplementos` en todo el sitio
- [ ] **Fase 2:** agregar Terapia de Reemplazo Hormonal (RHT)

---

**MOC:** [[MOC - Ecosistema PTM-PYS]]
