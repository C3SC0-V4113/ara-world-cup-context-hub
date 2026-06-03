# Flujo de Scoring y Ranking

```mermaid
flowchart LR
  U["Usuarios"] --> GP["Predicciones generales"]
  U --> DP["Predicciones diarias"]
  U --> EX["Prediccion extra diaria"]

  T["Partidos y resultados"] --> S["Motor de scoring"]
  R["Reglas de puntuacion"] --> S
  A["Resoluciones admin"] --> S
  GP --> S
  DP --> S
  EX --> S

  S --> L["Ledger o puntos calculados"]
  L --> RI["Ranking individual"]
  L --> RA["Ranking por area"]
  RI --> G["Narrativa de gamificacion"]
  RA --> G
  G --> H["Home y ranking"]
```
