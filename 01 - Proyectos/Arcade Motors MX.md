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
- Páginas: **index** (hero terminal), **buscar** (filtros catálogo), **anuncio** (?id), **publicar** (POST, cascade; **requiere login + correo verificado**), **planes**, **gestionar** (?token + **login + ser el dueño**), **registro / ingresar / cuenta / verificar** (cuentas de usuario)
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
- **Filebrowser por-sitio vs de cuenta (2026-06-26):** la tarjeta "Acceder a archivos de <sitio>" abre un filebrowser **SOLO LECTURA** (openresty da **403** en todo PUT → el editor Ace "guarda" sin persistir, engaña: `getValue` correcto pero live no cambia). El que **ESCRIBE** es "Accede a todos los archivos de Cloud Professional" (de cuenta), vía ruta `domains/arcademotorsmx.com/public_html/...`. Diagnóstico decisivo: `fetch` PUT directo a `…/<token>/api/resources/<ruta>?override=true` con header `X-Auth` (de `localStorage.jwt`) devuelve el código real (403 vs 200) en vez del fallo silencioso del editor.
- **Rate-limit del proxy:** abrir muchos tokens / muchos PUT seguidos hace que openresty devuelva **403 hasta en GET** un rato → espaciar las ops, o dejar que el usuario suba por File Manager.
- **Regla anti-clobber:** antes de re-subir un archivo COMPARTIDO entre sesiones (styles.css, header.php), hacer **diff local vs vivo** (`Get-FileHash` + `Compare-Object`) para no borrar el trabajo de otra sesión. (2026-06-26: local ⊇ vivo, seguro.)
- **`resize_window` de Chrome NO da viewport móvil real** (innerWidth se queda en 1440 → los `@media` no disparan) → verificar fixes móviles por código (regla en vivo + efecto en DOM) + prueba en el teléfono del usuario.

## ⚠️ Pendientes del usuario (no los puedo hacer yo)
- ✅ **Hosting Cloud Professional pagado hasta 2027-07-07** (verificado en hPanel 2026-06-27; la fecha "2026-07-07" era incorrecta — ya NO es pendiente urgente, >1 año de runway).
- Cambiar contraseñas temporales (BD `Alv_Crt7xK2mPq9wR`, admin `Alv_Admin_2026`).
- (Email del dominio: ✅ ya verificado por el usuario.)
- **Crear más correos de @arcademotorsmx.com**:
  - ✅ **`contacto@` RESUELTO (2026-06-29):** ya NO rebota. Configurado vía **alias `contacto@` → buzón `no-reply@`** (alias de email, 1/5 usado) + **reenviador `no-reply@` → gabo6999@gmail.com** (Activo, confirmado por el usuario; "Guardar copias" ON). Resultado: el correo a contacto@ llega al Gmail del usuario. *(Aprendizaje hPanel: el plan FREE Business Email permite hasta **100 buzones**, NO 1 como se creía. Un **reenviador exige un buzón YA existente como origen** → no se puede reenviar desde una dirección inexistente; por eso se usó alias hacia no-reply@. El reenviador a un correo externo queda en "Esperando confirmación" hasta que el dueño del destino hace clic en el email de Hostinger.)*
  - Otros recomendados (cuando se quiera): `redes@` (redes sociales), `soporte@`/`ayuda@` (soporte), `ventas@`/`lotes@` (B2B lotes/agencias), `hola@`/`info@` (genérico), `facturacion@` (Fase 2 cobros). Como hay 100 buzones libres, se pueden crear como buzones reales (requieren contraseña) o como alias del mismo no-reply@.

---

## Sesión 2026-06-24 — Catálogo + rebrand + migración
Catálogo ML completo (150/3011/8380) generado con Workflow multi-agente (1 agente/marca, 2 corridas). Cascade cableado en publicar + buscar (revisado con 2 rondas de Workflow adversarial: bug "estado fantasma" en filtros → fix). Rebrand **Autos ALV → Arcade Motors MX** (rebrand.py + edits manuales del logo/hero/boot). **Migración a arcademotorsmx.com**: sitio nuevo en hPanel (PHP/HTML personalizado), ZIP deploy, SSL, misma BD. Redirect alv.peptidosysuplementos.mx → arcademotors (301). 11 anuncios de prueba borrados (sitio arranca en 0). AdSense retirado de todo el sitio.

## Sesión 2026-06-25 — Cuentas + buscador + footer
**Buscador general** en el header (→`buscar.php?q=`). **Sistema de cuentas reales** (registro/login/sesión; `auth.php` autocrea la tabla `usuarios`; header con estado de sesión) en **verde fósforo `#7CFC6B`**. Confirmado end-to-end con el registro real del usuario (ANTONIO). Gotcha CSS: `.nav-login` perdía specificity contra `.header-nav a` → subido a `.header-nav a.nav-login`. **Footer rebrand fix:** el logo del footer seguía diciendo "AUTOS ALV" (spans que rebrand.py no atrapó) → corregido a "ARCADE MOTORS MX". **Breakthrough de deploy:** `form_input` llena los diálogos del filebrowser → ahora se crean archivos nuevos en el server sin intervención del usuario.

## Sesión 2026-06-25 (cont.) — Mis anuncios + incidente de token
- **"Mis anuncios" (cuenta funcional):** columna `anuncios.usuario_id` (auto-ALTER vía `anuncios_asegurar_usuario_id()` en auth.php, idempotente), publicar.php liga el anuncio a la cuenta si hay sesión, cuenta.php lista los anuncios del usuario (grid `car_card` + enlace "✎ Gestionar" → gestionar.php?token). Archivos: auth.php, publicar.php, cuenta.php, styles.css (`.mi-card`/`.mi-gestionar`).
- **⚠️ INCIDENTE de deploy:** el token del filebrowser **caduca en segundos** (muere al navegar directo a una URL de archivo profunda, p.ej. assets/css/, o tras 1-2 ops). Desplegando "Mis anuncios" el save de **publicar.php se truncó** con el token agonizando → publicar.php quedó **HTTP 500** en vivo (publishing roto). El home/buscar/login NO se afectaron (auth.php y header quedaron bien). **Fix:** re-desplegar los 4 archivos vía ZIP `arcade-mis-anuncios.zip`. **Aprendizaje:** para tandas de varios archivos NO confiar en direct-nav (token muere); usar UI del filebrowser (doble-clic la fila) o ZIP. Además los tabs del filebrowser abiertos por la tarjeta de hPanel a veces quedan FUERA del grupo MCP (no controlables).
- **FX visuales del home (sesión paralela, 2026-06-25):** 9 efectos CRT (power-on, boot terminal, piso synthwave, aberración VHS, íconos pixel, odómetro, badges/hover, loader cassette, sonido 8-bit con toggle) desplegados en `assets/css/fx.css` + `assets/js/fx.js` AISLADOS (cargados al final de footer.php, NO tocan header/styles → conviven con el buscador/cuentas). Detalle en la memoria del proyecto.

## Sesión 2026-06-26 — Pulido visual móvil
Cierres del pulido del home (todos EN VIVO, verificados en el teléfono del usuario):
- **Marcas populares → nombres en verde fósforo pixel** (`.brand-chip` en fx.css: mayúsculas + glow `#7CFC6B`). **SIN logos:** se descartó pixelar emblemas por riesgo de copyright/derivative de marcas registradas; el nombre como texto es uso nominativo (sin problema legal).
- **Íconos pixel de categorías:** confirmados funcionando (12 `.cat-card` con SVG pixel por carrocería; claves del mapa `ICONS` en fx.js == claves `$CARROCERIAS`). El "no se ven" era artefacto de encuadre del scroll, no un bug.
- **Hero "ARCADE MOTORS MX" se cortaba SIEMPRE en móvil:** el markup (`index.php`) usa `&nbsp;` entre palabras (espacios duros que impiden el salto de línea) → no cabía y se desbordaba. Fix en archivos aislados: **fx.js** convierte `&nbsp;`→espacio normal SOLO en los nodos de texto de `.bigwm` (conserva el `<span>` de MX) + **fx.css** `@media(max-width:600px)` baja tamaño y permite wrap. Desktop sin cambio. (Aprendizaje: CSS no puede romper un `&nbsp;` — hay que cambiarlo en JS o en el markup.)
- **Header se recortaba SIN sesión en móvil:** `.site-header` tenía altura FIJA (`height:var(--header-h)`) + `sticky`; al envolver el contenido a varias filas la nav se cortaba (peor sin sesión, con INGRESAR/REGISTRARTE más anchos). Fix en **styles.css** extendiendo `@media(max-width:760px)`: `.site-header{height:auto;min-height:var(--header-h);padding:8px 0}` + `flex-wrap`/`row-gap`/`gap`. La sesión del buscador YA terminó → OK tocar styles.css; antes de subir verifiqué diff local vs vivo (seguro). Verificado en cel con y sin sesión.

## Sesión 2026-06-26 (cont.) — Publicar fix, gestión segura, legales, verificación de correo
Detalle completo: [[2026-06-26 — Arcade Motors Publicar fix, Gestion segura, Verificacion correo]].
- **Fix publicar 500** (`catalog.php` viejo sin `anuncios_tiene_vin()`) + "Mis anuncios" (cuenta.php viejo). **🚀 Breakthrough deploy: API REST del filebrowser** (`/api/raw` + `/api/resources` con JWT de `localStorage`; escribir y borrar con header **`X-Auth`** —el DELETE con `?auth=` da 401—; ruta de dominio addon `domains/arcademotorsmx.com/public_html/`). String-replace quirúrgico por default; base64 para archivos nuevos.
- **Fotos:** fix EXIF (giro 90° de fotos de celular), reemplazo limpia filas `fotos`, límite 15 visible. **Galería + lightbox** en anuncio.php (vanilla JS/CSS, flechas/teclado/swipe).
- **Legales** (privacidad.php / terminos.php, deslinde fuerte: AMX = solo tablero de contacto) + **contacto gateado por registro** (anuncio visible sin login; WhatsApp solo logueado; `?volver=` regresa al anuncio).
- **gestionar.php rediseñado** (Editar / Eliminar; WhatsApp NO por anuncio) + **🔒 SEGURIDAD: login + ser el dueño** (`usuario_id` match; el token ya no basta). publicar.php exige login.
- **📧 Verificación de correo COMPLETA y EN VIVO:** SMTP Hostinger (cuenta `no-reply@` en plan FREE Business Email), columnas auto `email_verificado`+`verif_token`, **verificar.php** nueva, **publicar bloqueado hasta verificar**, banner en cuenta, reenviar. Cliente SMTP propio en auth.php (ssl://465, AUTH LOGIN; degrada sin romper si falta pass). **Probado: auth 235 / send 250.** Correo con Message-ID + multipart texto/HTML + **header de marca CSS** (renderiza hasta en spam). DNS: **SPF✅ DMARC✅ DKIM pendiente**. Spam de dominio nuevo = normal → marcar "No es spam".

## Sesión 2026-06-27 — KYC opcional, fotos, perfil v2, badge "Usuario verificado", favoritos
Detalle completo: [[2026-06-27 — Arcade Motors KYC opcional, fotos, perfil, favoritos]].
- **KYC teléfono → OPCIONAL:** Telcel filtra los SMS A2P de Firebase → la verificación de teléfono dejó de bloquear publicar (ahora es **badge de confianza**). **Blaze activado** ($5,204 en créditos de prueba; no cobra solo + alerta de presupuesto $5). WhatsApp volvió a **campo libre** en el perfil; publicar solo exige WhatsApp (verificarlo es opcional). WhatsApp OTP queda para cuando pase a cobros (lo más confiable en MX).
- **Nombre bloqueado** (seguridad/identidad anti-suplantación, debe coincidir con KYC/INE futuro); solo correo (no editable) y WhatsApp (editable).
- **Fotos:** badge "USADO" oculto (redundante, conserva Nuevo/Seminuevo/Destacado); **recorte centrado parejo** (`object-fit:cover` centro) en tarjetas; foto principal del anuncio **completa con relleno difuminado** (estilo Netflix); **tip de portada magenta** ("usa foto horizontal") en publicar + reemplazar fotos; botón "Elegir fotos" con estilo del tema. *(Gotcha: `fx.css` `display:grid` en `.thumb` rompía el `aspect-ratio:4/3` con fotos verticales → imgs en capa absoluta.)*
- **Perfil v2 (todo en cuenta.php, una página con cards):** Seguridad (**cambiar contraseña** + **eliminar cuenta** con zona de peligro roja), **"miembro desde"**, **estado / ciudad / tipo de vendedor** por defecto, **pre-llenado al publicar** (nombre/estado/ciudad), **stats** en "Mis anuncios" (👁 vistas + ● Activo/Vendido).
- **Badge "Usuario verificado"** (correo ✓ + tel ✓ — aplica a comprador o vendedor) en la tarjeta del vendedor del anuncio + tipo dinámico (Particular / Lote·Agencia).
- **Favoritos** para todos los usuarios: tabla `favoritos` + endpoint `favorito.php` (AJAX toggle) + página `favoritos.php` + corazón en `car_card` + link ♥ en header. Toggle en vivo; invitado → registro.
- **Alineación** de "Gestionar anuncio" (título a **2 líneas fijas** + tarjeta que crece y empuja el botón al fondo) vía **`amx-fix.css`** (override chico cargado tras styles.css, sin tocar el archivo grande).
- **⚠️ Lecciones de deploy:** token rate-limiteado devuelve **552 bytes = página de error** en TODOS los reads (NO es archivo roto; el vivo estaba intacto, verificar con `fetch` directo al sitio) → **nunca read-modify-write con un read de 552** (escribirías 552 y destruyes el archivo); `functions.php` usa **CRLF** (otros archivos LF) → anclas surgical con `\r\n`; drift local↔server (name-lock por 6 reemplazos en server vs rewrite limpio en local) → reemplazar funciones **por índice** (`indexOf`+`slice`); base64 grande puede corromperse al pegar (carácter cirílico) → preferir surgical/índice; detectar sitio correcto al reabrir token (un File Manager abrió en drbacilio.com por error — detectado antes de escribir).

## Próximos pasos / pendientes (dev)
- ✅ **RESUELTO (2026-06-26): el bug de publicar era el `catalog.php` del server VIEJO** (le faltaba `anuncios_tiene_vin()`, llamada dentro del POST → 500 solo al publicar; el GET cargaba bien). Re-subido el catalog.php local; verificado end-to-end.
- **Editar perfil (cuenta.php) + mover WhatsApp a nivel perfil** (hoy NO editable por anuncio, a propósito).
- ✅ **DKIM ACTIVADO (2026-06-26):** hPanel → Emails → arcademotorsmx.com → Mailboxes → Custom DKIM → "Generate DKIM record" (Hostinger lo agregó solo al DNS; TXT en `hostingermail1._domainkey` con llave RSA, verificado). Ya están **SPF✅ DKIM✅ DMARC✅**.
- **KYC** compradores/vendedores + **anti-abuso "sube y sube gratis"** (Fase 2): caducidad 30d + 1 gratis por identidad-KYC + 1 activo por VIN + límite por teléfono.
- **Verificación de correo por email** del usuario en registro (¿parte del KYC?) — la verificación de correo electrónico ya quedó hecha esta sesión.
- Sembrar **1–3 anuncios demo con foto** para ver los FX de tarjeta (hover sweep, badges, título verde, placeholder neón) y el odómetro — hoy invisibles con 0 anuncios.
