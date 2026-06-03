# Mapa del Repo de Aplicacion

Repo de referencia: `E:\Repositorios\ara-world-cup`

Este hub no implementa la app. El repo hermano se usa solo para contrastar estado
actual contra la intencion de producto y las recomendaciones tecnicas.

## Documentos existentes en la app

- `README.md`: resumen de stack, arquitectura local y comandos.
- `DESIGN.md`: estandar visual y design system.
- `PRD-predicciones-mundial.md`: requerimiento funcional ya derivado de la
  reunion y del estado del codigo.
- `docs/adr/`: ADRs tecnicos de stack, testing, design system y server actions.

## Vistas principales

- `app/page.tsx` y `components/home/HomeDashboard.tsx`: home con resumen, partidos
  de hoy, favoritos, predicciones, ranking, resultados y banner de Teams.
- `app/predicciones/page.tsx` y `components/predicciones/PredictionsBoard.tsx`:
  predicciones generales, predicciones del dia, reglas de puntos y bloqueo visual.
- `app/ranking/page.tsx` y `components/ranking/RankingBoard.tsx`: ranking
  individual, ranking por area, top 3, movimientos y badges.
- `app/admin/page.tsx` y `components/admin/AdminBoard.tsx`: operacion admin.

## Servicios y reglas

- `lib/services/predictions.ts`: lectura y guardado de predicciones.
- `lib/services/scoring.ts`: servicios de scoring.
- `lib/services/ranking.ts`: ranking individual y por area.
- `lib/services/admin.ts`: settings, reglas y acciones admin.
- `lib/predictionRules.ts`: bloqueo por partido y calculo parcial de puntos
  diarios.
- `lib/scoring.ts`: tabla visible de reglas de puntuacion.

## Modelo de datos actual

La migracion `supabase/migrations/202605280001_initial_schema.sql` ya incluye:

- `users`
- `teams`
- `matches`
- `general_predictions`
- `daily_predictions`
- `daily_extra_predictions`
- `scoring_rules`
- `admin_settings`
- `activity_log`
- preferencias de favoritos y notificaciones

## Brechas principales

- Scoring completo de predicciones generales.
- Resolucion administrable de categorias subjetivas.
- Modelo explicito para penales.
- Desempates para partido con mas goles del dia.
- Ranking auditable con desglose de puntos.
- Remplazo de usuario placeholder por autenticacion real.
- Definicion final de areas/equipos internos para ranking grupal.

## Relacion entre repos

- El repo de aplicacion conserva decisiones tecnicas de implementacion.
- Este hub conserva contexto de producto, decisiones funcionales y memoria
  compartida para agentes.
- Si una decision funcional del hub cambia la arquitectura de la app, debe
  considerarse un ADR tecnico en el repo de aplicacion.
