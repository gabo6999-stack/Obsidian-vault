---
tags: [sesion, pys, calculadora, peptidos, widget, elementor]
fecha: 2026-06-19
---

# 2026-06-19 — Calculadora de Reconstitución de Péptidos (PYS)

Se construyó una calculadora de reconstitución/dosificación de péptidos para
**peptidosysuplementos.mx**, inspirada en la de nexa-peptides.com/calculadora/.

> Nota: no se pudo navegar con Chrome (entorno remoto sin navegador real; Nexa
> y varias calculadoras devuelven 403 al fetch). La lógica se confirmó vía
> búsqueda web — es 100% estándar en todas las calculadoras de péptidos.

## Entregable
`01 - Proyectos/calculadora-peptidos-pys.html` — widget autocontenido
(HTML + CSS + JS en un bloque, sin dependencias).

## Variables / Fórmulas
- Concentración (mcg/mL) = `(mg_vial × 1000) / mL_agua`
- Volumen a inyectar (mL) = `dosis_mcg / concentración`
- **Unidades jeringa (IU U-100)** = `volumen_mL × 100` ← lo que marca la gráfica
- Dosis por vial = `floor((mg_vial × 1000) / dosis_mcg)`

## Features
- Inputs: péptido (preset), mg vial, agua bacteriostática mL, dosis (mcg/mg), jeringa (0.3/0.5/1 mL)
- Outputs: unidades a jalar (destacado), concentración, volumen, dosis por vial
- **Diagrama de jeringa SVG** que se llena dinámicamente con marcas de unidades
- Presets editables (BPC-157, TB-500, Ipamorelin, Semaglutida, Tirzepatida, etc.)
- Avisos: dosis excede capacidad de jeringa / volumen muy pequeño
- Aviso legal + CTA a /contacto

## Cómo publicar (Elementor)
1. Editar página en Elementor → widget **HTML**
2. Pegar todo el archivo → Actualizar
3. Todo el CSS está scopeado bajo `#pys-pep-calc` (no choca con el tema)

## Pendientes / mejoras posibles
- [ ] Ajustar paleta a colores exactos de marca PYS
- [ ] Validar lista de presets y dosis típicas con catálogo real de la tienda
- [ ] Opcional: enlazar cada preset a su producto en la tienda (CTA de compra)
- [ ] Publicar en WordPress y verificar render en móvil

## Proyecto
[[PYS Ecommerce]]
