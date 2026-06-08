---
tags: [contexto, stack, tecnologia, convenciones]
---

# Stack Compartido y Convenciones

## Deploy: Railway

Todos los backends/agentes se despliegan en Railway.

```bash
railway login
railway init
railway up
```

Archivos de configuración: `Procfile` (comando de inicio) + `railway.toml` o `railway.json`

## IA: Claude API (Anthropic)

Todos los proyectos usan Claude como motor de IA.

| Modelo | Uso |
|--------|-----|
| claude-sonnet-4-6 | Proyectos con razonamiento complejo |
| claude-haiku-4-5 | Apps ligeras (Gloria IEM AI) |

Variable de entorno siempre: `ANTHROPIC_API_KEY=sk-ant-xxx`

## Python: PyInstaller

Para compilar apps de escritorio (AutoDiag, Gloria IEM AI):

```bash
# SIEMPRE así — nunca pyinstaller directo
python -m PyInstaller archivo.spec
```

## WordPress: Rank Math SEO

El sitio PYS usa **Rank Math** (no Yoast).

Campos correctos:
- `rank_math_title` ← título SEO
- `rank_math_description` ← meta descripción

Para páginas Elementor: solo editar title/meta/slug/rank_math vía API. El diseño SOLO en el editor Elementor.

## Rutas de Proyectos

| Proyecto | Ruta |
|----------|------|
| PTM Novo | `C:\Users\gabom\PTM&PYS\PTM Novo\ptm-novo\` |
| Agente Blogs | `C:\Users\gabom\agente-blogs\` |
| Chatbot PYS | `C:\Users\gabom\chatbot\` |
| Ecommerce Agent | `C:\Users\gabom\ecommerce-agent\` |
| Social Video Agent | `C:\Users\gabom\social-video-agent\` |
| WhatsApp Bot | `C:\Users\gabom\whatsapp-bot\` |
| AutoDiag Pro | `C:\Users\gabom\autodiag\` |
| Gloria IEM AI | `C:\Users\gabom\iem-ai\` |
| Servidor Licencias | `C:\Users\gabom\autodiag-license-server\` |
| Bóveda Obsidian PTM&PYS | `C:\Users\gabom\PTM&PYS\` |
| Bóveda Obsidian (cerebro) | `C:\Users\gabom\obsidian\` |
