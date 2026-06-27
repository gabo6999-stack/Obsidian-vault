---
tags: [sesion, arcade-motors, kyc, fotos, perfil, favoritos]
fecha: 2026-06-27
proyecto: Arcade Motors MX
---

# 2026-06-27 — Arcade Motors: KYC opcional, fotos, perfil v2, badge, favoritos

Proyecto: [[Arcade Motors MX]] · Sesión larga. Todo **desplegado EN VIVO y verificado**.

## 1. KYC de teléfono → OPCIONAL (Firebase + Blaze)
- Se construyó la verificación de teléfono con **Firebase Phone Auth** (widget compat 10.12.2 → `getIdToken` → PHP `verificar_firebase_token()` valida el RS256 contra los certs x509 de Google). **Probado end-to-end con número de prueba.**
- **Pero Telcel filtra los SMS A2P de Firebase/Google** (problema real en MX) → los SMS reales no llegan. Además **SMS real requiere Blaze** (tarjeta; `auth/billing-not-enabled` en Spark).
- **Activamos Blaze:** la cuenta de facturación es la **prueba gratuita de Google Cloud** ($5,204 USD en créditos, 90 días, NO cobra automáticamente al terminar) + alerta de presupuesto de $5. Costo del SMS = centavos → ≈ $0 con los créditos.
- **Decisión:** la verificación de teléfono es **OPCIONAL** (solo badge de confianza, NO bloquea publicar). WhatsApp volvió a **campo libre** en el perfil; publicar solo exige tener un WhatsApp. **WhatsApp OTP** queda para cuando pase a cobros (lo más confiable en MX, ~$0.03–0.05/OTP, pero setup pesado).
- **Nombre bloqueado** por seguridad (identidad anti-suplantación, encaja con KYC/INE futuro): en cuenta.php es solo-lectura; `usuario_actualizar_perfil` ignora el nombre.

## 2. Fotos (mucho más estético)
- **Badge "USADO" oculto** (era la mayoría → redundante; conserva Nuevo/Seminuevo/Destacado).
- **Recorte centrado parejo** en las tarjetas (`object-fit:cover` con `object-position:center`) → grid simétrico, el coche centrado.
- **Foto principal del anuncio = completa** con **relleno difuminado** (copia borrosa de fondo, estilo Netflix) → no recorta, se ve premium.
- **Tip de portada magenta neón** ("la 1ª foto es la portada; mejor horizontal") en **publicar** y **reemplazar fotos**.
- Botón nativo "Elegir archivos" → **botón temático verde "📷 Elegir fotos"**.
- 🐛 **Gotcha:** `fx.css` ponía `display:grid` en `.thumb` → rompía el `aspect-ratio:4/3` con fotos verticales (tarjetas disparejas). Fix: imágenes en **capa absoluta** (`position:absolute`).
- 💡 Guardado para después: auto-centrado con `smartcrop` (gratis) y auto-edición con IA (`rembg` + fondo studio branded) cuando haya pagadores.

## 3. Perfil v2 (todo en cuenta.php)
- **Seguridad:** cambiar contraseña (verifica la actual) + **eliminar cuenta** (borra anuncios/fotos + cierra sesión, doble confirmación, zona de peligro roja).
- **"Miembro desde {mes año}"** en el saludo.
- **Estado / Ciudad / Tipo de vendedor** (Particular / Lote·Agencia) por defecto en el perfil.
- **Pre-llenado al publicar:** nombre + estado + ciudad se llenan solos desde el perfil (el WhatsApp ya se mostraba).
- **Stats en "Mis anuncios":** 👁 vistas + badge ● Activo / ● Vendido por tarjeta.

## 4. Badge "Usuario verificado"
- = correo `email_verificado` **Y** teléfono `telefono_verificado` (aplica a comprador o vendedor). Chip verde junto al nombre del vendedor en el anuncio + tipo dinámico.

## 5. Favoritos (todos los usuarios)
- Tabla `favoritos` + `includes/favoritos.php` (`favorito_toggle`, `favoritos_ids`) + endpoint **`favorito.php`** (AJAX POST → JSON) + página **`favoritos.php`** ("Mis favoritos") + **corazón** en `car_card` (top-right) + JS en footer (globals `AMX_LOGGED`/`AMX_FAVS`) + link **♥** en el header.
- Toggle en vivo sin recargar; invitado → registro. Probado end-to-end (agregar/listar/quitar, 0 fatales).

## 6. Alineación de "Gestionar anuncio"
- Títulos largos (Ford Fusion Hybrid Titanium, 2 líneas + "Automática") desalineaban el botón. Fix: **título a 2 líneas fijas** (`-webkit-line-clamp:2` + `min-height`) + **`.mi-card .car-card{flex:1}`** (la tarjeta crece, empuja el botón al fondo). Los 3 botones quedaron en la misma Y exacta.
- Hecho vía **`amx-fix.css`** (CSS chico cargado tras styles.css → override seguro, sin reescribir el archivo de 23KB).

## ⚠️ Lecciones de deploy (importantes)
- **Token del filebrowser rate-limiteado → devuelve ~552 bytes (página de error) en TODOS los `/api/raw` reads + 403 en writes.** NO confundir con "el archivo mide 552 / está roto": el archivo vivo está intacto (verificar con `fetch` directo a `arcademotorsmx.com/...`). **PELIGRO:** read-modify-write con un read de 552 → escribes 552 y **destruyes** el archivo. Guardar siempre el write tras un guard; si el read da 552, reabrir token fresco.
- **Line endings mixtos:** `includes/functions.php` usa **CRLF**, otros archivos LF → anclas surgical deben usar `\r\n`. Síntoma: `match:false` inexplicable.
- **Drift local↔server:** el name-lock de una función se hizo en el server por 6 reemplazos (quedó distinto al rewrite limpio del local) → la siguiente surgical no machó. Solución robusta: **reemplazar funciones completas por índice** (`indexOf('function X')`…`indexOf(siguiente)` + `slice`).
- **Base64 grande puede corromperse al pegar** (apareció un carácter cirílico → `atob` falló) → preferir surgical/índice para archivos grandes.
- **Detectar el sitio correcto al reabrir token:** un File Manager abrió en **drbacilio.com** por error (public_html vacío) — detectado leyendo un archivo marcador **antes de escribir**. Listar root → `public_html` → prefix.
- El token muere si su pestaña se cierra o pasa rato inactiva → en sesiones largas, reabrir.

## Pendientes para la próxima
- ⚠️ **Renovar hosting Cloud Professional** (vence 2026-07-07).
- WhatsApp OTP (al pasar a cobros), **KYC INE (Fase B)**, anti-abuso "sube y sube gratis", auto-edición de fotos con IA, badge en las tarjetas del grid (no solo en el detalle).
