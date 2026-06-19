---
tags: [sesion, ptm, ptm-novo, legal, monetizacion, 2026-06]
fecha: 2026-06-19 16:29
proyecto: grupoptm.com
---

# Sesión 2026-06-19 — grupoptm: Modelo de Monetización (telemedicina legítima)

Proyectos: [[grupoptm.com]] · [[PTM Novo]] · [[Ecosistema PTM-PYS]]

## Contexto
Pivote estratégico: legitimar PTM como **portal de telemedicina** (médicos mexicanos con cédula profesional, TRH + péptidos) y dejarlo **fuera de la cadena de venta** de productos. Disparado por pregunta sobre costo/permisos de un anuncio en Meta y dudas de COFEPRIS.

## Qué se hizo
- Se diseñó el **modelo de monetización** que mantiene a PTM como "plataforma de software + servicio médico" y fuera de la cadena de venta.
- Se documentó todo en el repo `ecommerce-agent`: **`MODELO_MONETIZACION_PTM.md`** (commit `138b7f9`, rama `claude/practical-edison-3qhcvw`).
- Se definió el flujo de agendamiento + pago con retención y la política de cancelación.

## Decisiones tomadas
- **PTM no vende, almacena ni surte productos.** Solo conecta paciente↔médico y cobra por la consulta + uso de plataforma. El paciente surte su receta donde quiera; ese dinero **nunca toca a PTM**.
- **Precio de consulta: $1,500 MXN** → $1,000 al médico / **$500 comisión PTM**.
- La comisión ($500) es **fija por consulta**, nunca % ni atada al producto recetado. *(Prueba: existiría aunque se receten cero productos.)*
- **PTM no factura al paciente.** El médico le factura al paciente (si lo pide); PTM factura su comisión de $500 al médico → cliente fiscal de PTM = el médico.
- **Pago con retención:** se cobra $1,500 al agendar pero el split se libera **al completarse la consulta** (el dinero cae al médico que realmente atendió, cero clawback).
- **Asignación automática de médico** al agendar fecha/hora (round-robin / por especialidad) + bloqueo del slot.
- **Política de cancelación:**
  - Paciente cancela o **no acude** → se cobra, **sin reembolso**; el médico cobra igual (apartó su hora). *(Requiere checkbox de aceptación visible — PROFECO.)*
  - Médico **no se presenta** → se **reagenda** (no reembolso); pago retenido hasta que un médico complete.
- **Stack de pago objetivo:** Stripe Connect (Express accounts), `application_fee = $500`, transferencia diferida.

## ⚠️ Cambio importante vs. modelo anterior (revisar en [[PTM Novo]])
El modelo viejo registrado en [[PTM Novo]] **contradice** lo definido hoy:
| | Modelo viejo (PTM Novo) | Modelo nuevo (esta sesión) |
|---|---|---|
| Consulta | $500 | **$1,500** |
| Split | $400 PTM / $100 médico | **$1,000 médico / $500 PTM** |
| Producto | lo cobra [[PYS Ecommerce]] (~$1,500/mes) = **cadena de venta** | PTM **fuera** de la venta; paciente surte donde quiera |
- **Tensión legal a resolver:** si PYS (entidad hermana del [[Ecosistema PTM-PYS]]) es quien vende el producto, la defensa de "PTM es solo plataforma" se **debilita** (riesgo de "favorecimiento" por parte relacionada). Esto debe entrar al dictamen del abogado.

## Próximos pasos
- [ ] **Paso 0 — Brief para abogado regulatorio sanitario (COFEPRIS):** validar el modelo y resolver el tema de **péptidos no registrados** (que un médico los recete no los vuelve legales de surtir). Crear `BRIEF_ABOGADO_PTM.md`.
- [ ] Reconciliar el modelo de [[PTM Novo]] con el nuevo (números $1,500/$1,000/$500, separar PYS de la cadena, o blindar la relación PTM↔PYS con el abogado).
- [ ] Borradores para revisión del abogado: Términos, Aviso de privacidad (LFPDPPP), Consentimiento informado de teleconsulta.
- [ ] Definir con contador: facturación por cuenta de terceros (mandato/comisión) o split en pasarela.
- [ ] Esbozar módulo técnico: datos (médicos+cédula, disponibilidad, citas, pagos retenidos), agendamiento+asignación, Stripe Connect, panel médico (reagendar/completar).
- [ ] Publicidad Meta: landing que vende la **consulta**, no el producto (sin claims).
- [ ] Decidir: ¿la plataforma vive en el repo `ecommerce-agent` (`web.py`) o proyecto nuevo?

## Referencias
- Documento maestro: `ecommerce-agent/MODELO_MONETIZACION_PTM.md` (commit `138b7f9`)
- Relacionados: [[grupoptm.com]] · [[PTM Novo]] · [[Ecosistema PTM-PYS]] · [[PYS Ecommerce]]
