---
tags: [proyecto, pys, whatsapp, bot, nodejs, railway]
estado: Pendiente (refresh token + OpenAI key)
ruta: C:\Users\gabom\whatsapp-bot\
---

# WhatsApp Bot Tanus

> Bot personal de WhatsApp con autopilot, Google Calendar y transcripción de audio.

## Descripción

Bot de WhatsApp con prefijo "tanus" que responde con autopilot on/off, integra Google Calendar y transcribe mensajes de voz con Whisper.

## Stack

| Componente | Tecnología |
|-----------|-----------|
| Plataforma | Node.js |
| WhatsApp | baileys/whatsapp-web |
| Calendar | Google Calendar API |
| Audio | OpenAI Whisper |
| IA | Claude API (`claude.js`) |
| Deploy | Railway (`railway.toml`) |

## Archivos Clave

```
whatsapp-bot/
├── index.js          ← entrada principal
├── claude.js         ← integración Claude
├── calendar.js       ← Google Calendar
├── scheduler.js      ← recordatorios
├── context.js        ← contexto conversación
├── style.json        ← estilo de respuestas
├── history.json      ← historial
├── notes.json        ← notas guardadas
└── auth_info/        ← credenciales WhatsApp (NO commitear)
```

## Pendientes Bloqueantes

- [ ] **Refresh token** de Google Calendar (expirado)
- [ ] **OpenAI API key** para Whisper (transcripción audio)

## Variables de Entorno

```env
ANTHROPIC_API_KEY=sk-ant-xxx
OPENAI_API_KEY=sk-xxx          ← pendiente
GOOGLE_CLIENT_ID=xxx
GOOGLE_CLIENT_SECRET=xxx
GOOGLE_REFRESH_TOKEN=xxx       ← pendiente renovar
```
