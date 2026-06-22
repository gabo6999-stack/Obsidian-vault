---
tags: [proyecto, autodiag, pyqt6, python, obd]
estado: v1.6 funcional
ruta: C:\Users\gabom\autodiag\
---

# AutoDiag Pro

> Aplicación de escritorio para diagnóstico automotriz profesional. Versión actual: **v1.6**

## Descripción

App PyQt6 para diagnóstico OBD-II/CAN de vehículos. Incluye escáner DTC, datos en vivo, predicción de fallas con IA, y módulos avanzados de programación/reprogramación.

## Stack

| Componente | Tecnología |
|-----------|-----------|
| UI | PyQt6 |
| IA | Claude API (`ai/analyzer.py`, `ai/predictor.py`) |
| Hardware | OBD-II / CAN FD |
| VIN | NHTSA API (`ai/nhtsa.py`) |
| Licencias | Servidor Flask en Railway |
| Base de datos | Local (`database/`) |
| Reportes | PDF (`reports/`) |
| Compilación | `python -m PyInstaller` (siempre con -m, nunca `pyinstaller` directo) |

## Módulos UI (21+ pantallas)

Los principales: `main_window.py`, `dashboard_widget.py`, `enhanced_scanner_widget.py`, `dtc_widget.py`, `live_data_widget.py`, `prediction_widget.py`, `analysis_center_widget.py`, `programming_widget.py`, `module_reprog_widget.py`, `license_dialog.py`

## Sistema de Licencias

| Estado | Comportamiento |
|--------|---------------|
| `licensed` | Badge activo |
| `trial` | Banner con días restantes |
| `expired` | Diálogo bloqueante |

Servidor de licencias: [[Servidor Licencias AutoDiag]]

## Estado Actual

| Tarea | Estado |
|-------|--------|
| App v1.6 funcional | ✅ |
| Predicción Fallas IA | ✅ |
| Sistema de licencias | ✅ |
| Servidor licencias Railway | ⏳ Pendiente deploy |
| Fix UI freeze en Conectar | ✅ (2026-06-21) |
| Adaptador probado: Vlinker FS BT (COM4) | ✅ |

## Adaptadores compatibles

| Adaptador | Estado | Puerto |
|-----------|--------|--------|
| Vlinker FS BT | ✅ Funciona | COM4, ELM327 v2.3 |
| ThinkDiag Mini III TKD08 | ❌ No compatible | BLE propietario |

## Relacionados

- [[Servidor Licencias AutoDiag]]
- [[Gloria IEM AI]] — otro proyecto PyQt6

---

## Sesión 2026-06-21
Investigamos compatibilidad de adaptadores OBD: ThinkDiag Mini III TKD08 descartado (BLE propietario, sin driver J2534 para PC). Vlinker FS BT funciona en COM4 con ELM327 v2.3. Fix crítico: la app se trababa al conectar porque `obd.OBD()` bloqueaba el UI thread — solucionado con `_ConnectWorker(QThread)` en `connection_widget.py`.

## Sesión 2026-06-21 (J2534 + Hardware)
Investigamos adaptadores J2534 para lectura+escritura real. Descartamos clon Dan&Dre ($409 MXN). Usuario compró OBDLink EX oficial en Amazon MX ($1,668 MXN, llega ~2026-06-24). Corregidos 3 bugs en `hardware/j2534.py`: CAN ID 2→4 bytes (crítico), WOW6432Node registry, ctypes.WinDLL.
