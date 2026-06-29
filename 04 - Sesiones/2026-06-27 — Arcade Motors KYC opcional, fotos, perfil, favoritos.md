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
- ✅ **Hosting Cloud Professional pagado hasta 2027-07-07** (verificado en hPanel 2026-06-27; la fecha 2026-07-07 era incorrecta — ya no es pendiente urgente).
- WhatsApp OTP (al pasar a cobros), **KYC INE (Fase B)**, anti-abuso "sube y sube gratis", auto-edición de fotos con IA.

## 7. Badge "✓ Verificado" en las tarjetas del grid (2026-06-27, EN VIVO ✓)
- Helper `usuario_esta_verificado(int $uid)` en functions.php (caché estática por request, 1 query por dueño) + chip verde **"✓ Verificado"** en la esquina inferior-izq de la foto del `car_card` (CSS `.seller-verif` en amx-fix.css). Misma lógica que el detalle: `email_verificado && telefono_verificado`.
- **Verificado por DOM en anónimo:** 3 chips, `rgba(25,160,100,.92)`, `bottom/left:10px`, fuente VT323.
- **Deploy:** read-modify-write **dentro de la página** del filebrowser (fetch GET `/api/raw` → string-replace idempotente → POST `/api/resources?override=true` con `X-Auth`), todo con el JWT sin salir del navegador → UTF-8 (emojis/acentos) intacto, 0 corrupción base64. **Clave:** `base = location.pathname.split('/files')[0]` (el prefijo `/<token>`), sin él da 403/552.
- ⚠️ **A decidir:** como el teléfono es opcional y Telcel filtra el SMS, casi nadie tendrá el badge a futuro (la cuenta del usuario lo tiene de las pruebas). Opciones: dejarlo "premium", cambiarlo a solo-correo, o reservarlo al KYC INE.

## 8. Fix buscador: marca de primer nivel (2026-06-27, EN VIVO ✓)
- **Bug (lo notó el usuario):** "Marcas populares" → `buscar.php?marca=Ford` mostraba los 3 anuncios, no filtraba a Ford.
- **Causa:** en `$cat_mode`, la sanitización de cascada borraba `$f_marca` cuando no venía `categoria` (los links de marca no la traen).
- **Fix:** `cat_todas_las_marcas()` en catalog.php + en buscar.php la marca se valida contra TODAS si no hay categoría, y el select de Marca queda siempre habilitado. **Verificado:** `?marca=Ford` → 1 resultado, dropdown "Ford" seleccionado. Cascada intacta.
- **Hosting:** de paso confirmé en hPanel que Cloud Professional vence **2027-07-07** (no 2026) → corregido en las notas, ya no es pendiente urgente.

## 9. Badge movido a la derecha del precio + lección de deploy (2026-06-27, EN VIVO ✓)
- A pedido del usuario, el chip "✓ Verificado" se **movió de la esquina de la foto al renglón del precio**, alineado al borde derecho (`$159,000 ⟷ ✓ Verificado`). `.car-card .price{display:flex}` + chip `margin-left:auto` en amx-fix.css; el span pasó del thumb al `<div class="price">`.
- **🧨 Lección cara:** el filebrowser **per-sitio** ("Acceder a archivos de <sitio>") tiene **READS STALE/cacheados** — devolvía functions.php=9200 / amx-fix.css=472 (viejos) aunque mis writes SÍ habían pegado. Me hizo creer que "no persistía" y casi reescribo una versión de functions.php SIN el helper (= fatal). **Fix:** usar el filebrowser **de CUENTA** ("Accede a todos los archivos de Cloud Professional", prefijo `domains/arcademotorsmx.com/public_html/`) = reads frescos + writes confiables. Verificar SIEMPRE por la ruta de cuenta o `fetch` directo con cache-bust, nunca por el read-back del per-sitio.

## 10. Badge a verde fósforo CRT + grilla "Explora por tipo" (2026-06-27, EN VIVO ✓)
- El chip "✓ Verificado" se reestilizó a **verde fósforo `#7CFC6B`** con fondo gradiente verde oscuro + borde + glow (igual que el botón INGRESAR, `.btn-login`). Verificado por `getComputedStyle`.
- Grilla de "Explora por tipo": de `auto-fill` (salían 7+5) → forzada a columnas fijas vía `.cat-grid` en amx-fix.css. Primero 6+6 (12 tipos), luego **4+4** al pasar a 8 clasificaciones.

## 11. "Explora por tipo" → 8 clasificaciones de vehículo + íconos 8-bit (2026-06-27→28, EN VIVO ✓)
- Reemplazados los 12 tipos de carrocería por las **8 clasificaciones** (= `cat_categorias`): Autos y Camionetas · Motos · Vehículos Pesados · Camiones · Otros Vehículos · Autos de Colección · Náutica · Colectivos y Autobuses. Array `$CLASIFICACIONES` en data.php; `index.php` itera con `data-ico` + `?categoria=`. **Clic → `buscar.php?categoria=X` ya filtrado + dropdown en ese default** (idea de navegación del usuario). Validado: "Autos y Camionetas" → 3 resultados.
- **Íconos pixel-art:** objeto `ICONS` en `fx.js` (SVG `crispEdges`, `fill=currentColor`); cambié la extracción del slug a `data-ico`. 8 íconos nuevos; **Motos = versión "B"** (relleno: 2 ruedas + asiento + manubrio en T) elegida por el usuario tras 2 iteraciones.
- ⚠️ Nombres en `cat_categorias.nombre` van **sin acento** → el `?categoria=` usa el valor BD; el tile muestra el acento aparte. (Pendiente opcional: corregir BD a con-acento, y renombrar título "Explora por tipo" → "Explora por categoría".)
- **🔑 Lección de deploy:** tras escribir un archivo, el `/api/raw` del filebrowser **congela su lectura** (read-after-write stale). Para leer fresco y hacer surgical replace → **abrir un token NUEVO** del filebrowser de cuenta (su 1ª lectura es fresca). Así se desbloqueó el cambio de Motos. Los WRITES sí pegan siempre.
