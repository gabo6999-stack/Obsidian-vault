---
tags: [proyecto, ptm-novo, telemedicina, nextjs]
estado: En desarrollo
ruta: C:\Users\gabom\PTM&PYS\PTM Novo\ptm-novo\
---

# PTM Novo

> Primera clínica de telemedicina especializada en péptidos en México.

## Descripción

Plataforma 100% online que conecta pacientes con médicos especializados en péptidos. Opera en mancuerna con [[PYS Ecommerce]] como brazo farmacéutico.

```
Paciente → Quiz → Pago ($500 MXN) → Video-consulta → Receta → Péptidos a domicilio
```

## Stack

| Componente | Tecnología |
|-----------|-----------|
| Frontend/Backend | Next.js |
| Base de datos | Prisma + PostgreSQL |
| Deploy | Railway (producción) |
| Video-consulta | Whereby |
| Pagos | Mercado Pago webhook |

## Modelo de Negocio

- **Consulta:** $500 MXN flat (inicial y seguimiento)
- **Split médico:** $400 MXN PTM / $100 MXN médico
- **Producto:** lo cobra PYS por separado (~$1,500 MXN/mes/paciente)
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
| Mercado Pago webhook | ✅ Productivo |
| Prueba pago real | ⏳ Pendiente |
| WhatsApp API | ⏳ Pendiente |
| Portal paciente | 🟡 Parcial |
| Portal médico | 🟡 Parcial |
| Panel admin (Antonio) | ⏳ Pendiente |

## Estructura Legal

- Plataforma tecnológica (no clínica médica)
- Médicos son contratistas independientes
- Pending: definir SA de CV vs SAPI

## Relacionados

- [[PYS Ecommerce]] — farmacia complementaria
- [[Ecosistema PTM-PYS]] — modelo de negocio completo
- Bóveda PTM&PYS: `C:\Users\gabom\PTM&PYS\00 - Inicio\PTM Novo & PYS\PTM Novo\`



---

## Sesión 2026-06-08 — Plan Maestro grupoptm.mx
Se definió el plan completo para construir grupoptm.mx (sitio WordPress marketing/SEO). Rol claro: grupoptm.mx atrae tráfico orgánico con 5 landings SEO, la app Railway es donde el cliente actúa. Se identificó bug: PTM_URL en agente tiene default grupoptm.com, debe ser grupoptm.mx via variable Railway. Próximo paso crítico: instalar JWT Auth + Rank Math en WP y agregar 3 variables en Railway (PTM_WP_USER, PTM_WP_PASSWORD, PTM_URL).


---

## Nota técnica 2026-06-13 — SEO WordPress
En Raditech se completó auditoría SEO 100/100. Técnicas reutilizables para grupoptm.com: slug cycling para redirects 301, cookie+nonce para Rank Math REST API, purge LiteSpeed via admin panel. Ver [[2026-06-13 - Raditech SEO Score 100]].