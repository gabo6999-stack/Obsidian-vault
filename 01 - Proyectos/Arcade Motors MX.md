---
aliases: [arcademotorsmx.com, Arcade Motors MX, Autos ALV]
tags: [proyecto, arcade-motors, marketplace, autos, php, mysql, hostinger]
estado: Activo
sitio: https://arcademotorsmx.com
carpeta: C:\Users\gabom\autos-alv\
---

# Arcade Motors MX

> Estado: 🟢 Activo · **EN VIVO en https://arcademotorsmx.com**
> Marketplace de anuncios de autos y motos para México. Publica gratis, vende directo, **sin comisiones por venta**.
> **Nombre oficial y único: "Arcade Motors MX"** (ex "Autos ALV" — solo referencia histórica del rebrand). Carpeta local sin renombrar: `C:\Users\gabom\autos-alv\`.

## Stack
- PHP vanilla + MySQL (PDO); fotos a `/uploads` con GD
- Hostinger — plan **Cloud Professional** (mismo que aloja alv.peptidosysuplementos.mx y marinecustomwire.com)
- Dominio oficial **arcademotorsmx.com** (SSL activo)
- BD: `u413644232_autosalv` / usuario `u413644232_alv` (pass en `includes/config.php`)
- Sin framework. Deploy vía **File Manager (filebrowser)** de hPanel. PHP no se previsualiza local (server estático no ejecuta PHP) → se prueba en Hostinger.

## Estética — "CRT Arcade 80s" (APROBADA)
- Fósforo **ámbar `#ffb627`** sobre negro `#050307`; acentos **magenta `#ff2d95`** + cyan; **verde fósforo `#7CFC6B`** (login/signup)
- Fuentes: **Press Start 2P** (logo/títulos pixel) + **VT323** (terminal)
- Logo = carrito neón rosa-naranja + wordmark "ARCADE MOTORS MX"; rayado IBM/CRT **solo en el logo** (`.bigwm`)
- Hero = terminal de arranque ("ARCADE-MOTORS-MX COMPUTER... SISTEMA LISTO. >")
- Prototipo aprobado: `preview-arcade.html`. Descartados: Matrix verde + 80s synthwave.

## Estructura
- Páginas: **index** (hero terminal), **buscar** (filtros catálogo), **anuncio** (?id), **publicar** (POST, cascade), **planes**, **gestionar** (?token, sin login), **registro / ingresar / cuenta** (cuentas de usuario)
- Admin en `/admin` (setup→login→dashboard), sesión independiente de las cuentas de usuario
- Bandera `COBRO_ACTIVO` en config.php: `false` = 100% gratis (oculta PLANES); `true` = Fase 2 freemium
- Leyenda obligatoria: "Publica gratis" + "sin comisiones por venta". NUNCA "gratuito"/"100% gratis" (en Fase 2 sí cobra).

## Catálogo tipo MercadoLibre (COMPLETO)
- **150 marcas / 3,011 modelos / 8,380 versiones**, 8 categorías. Base **factual** (NO scrape de ML; estructura como referencia).
- Esquema relacional `cat_categorias → cat_marcas → cat_modelos → cat_versiones`. Importado en phpMyAdmin (`catalog_full.sql`).
- Cascade Categoría→Marca→Modelo en **publicar.php** (AJAX vía catalog_api.php) y **buscar.php** (server-side, saneo anti-"estado fantasma"). Verificado en vivo.

## Cuentas de usuario + buscador en header (2026-06-25)
- **Buscador general CRT** en el header → `buscar.php?q=` (prompt `>` verde, refleja `$_GET['q']`).
- **Cuentas REALES**: `registro.php` + `ingresar.php` (login + `?logout=1`) + `cuenta.php` (perfil) + `includes/auth.php` (sesión `$_SESSION['uid']`, independiente del admin). **auth.php AUTOCREA la tabla `usuarios`** en el 1er registro (`CREATE TABLE IF NOT EXISTS`, no requiere SQL manual). Hash con `password_hash`.
- Header con estado: logueado → `▸ NOMBRE · SALIR`; sin sesión → `INGRESAR · REGISTRARTE` (verde `#7CFC6B`). **+ PUBLICAR conservado.**
- ✅ Confirmado end-to-end (usuario ANTONIO registrado, tabla autocreada, sesión OK).
- **Futuro:** ligar anuncios a `usuario_id` → "Mis anuncios" en cuenta.php.

## Modelo de negocio
- Meta **$50,000 MXN netos/mes en ~6 meses**.
- Beta: todo gratis (AdSense **retirado** del sitio — rompía la estética y daba poco).
- Fase 2 freemium: gratis 30d → renovación **$49**/30d, **Permanente $149** (candado anti-fraude), **Destacado +$99**. Lotes/agencias: **$1,799** y **$3,499**/mes.
- **Insight clave:** el motor del $50k son los **lotes/agencias (B2B recurrente)**, no particulares ni AdSense. Modelo en `Autos_ALV_Proyeccion_Financiera.xlsx` (5 lotes nuevos/mes desde mes 2 → meta en mes 6).

## Deploy — aprendizajes clave (filebrowser de Hostinger)
- **Caché de assets:** versionado `?v=<?= @filemtime() ?>` en header/footer (sin esto sirve JS/CSS viejos tras deploy).
- **Guardar archivo EXISTENTE en el server:** editor Ace → `setValue(c,-1)` + marcar dirty (`insert(' ')`/`remove('left')`) + disparar Save por **JS `.click()`**. (setValue solo NO persiste; Ctrl+S tampoco.)
- **Crear archivo NUEVO sin usuario (breakthrough 2026-06-25):** JS-clic "New file" → `find` el input → **`form_input(ref, nombre)`** → JS-clic Create → abre editor → inyectar contenido (PowerShell base64 → `decodeURIComponent(escape(atob(b64)))`) + save. (El `computer.type` real es intermitente; el native-setter NO sincroniza el modelo Vue; **`form_input` SÍ**.)
- **Token del filebrowser caduca (403):** reabrir desde hPanel → sitio → Archivos → "Acceder a archivos de <dominio>".
- Borrados directos: marcar "Skip trash bin and delete immediately".
- **Rebrand gotcha:** el reemplazo de texto (`rebrand.py`) NO atrapa wordmarks partidos en spans (`<span class="a">AUTOS</span><span class="b">ALV</span>`) — revisar header **Y** footer a mano.

## ⚠️ Pendientes del usuario (no los puedo hacer yo)
- **Renovar el plan Cloud Professional** (vence **2026-07-07**; sin renovar, arcademotorsmx.com se cae con el plan).
- Cambiar contraseñas temporales (BD `Alv_Crt7xK2mPq9wR`, admin `Alv_Admin_2026`).
- (Email del dominio: ✅ ya verificado por el usuario.)

---

## Sesión 2026-06-24 — Catálogo + rebrand + migración
Catálogo ML completo (150/3011/8380) generado con Workflow multi-agente (1 agente/marca, 2 corridas). Cascade cableado en publicar + buscar (revisado con 2 rondas de Workflow adversarial: bug "estado fantasma" en filtros → fix). Rebrand **Autos ALV → Arcade Motors MX** (rebrand.py + edits manuales del logo/hero/boot). **Migración a arcademotorsmx.com**: sitio nuevo en hPanel (PHP/HTML personalizado), ZIP deploy, SSL, misma BD. Redirect alv.peptidosysuplementos.mx → arcademotors (301). 11 anuncios de prueba borrados (sitio arranca en 0). AdSense retirado de todo el sitio.

## Sesión 2026-06-25 — Cuentas + buscador + footer
**Buscador general** en el header (→`buscar.php?q=`). **Sistema de cuentas reales** (registro/login/sesión; `auth.php` autocrea la tabla `usuarios`; header con estado de sesión) en **verde fósforo `#7CFC6B`**. Confirmado end-to-end con el registro real del usuario (ANTONIO). Gotcha CSS: `.nav-login` perdía specificity contra `.header-nav a` → subido a `.header-nav a.nav-login`. **Footer rebrand fix:** el logo del footer seguía diciendo "AUTOS ALV" (spans que rebrand.py no atrapó) → corregido a "ARCADE MOTORS MX". **Breakthrough de deploy:** `form_input` llena los diálogos del filebrowser → ahora se crean archivos nuevos en el server sin intervención del usuario.
