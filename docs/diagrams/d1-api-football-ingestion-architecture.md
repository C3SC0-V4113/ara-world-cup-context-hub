# Arquitectura D1 con Ingestion API-Football

Estado: Proposed / no aprobado.

```mermaid
flowchart LR
  openfootball["OpenFootball<br/>seed gratuito"]
  apiFootball["API-Football<br/>fixtures, live, events,<br/>statistics, top scorers"]
  graph["Microsoft Graph<br/>identidad, avatar,<br/>Teams"]
  import["Import inicial<br/>seed teams/matches"]
  sync["Sync worker / polling<br/>cuotas, retries, backoff"]
  normalize["Normalizacion<br/>provider IDs -> IDs internos"]
  d1[("Cloudflare D1<br/>canonico normalizado")]
  conflicts["Provider conflicts<br/>API vs D1/admin"]
  admin["Admin UI<br/>overrides y resolucion"]
  services["Servicios app<br/>predicciones, scoring,<br/>ranking"]
  views["Vistas app<br/>home, predicciones,<br/>ranking"]

  openfootball --> import
  import --> normalize
  apiFootball --> sync
  sync --> normalize
  graph --> normalize
  normalize --> d1
  normalize --> conflicts
  conflicts --> admin
  admin -->|"override gana"| d1
  d1 --> services
  services --> d1
  services --> views

  classDef source fill:#e8f4f8,stroke:#6c8ebf,color:#111827
  classDef store fill:#e8f5e9,stroke:#82b366,color:#111827
  classDef adminCls fill:#fff0e6,stroke:#d79b00,color:#111827
  classDef syncCls fill:#f5f5f5,stroke:#666666,color:#111827

  class openfootball,apiFootball,graph source
  class d1 store
  class admin,conflicts adminCls
  class import,sync,normalize,services,views syncCls
```

