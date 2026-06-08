---
tags: [proyecto, pys, agente, ecommerce, woocommerce, railway]
estado: Activo
ruta: C:\Users\gabom\ecommerce-agent\
---

# Ecommerce Agent

> Agente con acceso a WooCommerce para consultas y reportes de [[PYS Ecommerce]].

## Descripción

Agente Claude con tool use para consultar productos, órdenes, clientes e inventario de WooCommerce. Genera reportes PDF. Interfaz web incluida.

## Stack

| Componente | Tecnología |
|-----------|-----------|
| IA | Claude API (tool use) |
| Ecommerce | WooCommerce REST API v3 |
| PDF | fpdf2 |
| Deploy | Railway (`Procfile`) |

## Estructura

```
ecommerce-agent/
├── agent.py             ← agente principal + herramientas
├── web.py               ← interfaz web
├── exportar_chat.py     ← exportar conversaciones
├── sessions/            ← sesiones guardadas
├── templates/           ← templates de respuesta
└── chats_exportados/    ← historial exportado
```

## Herramientas Disponibles

- `get_products` — buscar por nombre, categoría, popularidad
- `get_orders` — órdenes por estado (pending, processing, completed)
- `get_customers` — clientes
- `get_inventory` — stock e inventario
- Reportes PDF con fpdf2

## Variables de Entorno

```env
WC_STORE_URL=https://peptidosysuplementos.mx
WC_CONSUMER_KEY=ck_xxx
WC_CONSUMER_SECRET=cs_xxx
ANTHROPIC_API_KEY=sk-ant-xxx
PTM_WP_USER=<pendiente>
PTM_WP_PASSWORD=<pendiente>
```

## Cross-linking PTM (Opción C) — mergeado 2026-06-08

El agente ahora tiene acceso bidireccional a **grupoptm.com** (WordPress):
- 8 funciones CRUD para posts y pages de PTM
- 8 herramientas Claude registradas
- System prompt actualizado con reglas de CTA bidireccional PYS ↔ PTM
- `PTM_URL` default: `https://grupoptm.com` (no necesita var Railway)
- **Pendiente:** agregar `PTM_WP_USER` y `PTM_WP_PASSWORD` en Railway una vez que WP esté configurado

### Setup WP grupoptm.com pendiente
- [ ] Plugin JWT Authentication for WP-API + editar wp-config.php
- [ ] Plugin Rank Math SEO
- [ ] Application Password → agregar en Railway

---

## Sesión 2026-06-08
Mergeó branch `claude/new-session-jthvC` a main. PTM cross-linking deployado en Railway. WordPress en grupoptm.com: Hostinger instalado, faltan plugins JWT Auth y Rank Math + credenciales Railway.
