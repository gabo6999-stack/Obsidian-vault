---
tags: [sesion, nexus, pys, agentes, infraestructura, fix, 2026-06]
fecha: 2026-06-19
proyecto: NEXUS Centro de Comando
---

# Sesión 2026-06-19 — NEXUS Centro de Comando + Fix Agente Blogs PYS

Proyectos: [[PYS Ecommerce]] · [[Agente de Blogs]] · [[Ecommerce Agent]] · [[Social Video Agent]] · [[WhatsApp Bot Tanus]] · [[Chatbot PYS]]

> **Resumen:** Se construyó y desplegó **NEXUS**, una app de gestión de proyectos/ideas/tareas estilo Matrix para el equipo, y se integró con Obsidian y con todos los agentes de Railway. Al final se diagnosticó y arregló por qué el Agente de Blogs había dejado de publicar en peptidosysuplementos.mx.

## Contexto
El usuario pidió una app visual (fondo negro estilo Matrix) para guardar lluvia de ideas, llevar tareas con checkbox, avisar pendientes, planes de trabajo, % de avance, eficiencia y prioridad de proyectos. Debía vivir en Railway con memoria (base de datos).

## Qué se hizo

### 1. NEXUS — Centro de Comando (app nueva)
- Stack: **Node + Express + PostgreSQL** (memoria) en **Railway**. Frontend HTML/JS estilo Matrix (lluvia de código verde).
- Funciones: login de equipo (JWT+bcrypt), proyectos con prioridad y % de avance, lluvia de ideas con votos (convertibles a tareas), tareas Kanban drag&drop, plan de trabajo por fases, métricas de eficiencia, búsqueda, etiquetas, recordatorios automáticos, reporte PDF, y botón para eliminar proyectos.
- Código local: `C:\Users\Fam. Gavito Llernadi\nexus-app`. Repo GitHub privado: `gabo6999-stack/nexus-app`.
- **En vivo:** https://nexus-app-production-22e1.up.railway.app (login admin en variables de Railway `ADMIN_*`).

### 2. Integraciones
- **Fase 1 — Pendientes:** se importaron los proyectos/tareas reales a NEXUS (PTM Telemedicina, Agente Instagram, Blogs grupoptm, 4 verticales, Calculadora de Dosis, NEXUS sistema).
- **Fase 2 — Obsidian (sync continuo):** NEXUS lee este vault público (`gabo6999-stack/Obsidian-vault`) desde GitHub **cada 15 min** y vuelca los checkboxes `- [ ]`/`- [x]` a un proyecto "📓 Obsidian Vault" (91 tareas detectadas). Es **una sola dirección** (NEXUS no escribe de vuelta al vault, por seguridad).
- **Fase 3a — Salud de agentes:** tablero que hace ping a cada servicio de Railway (Online/Offline). Detectó que `upbeat-happiness` está caído.
- **Fase 3b — Reporte de actividad:** cada agente avisa a NEXUS cuando completa una acción (endpoint protegido `/api/ingest`). Conectados: Agente Blogs, Agente-SEO, Raditech Agente Blogs, Social Video Agent, Tanus-Whatsapp, Tanus-Chatbox, pys-backlink-prospector. **Pendiente:** RAFA VideoAgente (su deploy falla desde el 30-may por problema previo del Dockerfile, no por el código nuevo).

### 3. Fix — Agente de Blogs no publicaba en PYS
- **Síntoma:** peptidosysuplementos.mx fallaba con "Post creation failed" en cada intento desde el **9 de junio**; grupoptm.com publicaba bien.
- **Causa raíz:** la **contraseña de WordPress de PYS había cambiado**; el agente seguía con la vieja → el login JWT (`/wp-json/jwt-auth/v1/token`) daba 403. PYS autentica por **JWT con la contraseña normal de WP** (no Application Password).
- **Arreglo:** se actualizó `SITE1_WP_PASSWORD` en Railway; se verificó que el JWT ya autentica. Se disparó un blog de prueba real (**TB-500**) que se publicó OK y se reportó a NEXUS. Usuario de publicación PYS: `GavitoA` (id 4).

### 4. Fix — Imagen grande con firma de Unsplash
- **Problema:** los blogs mostraban una imagen grande (a veces repetida y fuera de tema, ej. un frasco de CBD) con crédito "Foto: … en Unsplash" antes del texto.
- **Causa:** el agente **incrustaba la imagen como `<figure>` al inicio del contenido** (`tools/wordpress.py`), además de ponerla como destacada; y Unsplash siempre devolvía la primera coincidencia para queries genéricos.
- **Arreglo:** se quitó la inyección de la imagen en el contenido (queda solo como **imagen destacada**, como los demás blogs). Se limpió también el post de TB-500 ya publicado.

## Decisiones tomadas
- NEXUS es un **tablero que refleja** el trabajo, no controla los sistemas: todas las integraciones son **de una vía hacia NEXUS**. Borrar un proyecto en NEXUS **no afecta** agentes, sitios ni registros reales.
- Imágenes de blog: por ahora solo **imagen destacada**, sin imagen incrustada ni crédito de Unsplash en el cuerpo.
- RAFA VideoAgente: se deja para otra sesión (arreglar su deploy pre-existente).

## Próximos pasos
- [ ] Arreglar el deploy del **RAFA VideoAgente** (falla desde 30-may, probable Dockerfile/healthcheck) para cerrar 8/8 agentes activos.
- [ ] Revisar qué es `upbeat-happiness` (servicio Railway caído).
- [ ] (Opcional) Mejorar imágenes de blog: aleatorizar/optimizar queries de Unsplash o pasar a imágenes con IA.
- [ ] (Opcional) Conectar auto-deploy de NEXUS desde GitHub.

## Referencias
- App NEXUS: https://nexus-app-production-22e1.up.railway.app · repo `gabo6999-stack/nexus-app` · local `C:\Users\Fam. Gavito Llernadi\nexus-app`
- Blog de prueba: https://peptidosysuplementos.mx/tb-500-peptido-recuperacion-atletas/
- Relacionados: [[MOC - Ecosistema PTM-PYS]] · [[PYS Ecommerce]] · [[Agente de Blogs]] · [[Ecommerce Agent]]
