---
tags: [sesion, raditech, agente-seo, 2026-06]
fecha: 2026-06-09 23:12
proyecto: Raditech + Agente SEO
---

# Sesion 2026-06-09 - Raditech GSC + Redirects

## Que se hizo
- Diagnosticado problema raiz de GSC Raditech: las 3 funciones usaban el mismo GOOGLE_REFRESH_TOKEN de PYS con site_url distinto
- Implementado token OAuth separado para Raditech (RADITECH_GSC_REFRESH_TOKEN)
- Modificado get_gsc_service() y fetch_gsc_data() para aceptar refresh_token opcional
- Agregados endpoints /search-console/raditech/auth y /search-console/raditech/callback
- Registrada nueva redirect URI en Google Cloud Console (OAuth client de PYS)
- Generado y configurado RADITECH_GSC_REFRESH_TOKEN en Railway via CLI
- Ejecutado analisis GSC completo de raditech.mx: top queries, CTR opportunities, page performance
- Verificados 4 redirects 301 del usuario (todos correctos)
- Agregado redirect landing hospital-elipse a home (landing retirada definitivamente)

## Decisiones tecnicas
- Token separado por sitio GSC, misma app OAuth (mismo CLIENT_ID/SECRET, distinto refresh token)
- gsc_inspect_url pendiente de actualizar para Raditech (hardcoded a site_url de PYS)
- Indexacion de sistema-his-medsi solicitada manualmente (SA sin acceso a raditech.mx en GSC)

## Problemas encontrados
- redirect_uri_mismatch: Google tardo varios minutos en propagar URI
- OAuth error app not verified: cuenta Raditech no estaba en test users
- Primer token Raditech era de cuenta incorrecta (403), regenerado con cuenta correcta
- railway link no configurado en ecommerce-agent, vinculado manualmente a proyecto Agente-SEO

## Proximos pasos
- [ ] Monitorear consolidacion de autoridad en URLs nuevas (redirects activos desde hoy)
- [ ] Confirmar indexacion de sistema-his-medsi en 1-2 semanas
- [ ] Crear landing hbg-lomas-verdes como caso de exito (400+ impresiones sin pagina)
- [ ] Crear contenido no-branded: sistema pacs ris mexico, teleradiologia 24/7
- [ ] Remover referencias a Grupo PTM en SEO title de la home de Raditech
- [ ] Mejorar gsc_inspect_url para soportar Raditech (parametro site_url opcional)