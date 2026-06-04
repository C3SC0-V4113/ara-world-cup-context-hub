# Flujo de Scoring y Ranking

```mermaid
flowchart LR
  users["Usuarios"] --> generalPredictions["Predicciones generales<br/>campeon, finalistas,<br/>clasificados, mas goles,<br/>mejor defensa"]
  users --> dailyPredictions["Predicciones del dia<br/>marcador, ganador,<br/>empate, tarjetas, tiros"]

  d1Store[("Cloudflare D1<br/>predicciones, datos API,<br/>reglas y overrides")]
  importedData["Datos normalizados<br/>API-Football Free"]
  adminOps["Admin protegido por rol<br/>overrides y recalculos"]
  auditLog["Audit log<br/>cambios y recalculos"]
  scoringEngine["Motor de scoring<br/>aplica reglas vigentes"]
  scoreEvents["Score events<br/>ledger auditable"]
  rankingEngine["Motor de ranking"]
  individualRanking["Ranking individual"]
  normalizedGroupRanking["Ranking grupos<br/>promedio activo"]
  rawGroupRanking["Ranking grupos<br/>suma bruta"]
  rankingSnapshots["Ranking snapshots<br/>estado consultable"]
  gamification["Gamificacion<br/>top 3, movimientos,<br/>log de puntos"]
  views["Home y ranking"]

  generalPredictions --> d1Store
  dailyPredictions --> d1Store
  importedData --> d1Store
  adminOps --> d1Store
  adminOps --> auditLog
  d1Store --> scoringEngine
  scoringEngine --> scoreEvents
  scoreEvents --> d1Store
  scoringEngine --> auditLog
  d1Store --> rankingEngine
  rankingEngine --> individualRanking
  rankingEngine --> normalizedGroupRanking
  rankingEngine --> rawGroupRanking
  individualRanking --> rankingSnapshots
  normalizedGroupRanking --> rankingSnapshots
  rawGroupRanking --> rankingSnapshots
  rankingSnapshots --> d1Store
  rankingSnapshots --> gamification
  scoreEvents --> gamification
  gamification --> views
```
