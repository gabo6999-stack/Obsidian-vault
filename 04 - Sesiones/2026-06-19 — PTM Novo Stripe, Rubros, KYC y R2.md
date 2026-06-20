---
tags: [sesion, ptm-novo, telemedicina, nextjs, stripe, kyc, cloudflare-r2, 2026-06]
fecha: 2026-06-19
proyecto: PTM Novo
---

# Sesión 2026-06-19 — PTM Novo: Stripe, Rubros, KYC y Almacenamiento R2

Proyectos: [[PTM Novo]]

## Contexto
Sesión larga de implementación en el repo **PTM-NOVO** (Next.js 16 + Prisma/Postgres en Railway, dominio `ptm-novo-production.up.railway.app`). Se configuró Stripe Connect de punta a punta, se construyó el sistema de rubros de terapia del médico, la credencialización (KYC médico) y se conectó el almacenamiento de documentos a Cloudflare R2. Todo en **modo test** — nada cobra de verdad hasta tener el dictamen legal COFEPRIS.

## Qué se hizo

### 1. Stripe Connect (test) — configurado y verificado E2E
- Cuenta Stripe **GrupoPTM** (entorno de prueba); Connect habilitado con **modelo de negocio Paciente → PTM → Médico** (separate charges & transfers; la plataforma cobra y retiene, luego transfiere $1,000 al médico).
- Variables en Railway: `STRIPE_SECRET_KEY` (sk_test), `STRIPE_WEBHOOK_SECRET` (whsec), `NEXTAUTH_URL`.
- Webhook **"PTM Novo - pagos consulta"** → `/api/payments/webhook`, escucha solo `checkout.session.completed`.
- **Prueba real:** checkout $1,500 → tarjeta `4242` → `/pago-exitoso` → webhook entregado **200** → `Payment` HELD creado. ✅
- Borradas variables vestigiales de Railway: `PHARMACY_API_*` (línea roja) y `MP_ACCESS_TOKEN` (Mercado Pago). `WHEREBY_API_KEY` se queda (video).
- Ajuste: `connect/onboard` pre-llena `business_profile` (url + descripción + MCC 8011) para que Stripe **no pida sitio web** a médicos nuevos.
- **Onboarding de médico** Connect Express probado E2E → médico quedó **Activo** (recibe $1,000).

### 2. Corrección de cálculos al split fijo
Quedaban restos del viejo modelo 80/20 en 3 pantallas → corregido a **$500 PTM / $1,000 médico**: `admin/medicos`, `admin` (dashboard) y `doctor` (dashboard).

### 3. Rubros de terapia del médico (4)
- Enum **`TherapyArea`**: `WEIGHT_LOSS`, `PEPTIDES_LONGEVITY`, `MENS_HEALTH` (TRH), `WOMENS_HEALTH`. Distinto del enum `Program` (paciente).
- El médico elige rubros al alta en `/admin/medicos`; chips en la tabla. Fuente única: `src/lib/therapyAreas.ts`.
- **Editor de médicos** `/admin/medicos/[id]` (nombre, cédula, especialidad, bio, rubros, activo).
- **Auto-edición con aviso:** `/doctor/perfil` ("Mi perfil") el médico solicita cambio → `pendingTherapyAreas` + `areasReviewPending`; el admin Aprueba/Rechaza.

### 4. Credencialización (KYC médico)
- Modelo **`DoctorDocument`** (tipos `OFFICIAL_ID`, `MEDICAL_DEGREE`, `SPECIALTY_DEGREE`, `DIPLOMA`) + `Doctor.verificationStatus` (NOT_SUBMITTED / PENDING_REVIEW / APPROVED / REJECTED) + notas.
- El médico sube documentos en `/doctor/perfil` y envía a revisión; el admin revisa documento por documento (Aprobar/Rechazar) + verificación global en `/admin/medicos/[id]`. Badge de verificación en la tabla.

### 5. Almacenamiento R2 (Cloudflare) — conectado y verificado
- `src/lib/storage.ts` usa **Cloudflare R2** (API S3-compatible, `@aws-sdk/client-s3`). `PutObject` sube a bucket **privado** `ptm-novo-docs`; `getDocumentUrl` devuelve **URL firmada de 5 min** para el admin. Init perezosa (sin `R2_*` cae a placeholder).
- Variables en Railway: `R2_ACCOUNT_ID=39a308c4365f64e40afd383ebd5affa9`, `R2_ACCESS_KEY_ID`, `R2_SECRET_ACCESS_KEY`, `R2_BUCKET=ptm-novo-docs`. Token Account API "Object Read & Write" scoped al bucket, TTL forever, sin IP filter.
- `next.config.ts`: `serverActions.bodySizeLimit = 10mb` (para subir PDFs/fotos).
- **Prueba real:** médico subió imagen → guardada en R2 → admin abrió el link firmado (se ve la foto) → aprobar documento + aprobar verificación → médico **Verificado**. ✅

## Decisiones / hallazgos clave
- Para crear cuentas Connect, Stripe exige **completar el alta de Connect + elegir el modelo de negocio** (la opción correcta es Paciente→PTM→Médico). Sin eso, `accounts.create` falla con *"You can only create new accounts if you've signed up for Connect"*.
- El pre-llenado de `business_profile` solo aplica a cuentas Connect creadas **después** del deploy, no a las previas.
- El sistema de archivos de Railway es **efímero** → los documentos van obligatoriamente a almacenamiento externo (R2). Bucket **privado** + URLs firmadas; nunca acceso público.
- Documentos subidos con el stub (antes de R2) quedan como placeholder sin archivo → recargar.

## Commits clave (main, repo PTM-NOVO)
- `954267d` Stripe Connect (migración desde Mercado Pago) + onboarding pagos
- `58e153e` pre-llenar `business_profile` (no pedir sitio web)
- `5c23c35` fix split fijo $500/$1000 en 3 pantallas
- `16401d0` rubros de terapia (alta) · `1fb0f61` editor de médicos · `ad50b46` auto-edición con aprobación
- `ec0ea26` KYC médico (documentos + verificación) · `4d92e3d` almacenamiento R2

## Médicos de prueba
`agavito@ptmnovo.mx`, `antonio2@ptmnovo.mx`, `medico3@ptmnovo.mx` (ANTONIO3 — verificado).

## Pendientes
- [ ] **Gatear asignación** de pacientes solo a médicos **APPROVED + con el rubro aprobado**.
- [ ] **Linkear rubros con el quiz** (mapear el resultado del quiz → `TherapyArea`).
- [ ] Probar el ciclo **`consulta → completar → transfer $1,000`** al médico activo.
- [ ] Pasar a **llaves Stripe LIVE** — solo con el **dictamen legal COFEPRIS firmado**.
- [ ] BUG conocido: confirmar que no queden otras pantallas con el viejo modelo 80/20.
