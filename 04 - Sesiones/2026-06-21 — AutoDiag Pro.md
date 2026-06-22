---
tags: [sesion, autodiag, 2026-06]
fecha: 2026-06-21 00:07
proyecto: AutoDiag Pro
---

# Sesión 2026-06-21 — AutoDiag Pro

Proyectos: [[AutoDiag Pro]]

## Qué se hizo
- Cargué contexto completo de AutoDiag Pro (v1.6, 21 pantallas, licenciamiento)
- Investigué compatibilidad ThinkDiag Mini III (TKD08): usa BLE propietario THINKCAR → no compatible
- El usuario instaló el driver J2534Plus de THINKCAR: DLL 32-bit, solo para SmartLink/SmartBox, no sirve para TKD08
- Identificamos el Vlinker FS BT como adaptador compatible → COM4, ELM327 v2.3 ✅
- Fix: baudrate BT subido de 9600 → 38400 en `hardware/elm327.py`
- Bug encontrado: clic en Conectar congelaba la app (`obd.OBD()` bloqueaba el UI thread)
- Fix aplicado: creé `_ConnectWorker(QThread)` en `ui/connection_widget.py` para mover la conexión al background

## Decisiones técnicas
- ThinkDiag Mini III TKD08 descartado: integración BLE requeriría reverse engineering con `bleak` (días de trabajo)
- Vlinker FS BT elegido como adaptador de prueba: funciona out-of-the-box en COM4
- Conexión movida a QThread para no bloquear UI; la barra de progreso aparece durante la espera
- `_conn_worker` guardado en `self` para evitar que el GC lo destruya antes de terminar

## Problemas encontrados
- ThinkDiag Mini III TKD08: no tiene driver J2534 para PC, BLE propietario, sin soporte Windows
- Driver J2534Plus THINKCAR: DLL 32-bit, Python 64-bit no puede cargarlo via ctypes
- App se trababa al dar clic en Conectar (`obd.OBD()` bloqueante en UI thread)

## Próximos pasos
- [ ] Probar conexión Vlinker FS BT completa con auto encendido (COM4 → Conectar → VIN)
- [ ] Deploy servidor licencias en Railway (pendiente desde v1.6)
- [ ] Modo Cliente: reporte simplificado sin jerga técnica
- [ ] Módulo de Cotización: logo + IVA + firma
