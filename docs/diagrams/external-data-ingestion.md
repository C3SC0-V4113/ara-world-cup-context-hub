# Propuesta de Ingestion de Datos Externos

Estado: propuesta no aprobada. Este flujo visualiza ADR-0008 y no representa una
decision vigente de implementacion.

```mermaid
flowchart LR
  ext["APIs externas y datasets<br/>football-data.org, API-Football,<br/>OpenFootball, Sportmonks"]
  graph["Microsoft Graph<br/>perfil, foto, Teams"]
  sync["Ingestion / sync<br/>jobs, server actions o workers"]
  normalize["Normalizacion<br/>IDs internos, UTC, estados"]
  d1[("Cloudflare D1<br/>fuente canonica")]
  services["Servicios de app<br/>predicciones, scoring, ranking"]
  views["Vistas<br/>home, predicciones, ranking, admin"]
  admin["Admin<br/>correcciones y cierres"]
  ai["IA asistiva<br/>categorias ambiguas y narrativa"]
  approval["Validacion admin<br/>resultado final subjetivo"]

  ext --> sync
  graph --> sync
  sync --> normalize
  normalize --> d1
  d1 --> services
  services --> views
  views --> admin
  admin -->|"override gana sobre API"| d1
  d1 --> ai
  ai --> approval
  approval --> d1

  classDef source fill:#e8f4f8,stroke:#6c8ebf,color:#111827
  classDef store fill:#e8f5e9,stroke:#82b366,color:#111827
  classDef admin fill:#fff0e6,stroke:#d79b00,color:#111827
  classDef ai fill:#f5f5f5,stroke:#666666,color:#111827

  class ext,graph source
  class d1 store
  class admin,approval admin
  class ai ai
```
