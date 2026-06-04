# Summary para CTO: Decisiones de Arquitectura Ara World Cup

## Estado

Accepted.

Este documento resume las decisiones aprobadas para orientar implementacion y
discusion tecnica posterior. No reemplaza los ADRs; los agrupa para lectura
ejecutiva.

## Resumen ejecutivo

La arquitectura vigente de Ara World Cup queda definida asi:

- **Cloudflare D1** como base de datos objetivo y fuente canonica interna.
- **Wrangler** para configuracion, migraciones y bindings.
- **Cloudflare** como despliegue primario de la web.
- **Microsoft Entra ID / Graph** para identidad, perfil y avatar.
- **API-Football Free** como proveedor deportivo inicial.
- **OpenFootball** como seed estatico opcional.
- **Sin LLMs** en esta version.
- **Notificaciones/Teams bot fuera de scope**.

Frontend puede consultar backend con frecuencia, pero backend es el unico que
habla periodicamente con API-Football. La mayoria de datos vive en D1 y las
vistas deben mostrar frescura cuando usen datos importados.

## Decisiones aceptadas

| Area | Decision |
| --- | --- |
| Datos deportivos | API-Football Free + OpenFootball seed opcional + D1 como fuente interna. |
| Predicciones | Dos tabs: `Predicciones del dia` principal y `Predicciones generales` secundario. |
| Scoring global | Globales reforzadas: campeon 25, finalista 15, mas goles 12, mejor defensa 12, clasificados 2 por acierto. |
| Scoring por partido | Mas mecanicas simples: marcador, ganador, empate, ambos anotan, tarjetas, tiros al arco y penales. |
| Ambiguas | Seleccion sorpresa y seleccion decepcion fuera de alcance. |
| Ranking | Individual, grupos normalizados por promedio activo y grupos sin normalizacion. |
| Home | Redirige a Predicciones; no guarda quick actions. |
| Notificaciones | Fuera de scope; items relacionados quedan deshabilitados. |

## Scoring aprobado

### Predicciones globales

| Mecanica | Puntos | Nota |
| --- | ---: | --- |
| Campeon acertado | 25 | Dato objetivo. |
| Finalista acertado | 15 | Por finalista acertado si se modela multiple. |
| Seleccion con mas goles | 12 | Derivada de goles acumulados. |
| Mejor defensa | 12 | Metrica compuesta simple. |
| Clasificados desde grupos | 2 por acierto | Equipos que avanzan a la siguiente ronda eliminatoria. |
| Seleccion sorpresa | Fuera de alcance | Ambigua. |
| Seleccion decepcion | Fuera de alcance | Ambigua. |

### Predicciones por partido

| Mecanica | Puntos |
| --- | ---: |
| Marcador exacto | 5 |
| Ganador acertado | 3 |
| Empate acertado | 3 |
| Ambos equipos anotan | 1 |
| Al menos una tarjeta roja | 1 |
| Rango de tarjetas amarillas totales | 1 |
| Rango de tiros al arco totales | 1 |
| Ganador por penales en eliminatorias, si aplica | 2 |

## Arquitectura API-Football Free

- Limite externo: 100 requests/dia.
- Recomendacion operativa: 80 requests automaticas/dia y 20 en reserva.
- Sync por defecto cada 60 minutos en ventanas de partido.
- Reconciliacion post-partido para resultados, eventos y standings.
- Guardar datos normalizados en D1 antes de UI, scoring o ranking.
- Mostrar `synced_at` o "actualizado hace X" en marcadores y datos importados.
- Admin overrides ganan sobre datos del proveedor y quedan auditables.

## Impactos de implementacion

- Migrar schema hacia D1 con tablas canonicas e ingestion API-Football.
- Ajustar Predicciones a tabs y quitar categorias sorpresa/decepcion.
- Actualizar reglas visibles y motor de scoring.
- Implementar score events y ranking snapshots.
- Implementar rankings por grupo normalizado y sin normalizacion.
- Deshabilitar controles de notificacion/Teams.

## Material de soporte

- [ADR-0008](../adr/0008-prefer-traditional-apis-for-verifiable-football-data.md)
- [ADR-0009](../adr/0009-propose-rebalancing-predictions-ux-and-daily-scoring.md)
- [ADR-0010](../adr/0010-propose-normalized-group-ranking-and-personal-performance-view.md)
- [ADR-0011](../adr/0011-propose-teams-reminders-and-home-reminder-flow.md)
- [ADR-0012](../adr/0012-propose-d1-schema-options-for-manual-and-api-ingestion.md)
- [Schema D1](../architecture/d1-schema-options.md)
- [APIs tradicionales y fuentes de datos](../integrations/football-data-apis.md)
- [Arquitectura D1 con API-Football](../diagrams/d1-api-football-ingestion-architecture.md)
