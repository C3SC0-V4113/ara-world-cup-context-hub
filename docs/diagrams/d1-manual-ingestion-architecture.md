# Arquitectura D1 con Ingesta Manual

Estado: Proposed / no aprobado.

```mermaid
flowchart LR
  admin["Admin UI<br/>carga equipos, partidos,<br/>resultados y reglas"]
  adminSvc["Servicios admin<br/>validacion, overrides,<br/>audit log"]
  ai["IA asistiva<br/>sorpresa, decepcion,<br/>narrativa"]
  review["Revision admin<br/>aprueba o rechaza"]
  d1[("Cloudflare D1<br/>fuente canonica")]
  scoring["Scoring engine<br/>score events"]
  ranking["Ranking engine<br/>snapshots"]
  views["Vistas app<br/>home, predicciones,<br/>ranking, admin"]

  admin --> adminSvc
  adminSvc --> d1
  d1 --> ai
  ai --> review
  review --> adminSvc
  d1 --> scoring
  scoring --> d1
  d1 --> ranking
  ranking --> d1
  d1 --> views

  classDef source fill:#e8f4f8,stroke:#6c8ebf,color:#111827
  classDef store fill:#e8f5e9,stroke:#82b366,color:#111827
  classDef adminCls fill:#fff0e6,stroke:#d79b00,color:#111827
  classDef aiCls fill:#f5f5f5,stroke:#666666,color:#111827

  class admin,adminSvc,review adminCls
  class d1 store
  class ai aiCls
  class scoring,ranking,views source
```

