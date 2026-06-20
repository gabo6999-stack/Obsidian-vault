---
tags: [proyecto, ptm-novo, telemedicina, nextjs]
estado: En desarrollo
ruta: C:\Users\gabom\PTM&PYS\PTM Novo\ptm-novo\
---

# PTM Novo

> Primera clínica de telemedicina especializada en péptidos en México.

## Descripción

Plataforma 100% online que conecta pacientes con médicos independientes con cédula profesional (TRH + péptidos). PTM cobra por la **consulta y el uso de la plataforma**; **no vende, surte ni cobra productos** — el paciente surte su receta donde elija. Modelo completo: `ecommerce-agent/MODELO_MONETIZACION_PTM.md`.

```
Paciente → Quiz → Pago consulta $1,500 (retenido) → Asignación de médico → Video-consulta → Receta → El paciente surte su receta donde elija (PTM no interviene)
```

## Stack

| Componente | Tecnología |
|-----------|-----------|
| Frontend/Backend | Next.js |
| Base de datos | Prisma + PostgreSQL |
| Deploy | Railway (producción) |
| Video-consulta | Whereby |
| Pagos | **Stripe Connect** — split $1,000 médico / $500 PTM vía `application_fee`. ✅ Configurado · ⏳ pruebas pendientes. *(Mercado Pago webhook previo)* |

## Modelo de Negocio
*(actualizado 2026-06-19 — fuera de la cadena de venta. Ver [[2026-06-19 — grupoptm Modelo Monetización]])*

- **Consulta:** $1,500 MXN (inicial y seguimiento)
- **Split:** $1,000 MXN médico / **$500 MXN comisión PTM** — comisión **fija por consulta**, nunca % ni atada al producto recetado.
- **Producto:** PTM **fuera de la venta**. El paciente surte su receta donde elija; ese dinero **nunca toca a PTM**. (PYS, si participa, es proveedor independiente — ver nota legal abajo.)
- **Retención:** se cobra $1,500 al agendar; el split se **libera al completarse la consulta** (el dinero cae al médico que atendió, cero clawback).
- **Facturación:** PTM **no factura al paciente**; el médico factura su acto médico, PTM factura su comisión de $500 al médico.
- **Política de cancelación:** paciente que cancela/no acude → se cobra, **sin reembolso** (el médico cobra igual); médico no-show → se **reagenda**, pago retenido hasta completar.
- **Capacidad:** 1 médico = 220 consultas/mes máx
- **Escala:** contratar nuevo médico cuando el actual supera ~150 pacientes

## Dos Programas

| Programa | Péptidos clave |
|----------|---------------|
| Pérdida de Peso | Semaglutide, Tirzepatide, AOD-9604 |
| Péptidos & Longevidad | BPC-157, TB-500, CJC-1295, Epithalon |

## Estado Actual

| Módulo | Estado |
|--------|--------|
| Frontend/Backend Next.js | 🟡 En desarrollo |
| Whereby video-consulta | ✅ Integrado |
| Stripe Connect split ($1,000/$500) | ✅ Configurado y **probado E2E** (test) |
| Prueba pago real (checkout→webhook→Payment HELD) | ✅ Verificada (test) |
| Onboarding Connect del médico | ✅ Verificado (médico Activo) |
| Rubros de terapia (4) + editor de médicos | ✅ |
| Credencialización / KYC médico (documentos + verificación) | ✅ |
| Almacenamiento documentos (Cloudflare R2 privado) | ✅ Conectado y probado |
| Mercado Pago | ❌ Eliminado (migrado a Stripe) |
| WhatsApp API | ⏳ Pendiente |
| Portal paciente | 🟡 Parcial |
| Portal médico | 🟡 Parcial (+ Mi perfil: rubros + documentos) |
| Panel admin (Antonio) | 🟡 Parcial (médicos, ingresos, credencialización) |
| Llaves Stripe LIVE | ⏳ Solo con dictamen legal COFEPRIS |

## Estructura Legal

- Plataforma tecnológica (no clínica médica)
- Médicos son contratistas independientes (con cédula profesional verificable)
- PTM **fuera de la cadena de venta** de productos (monetiza la consulta, no el producto)
- **Separación de [[PYS Ecommerce]] (decidida 2026-06-19):** PTM y PYS son **empresas independientes**; PYS NO es parte de la cadena de cobro de PTM. Ver [[Ecosistema PTM-PYS]].
- ⚠️ **Pendiente para que la separación sea real (no solo documental):** propiedad/control separados, cero flujo de dinero de producto, y eliminar el cross-linking operativo. Si persiste propiedad común, siguen siendo **parte relacionada** → lo valida el abogado (dictamen COFEPRIS).
- ⚠️ **Péptidos no registrados:** que un médico los recete no los vuelve legales de surtir. Es el hueco que el modelo de cobro no resuelve solo.
- Pending: definir SA de CV vs SAPI

## Relacionados

- [[PYS Ecommerce]] — **empresa independiente** (NO en la cadena de cobro de PTM)
- [[Ecosistema PTM-PYS]] — nota de **separación** PTM / PYS
- [[grupoptm.com]] — sitio marketing/SEO de PTM
- [[2026-06-19 — grupoptm Modelo Monetización]] — sesión donde se definió este modelo
- Bóveda PTM&PYS: `C:\Users\gabom\PTM&PYS\00 - Inicio\PTM Novo & PYS\PTM Novo\`



---

## Sesión 2026-06-08 — Plan Maestro grupoptm.mx
Se definió el plan completo para construir grupoptm.mx (sitio WordPress marketing/SEO). Rol claro: grupoptm.mx atrae tráfico orgánico con 5 landings SEO, la app Railway es donde el cliente actúa. Se identificó bug: PTM_URL en agente tiene default grupoptm.com, debe ser grupoptm.mx via variable Railway. Próximo paso crítico: instalar JWT Auth + Rank Math en WP y agregar 3 variables en Railway (PTM_WP_USER, PTM_WP_PASSWORD, PTM_URL).


---

## Nota técnica 2026-06-13 — SEO WordPress
En Raditech se completó auditoría SEO 100/100. Técnicas reutilizables para grupoptm.com: slug cycling para redirects 301, cookie+nonce para Rank Math REST API, purge LiteSpeed via admin panel. Ver [[2026-06-13 - Raditech SEO Score 100]].


---

## Sesión 2026-06-19 — Nuevo modelo de monetización (fuera de cadena de venta)
Se reescribió el **Modelo de Negocio** y el flujo para legitimar PTM como pura plataforma de telemedicina: consulta **$1,500** ($1,000 médico / $500 comisión fija PTM), pago con **retención** liberado al completar la consulta, **sin reembolso** por no-show del paciente, asignación automática de médico, PTM no factura al paciente. Se eliminó "péptidos a domicilio" (era cadena de venta). Flagueada la **tensión legal con PYS** (parte relacionada que vende producto debilita la defensa de plataforma) y el tema de **péptidos no registrados**. Detalle: [[2026-06-19 — grupoptm Modelo Monetización]] · `ecommerce-agent/MODELO_MONETIZACION_PTM.md` (commit 138b7f9).


---

## Sesión 2026-06-19 — Stripe, Rubros, KYC y Almacenamiento R2
Se implementó y verificó (modo test) gran parte de la plataforma: **Stripe Connect** de punta a punta (checkout $1,500 → webhook 200 → `Payment` HELD; onboarding del médico → Activo), migrando fuera de Mercado Pago; corrección del split a **$500/$1,000** en 3 pantallas; **4 rubros de terapia** del médico (`TherapyArea`) con **editor de médicos** y **auto-edición con aprobación** del admin; **credencialización (KYC médico)** con subida de documentos (ID, título general, especialidad, diplomados) y revisión del admin; y **almacenamiento de documentos en Cloudflare R2** (bucket privado + URLs firmadas), probado E2E. Detalle: [[2026-06-19 — PTM Novo Stripe, Rubros, KYC y R2]].

**Pendientes:** gatear asignación a médicos verificados + rubro aprobado · linkear rubros con el quiz · ciclo consulta→completar→transfer $1,000 · pasar a Stripe LIVE solo con dictamen legal COFEPRIS.

> Nota: los **2 programas** de la tabla de arriba quedaron desactualizados — el modelo ahora maneja **4 rubros** (Pérdida de peso, Péptidos & Longevidad, Salud para Hombres/TRH, Salud para Mujeres) en el enum `TherapyArea`. La ruta de la bóveda y el stack de "2 programas" son históricos.