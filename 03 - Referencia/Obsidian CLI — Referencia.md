---
tags: [referencia, cli, obsidian, herramientas]
---

# Obsidian CLI — Referencia

> CLI instalado como parte de Obsidian desktop. Vault de este cerebro: `obsidian`

> ⚠️ **Requiere Obsidian abierto.** El CLI habla con la app de escritorio; si Obsidian está cerrado, los comandos fallan y `/gs` no puede escribir en el vault. Los agentes headless (Railway) **no** tienen este CLI — para ellos el contexto vive en `CLAUDE.md` (commiteado al repo) y en la memoria del proyecto.

## Instalación

1. Abrir Obsidian → Settings → General → habilitar "Command line interface"
2. Registrar → se agrega al PATH del sistema
3. Verificar: `obsidian help`

## Comandos Esenciales

### Buscar en el vault

```powershell
obsidian search query="PTM Novo" vault=Obsidian
obsidian search query="chatbot fastapi" vault=Obsidian
obsidian search query="railway deploy" vault=Obsidian
```

### Leer notas

```powershell
obsidian read file="PTM Novo" vault=Obsidian
obsidian read file="Ecosistema PTM-PYS" vault=Obsidian
obsidian read path="01 - Proyectos/AutoDiag Pro.md" vault=Obsidian
```

### Crear/modificar notas

```powershell
# Crear nueva nota
obsidian create name="Nueva Nota" content="contenido" vault=Obsidian

# Agregar contenido al final
obsidian append file="PTM Novo" content="nuevo texto" vault=Obsidian

# Agregar al inicio
obsidian prepend file="PTM Novo" content="nuevo texto" vault=Obsidian
```

### Listar contenido

```powershell
obsidian files vault=Obsidian
obsidian files folder="01 - Proyectos" vault=Obsidian
obsidian folders vault=Obsidian
obsidian tags vault=Obsidian
```

### Notas diarias

```powershell
obsidian daily vault=Obsidian
obsidian daily:append content="nota rápida" vault=Obsidian
obsidian daily:read vault=Obsidian
```

### Metadatos y propiedades

```powershell
obsidian properties file="PTM Novo" vault=Obsidian
obsidian property:read name="estado" file="PTM Novo" vault=Obsidian
```

## Opción `vault=`

Siempre especificar `vault=Obsidian` para apuntar a este cerebro.
Los vaults disponibles se listan con: `obsidian vaults`

## Estructura de Este Vault

```
obsidian/
├── Home.md                            ← dashboard principal
├── MOC SEO.md                         ← MOC transversal de SEO
├── PAGINA RECLUTAMIENTO DE MEDICOS.md
├── 01 - Proyectos/                    ← una nota por proyecto
│   ├── PTM Novo.md
│   ├── grupoptm.com.md
│   ├── PYS Ecommerce.md
│   ├── Raditech.md
│   ├── Agente de Blogs.md
│   ├── Agente SEO GSC.md
│   ├── Chatbot PYS.md
│   ├── Ecommerce Agent.md
│   ├── Social Video Agent.md
│   ├── WhatsApp Bot Tanus.md
│   ├── AutoDiag Pro.md
│   ├── Gloria IEM AI.md
│   └── Servidor Licencias AutoDiag.md
├── 02 - Contexto/
│   ├── Ecosistema PTM-PYS.md
│   └── Stack Compartido.md
├── 03 - Referencia/
│   └── Obsidian CLI — Referencia.md (este archivo)
├── 04 - Sesiones/                     ← bitácoras por fecha + MOCs de proyecto
│   ├── 2026-MM-DD — <tema>.md
│   ├── MOC - Raditech.md
│   └── MOC - Ecosistema PTM-PYS.md
└── Diario/                            ← daily notes (YYYY-MM-DD)
```
