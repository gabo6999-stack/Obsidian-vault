---
aliases: [Agente Ecommerce, WooCommerce Agent]
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

## Cross-linking PTM (Opción C) — ⚠️ A REVERTIR (2026-06-19)

> PTM Novo y [[PYS Ecommerce]] son **empresas independientes**. El **CTA bidireccional PYS ↔ PTM** debe **quitarse** del system prompt — embudar paciente PTM → producto PYS es la cadena de venta que rompe la defensa de "PTM es solo plataforma". Ver [[Ecosistema PTM-PYS]].
>
> **Pendiente operativo:** remover del system prompt las reglas de CTA cruzado PYS↔PTM. (El acceso técnico CRUD a grupoptm.com puede quedarse para gestionar el sitio de PTM; lo que se elimina es el **embudo cruzado** hacia PYS.)

Estado mergeado 2026-06-08 (a revertir en la parte de CTA):
- 8 funciones CRUD para posts y pages de PTM
- 8 herramientas Claude registradas
- ~~System prompt con reglas de CTA bidireccional PYS ↔ PTM~~ ← eliminar
- `PTM_URL` default: `https://grupoptm.com` (no necesita var Railway)

### Setup WP grupoptm.com pendiente
- [ ] Plugin JWT Authentication for WP-API + editar wp-config.php
- [ ] Plugin Rank Math SEO
- [ ] Application Password → agregar en Railway

---

## Sesión 2026-06-08
Mergeó branch `claude/new-session-jthvC` a main. PTM cross-linking deployado en Railway. WordPress en grupoptm.com: Hostinger instalado, faltan plugins JWT Auth y Rank Math + credenciales Railway.



---

## Sesión 2026-06-13 — Redirects 301 via _wp_old_slug
Descubierto que el mecanismo correcto para redirects en raditech.mx es _wp_old_slug (nativo WP), no Rank Math REST API. Se configuraron 8 slugs antiguos en las páginas destino correctas. Pendiente: verificar que funcionen tras purge de LiteSpeed Cache.