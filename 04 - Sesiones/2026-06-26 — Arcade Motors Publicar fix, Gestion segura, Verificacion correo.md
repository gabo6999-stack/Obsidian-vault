---
fecha: 2026-06-26
tags: [sesion, arcade-motors, marketplace, php, hostinger, smtp, seguridad]
proyecto: "[[Arcade Motors MX]]"
---

# 2026-06-26 — Arcade Motors: Publicar fix, gestión segura, verificación de correo

Sesión larga sobre **[[Arcade Motors MX]]**. Cierre del flujo publicar → gestionar → legales → contacto, y construcción completa de **verificación de correo por SMTP**.

## 🐛 Fix del bug "no se puede publicar" (HTTP 500)
- **Causa raíz:** el `includes/catalog.php` DEL SERVER estaba viejo — le faltaba la función `anuncios_tiene_vin()`. `publicar.php` la llama **dentro del bloque POST** → el GET cargaba el form pero el POST fatalaba (`Call to undefined function`). (Secuela del truncado de la sesión previa.)
- **Fix:** re-subir el `catalog.php` local. Verificado end-to-end (POST → INSERT → redirect a gestionar?nuevo=1).
- También estaba viejo `cuenta.php` (sin "Mis anuncios") → re-desplegado. **Lección: tras un ZIP/restore, verificar versión de CADA archivo crítico vs local.**
- Corrección a un diagnóstico previo: **el usuario de BD SÍ tiene ALTER** (la columna `usuario_id` se creó sola); "Mis anuncios" funciona.

## 🚀 BREAKTHROUGH de deploy — API REST del filebrowser (usar SIEMPRE)
Se acabó pelear con el editor Ace y los tokens. Desde la pestaña del filebrowser (JWT en `localStorage.jwt`), vía `fetch` same-origin:
- **Leer:** `GET /api/raw/<ruta>?auth=<jwt>` (texto) · `GET /api/resources/<ruta>?auth=<jwt>` (listado JSON `.items[]`).
- **Escribir:** `POST /api/resources/<ruta>?override=true` con header `X-Auth:<jwt>` + body → **200**. UTF-8: PowerShell base64 → `decodeURIComponent(escape(atob(b64)))`.
- **Borrar:** `DELETE /api/resources/<ruta>` con header **`X-Auth`** → **204** (¡OJO! con `?auth=` query da **401**; el DELETE necesita el header).
- **Ruta (dominio addon):** arcademotorsmx.com vive en `domains/arcademotorsmx.com/public_html/...` con el token "Access all files of Cloud Professional"; con el token por-sitio ("Access files of arcademotorsmx.com") la raíz es `public_html/` directo.
- **String-replace quirúrgico por default** (read → `.replace()` en el navegador → write) para ahorrar tokens; base64 completo solo para archivos nuevos/grandes.
- **El token CADUCA / se rate-limitea** (403 hasta en el navigate) tras ~6-10 escrituras seguidas → reabrir token fresco: hPanel → Websites → arcademotorsmx.com → **Dashboard → Files → File Manager** → tarjeta de las 2 (sitio vs cuenta). **Sondear el 403 con curl NO sirve** (sin JWT siempre da 403); verificar solo con un GET autenticado desde la pestaña.

## 📸🖼️ Fotos, galería y UX del anuncio
- **EXIF:** `guardar_fotos()` no leía `Orientation` → fotos de celular giradas 90°. Fix con `exif_read_data()` + `imagerotate()` antes de redimensionar.
- **Reemplazar fotos:** `borrar_fotos_de()` solo hacía `@unlink` pero no borraba filas de `fotos` → quedaban duplicadas. Fix: `DELETE FROM fotos`. Botón "Reemplazar fotos" en gestionar.php.
- **Límite visible:** 15 fotos / 8 MB c/u, con contador en vivo.
- **Galería + lightbox** en anuncio.php: vanilla JS/CSS (sin librerías), flechas ❮❯ + contador, click → fullscreen con teclado ←/→/Esc + swipe. Lee las URLs del DOM (no toca el markup).
- Beep del home −80% (`master.gain` 0.85→0.17). Quitado "EXPLORAR" de cuenta y del header.
- Tabla de specs más compacta (padding/line-height/max-width) + fix doble-espacio en descripción (`nl2br` + `white-space:pre-line` duplicaba saltos).

## ⚖️ Legales + 🔒 gating de contacto
- **privacidad.php** + **terminos.php** (tema CRT). Términos = deslinde fuerte: AMX es **solo tablero de contacto**, NO parte de las operaciones, NO responsable de fraudes; "reclamaciones contra la otra parte, nunca contra AMX ni su titular". Footer cableado.
- **Contacto gateado por registro:** el anuncio se ve completo sin login, pero el botón WhatsApp solo aparece logueado; sin sesión → "Regístrate para ver el contacto" (`registro.php?volver=...` → regresa al anuncio). Nota de aceptación de T&C en registro/publicar. (KYC = futuro.)

## 🛠️🔒 Gestión del anuncio — rediseño + SEGURIDAD
- Rediseño de **gestionar.php**: Resumen → **Editar** (precio/km/versión/transmisión/.../descripción; marca/modelo/año FIJOS) → Fotos → Estado → **Zona de peligro = Eliminar**. WhatsApp **NO** editable por anuncio (irá en el perfil).
- **SEGURIDAD (a pedido del usuario): el token ya NO basta.** `gestionar.php` ahora exige **LOGIN + ser el DUEÑO** (`anuncios.usuario_id == usuario_actual()`; si no, "este anuncio no es tuyo"; sin sesión → ingresar.php?volver=...). `publicar.php` exige **LOGIN** (→ registro.php?volver=) para que cada anuncio tenga dueño. Anuncios legacy sin dueño se "reclaman" para el 1er logueado con el token. Verificado en vivo los 4 casos.

## 📧 Verificación de correo (NUEVO, completo y EN VIVO)
Decisión del usuario: **SMTP de Hostinger** (no `mail()`) + **bloquear publicar hasta verificar**.
- **Mecánica:** columnas auto `usuarios.email_verificado`(def 0)+`verif_token`; registro → manda correo + lleva a `verificar.php?registrado`; **publicar bloqueado** si `!usuario_verificado()` → `verificar.php?pendiente`; banner en cuenta; `verificar.php?token=` confirma; reenviar por POST. Página **verificar.php** nueva.
- **Cliente SMTP propio** en `auth.php` (`enviar_correo_smtp()`: sockets `ssl://smtp.hostinger.com:465`, AUTH LOGIN; **degrada a false si `SMTP_PASS` vacío, NO rompe el sitio**). Constantes `SMTP_*` en config.php.
- **Cuenta:** `no-reply@arcademotorsmx.com` en plan **FREE Business Email** (no hay que pagar; basta para SMTP). El usuario creó la cuenta y puso la pass en `SMTP_PASS` (yo no manejo contraseñas).
- **✅ ENVÍO REAL CONFIRMADO** vía diagnóstico (ya borrado): handshake perfecto — greet 220, ehlo 250, **auth 235**, from/rcpt 250, data 354, **send 250**.

## 📨 Deliverability (correo nuevo → spam al inicio = NORMAL)
- DNS verificado: **SPF ✅** (`include:_spf.mail.hostinger.com`), **DMARC ✅** (`p=none`), **DKIM ❌** (no aparece en selectores estándar; activar en hPanel — pendiente opcional). El correo SÍ autentica (SPF→DMARC pasa).
- **Mejoras al correo:** + header `Message-ID` (su ausencia = señal fuerte de spam), + `Reply-To`, + **multipart/alternative** (texto plano auto + HTML; antes HTML-only → Outlook lo convertía a texto plano feo). + **encabezado de marca CSS** (barra negra + "🏎️ ARCADE MOTORS MX" neón ámbar/magenta) que **renderiza incluso en spam** (es CSS, no imagen; imágenes se bloquean en la carpeta de spam).
- **Acción del usuario para salir de spam:** marcar **"No es un correo no deseado"** → mueve a bandeja + reactiva enlaces + entrena el filtro. Verificado: el correo renderiza precioso y ya deja marcarlo.

## ⏭️ Pendientes (próximas sesiones)
- **Editar perfil (cuenta.php)** + **mover WhatsApp a nivel perfil** (no por anuncio).
- **DKIM** (activar en hPanel para reforzar entrega).
- **KYC** compradores/vendedores.
- **Anti-abuso "sube y sube gratis"** (Fase 2): caducidad 30d + 1 gratis por identidad-KYC + 1 activo por VIN + límite por teléfono.
- Standing: renovar **Cloud Professional** (vence 2026-07-07); cambiar contraseñas temporales.
