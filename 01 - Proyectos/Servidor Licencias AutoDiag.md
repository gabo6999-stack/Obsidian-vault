---
tags: [proyecto, autodiag, flask, railway, licencias]
estado: Deploy pendiente
ruta: C:\Users\gabom\autodiag-license-server\
---

# Servidor de Licencias AutoDiag

> API Flask que gestiona licencias de [[AutoDiag Pro]]. Deploy pendiente en Railway.

## Descripción

Valida y activa licencias de AutoDiag Pro. La app cliente consulta este servidor para determinar si está en estado `licensed`, `trial` o `expired`.

## Stack

| Componente | Tecnología |
|-----------|-----------|
| Backend | Flask (Python) |
| Base de datos | `db.py` |
| Deploy | Railway (`Procfile`, `railway.json`) |

## Estructura

```
autodiag-license-server/
├── app.py          ← API Flask principal
├── db.py           ← gestión de base de datos
├── Procfile        ← comando de inicio Railway
└── railway.json    ← configuración Railway
```

## Pendientes

- [ ] Deploy en Railway
- [ ] Configurar variables de entorno en Railway
- [ ] Probar flujo completo: activación → validación → expiración
