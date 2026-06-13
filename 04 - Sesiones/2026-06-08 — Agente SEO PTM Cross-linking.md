---
tags:
  - sesion
  - agente-seo
  - peptidosysuplementos
  - grupoptm
  - 2026-06
fecha: 2026-06-08 01:48
proyecto: Agente SEO
---

# Sesión 2026-06-08 — Agente SEO: PTM Cross-linking Deploy

## Qué se hizo
- Detecté que la Opción C (cross-linking PYS ↔ grupoptm.com) estaba implementada en la branch `claude/new-session-jthvC` pero no mergeada a main
- Revisé el diff completo: 329 líneas agregadas (JWT auth PTM, 8 funciones CRUD, 8 herramientas Claude, system prompt con reglas de cross-linking bidireccional)
- Hice merge de `claude/new-session-jthvC` → `main` con `--no-ff`
- Push a GitHub → Railway desplegó automáticamente
- Discutimos y detallé los pasos para levantar WordPress en grupoptm.com
- Usuario confirmó que ya tiene Hostinger con WordPress instalado

## Decisiones técnicas
- Merge con `--no-ff` para preservar historia de la branch PTM
- `PTM_URL` usa `https://grupoptm.com` como default (no necesita var en Railway)
- Application Password de WP (más seguro que contraseña directa del admin)
- JWT Authentication plugin requiere `JWT_AUTH_SECRET_KEY` en wp-config.php

## Problemas encontrados
- La branch PTM no estaba mergeada a main — el usuario creía que ya estaba deployada
- En la revisión inicial de web.py (en main) no había ningún código PTM, causando confusión inicial

## Próximos pasos
- [ ] Instalar plugin JWT Authentication for WP-API en grupoptm.com
- [ ] Instalar Rank Math SEO en grupoptm.com
- [ ] Editar wp-config.php con JWT_AUTH_SECRET_KEY y JWT_AUTH_CORS_ENABLE
- [ ] Crear Application Password en WP Admin para el agente SEO
- [ ] Agregar PTM_WP_USER y PTM_WP_PASSWORD en Railway (ecommerce-agent)
- [ ] Verificar que el agente lee/escribe correctamente en grupoptm.com
- [ ] Crear landing pages SEO iniciales en grupoptm.com (pérdida de peso, longevidad, rendimiento, salud hormonal)



---

**MOC:** [[MOC - SEO]] | [[MOC - Ecosistema PTM-PYS]]
