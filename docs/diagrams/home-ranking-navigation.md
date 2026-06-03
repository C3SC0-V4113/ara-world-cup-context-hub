# Navegacion Home, Ranking y Gamificacion

```mermaid
flowchart TD
  H["Home"] --> P["Predicciones"]
  H --> R["Ranking"]
  H --> C["Calendario"]
  H --> F["Mis selecciones favoritas"]
  H --> B["Teams bot"]

  P --> PG["Predicciones generales"]
  P --> PD["Predicciones del dia"]
  R --> RI["Ranking individual completo"]
  R --> RA["Ranking por area"]
  R --> T3["Top 3 destacado"]
  R --> M["Movimientos recientes"]

  F --> H
  PG --> R
  PD --> R
  B --> H
  C --> H
```
