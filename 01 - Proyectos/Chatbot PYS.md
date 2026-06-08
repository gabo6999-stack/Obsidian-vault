---
tags: [proyecto, pys, chatbot, fastapi, chromadb, railway]
estado: Deploy pendiente
ruta: C:\Users\gabom\chatbot\
---

# Chatbot PYS

> Chatbot RAG para atender clientes de peptidosysuplementos.mx.

## Descripción

Backend FastAPI con RAG (ChromaDB) que responde preguntas de clientes sobre péptidos. Widget HTML embebible en WordPress. Streaming SSE para respuestas en tiempo real.

## Stack

| Componente | Tecnología |
|-----------|-----------|
| Backend | FastAPI (Python) |
| Memoria | SQLite (`chatbot.db`) |
| Búsqueda semántica | ChromaDB (RAG) |
| IA | Claude API (streaming SSE) |
| Frontend | `chat-widget.html` (vanilla JS) |
| Deploy | Railway (pendiente) |

## Estructura

```
chatbot/
├── backend/
│   ├── main.py          ← FastAPI, endpoint /chat streaming
│   ├── memory.py        ← historial conversación (SQLite)
│   ├── rag.py           ← búsqueda semántica (ChromaDB)
│   ├── analytics.py     ← métricas de uso
│   └── requirements.txt
└── frontend/
    └── chat-widget.html ← widget para WordPress
```

## Endpoints

| Método | Ruta | Descripción |
|--------|------|-------------|
| POST | `/chat` | Mensaje → respuesta streaming SSE |
| POST | `/chat/clear` | Limpia historial sesión |
| POST | `/rag/index` | Indexa documentos |
| GET | `/health` | Status del servidor |

## Pendientes para Deploy

1. `railway login && railway init && railway up`
2. Cambiar `BACKEND_URL` en el widget al URL de Railway
3. Copiar bloque widget en WordPress antes de `</body>`

## Variables de Entorno

```env
ANTHROPIC_API_KEY=sk-ant-xxx
```
