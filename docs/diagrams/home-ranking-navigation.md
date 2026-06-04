# Navegacion Home, Ranking y Gamificacion

```mermaid
flowchart TD
  H["Home<br/>resumen y redireccion"] --> P["Predicciones"]
  H --> R["Ranking"]
  H --> C["Calendario / partidos"]
  H --> F["Mis selecciones favoritas"]
  H --> N["Notificaciones<br/>deshabilitadas"]

  P --> PD["Tab principal<br/>Predicciones del dia"]
  P --> PG["Tab secundario<br/>Predicciones generales"]

  R --> RI["Ranking individual"]
  R --> RGN["Ranking grupos<br/>normalizado"]
  R --> RGB["Ranking grupos<br/>sin normalizacion"]
  R --> DP["Desempeno personal<br/>log de puntos"]
  R --> T3["Top 3 destacado"]
  R --> M["Movimientos recientes"]
  R --> FS["Filtros y sort"]

  F --> H
  PD --> R
  PG --> R
  C --> H
```
