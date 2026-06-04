# Propuesta de Ingestion de Datos Externos

Estado: propuesta no aprobada. Este flujo visualiza ADR-0008 y no representa una
decision vigente de implementacion.

```mermaid
flowchart LR
  subgraph identitySection["Identidad"]
    direction TB
    microsoftIdentity["Microsoft Entra ID / Graph<br/>login, perfil, foto"]
    avatarFallback["Avatar fallback<br/>si no hay foto"]
  end

  subgraph externalSourcesSection["APIs y datasets externos"]
    direction TB
    sportsSources["Datos deportivos<br/>football-data.org, API-Football,<br/>OpenFootball, Sportmonks"]
    quotaWatch["Control de cuotas<br/>costos, limites, alertas"]
  end

  subgraph ingestionSection["Ingestion y normalizacion"]
    direction TB
    syncJobs["Ingestion / sync<br/>Workers, jobs o server actions"]
    normalizeData["Normalizacion<br/>IDs internos, UTC, estados"]
    providerTrace["Trazabilidad<br/>sync runs, payloads, mapeos"]
  end

  subgraph databaseSection["Base de datos"]
    direction TB
    d1Store[("Cloudflare D1<br/>fuente canonica interna")]
    auditLog["Audit log<br/>cambios y resoluciones"]
    adminOverrides["Admin overrides<br/>correcciones manuales"]
  end

  subgraph servicesSection["Servicios"]
    direction TB
    appServices["Servicios de app<br/>predicciones, scoring, ranking"]
    roleGuard["Proteccion por rol<br/>acciones admin en servidor"]
    assistiveAi["IA asistiva<br/>categorias ambiguas y narrativa"]
  end

  subgraph frontendSection["Frontend"]
    direction TB
    appViews["Vistas<br/>home, predicciones, ranking, admin"]
    adminUi["Admin<br/>correcciones, cierres, overrides"]
    adminApproval["Validacion admin<br/>resultado final subjetivo"]
  end

  microsoftIdentity --> avatarFallback
  microsoftIdentity --> appServices
  sportsSources --> syncJobs
  quotaWatch --> syncJobs
  syncJobs --> normalizeData
  syncJobs --> providerTrace
  normalizeData --> d1Store
  providerTrace --> d1Store
  d1Store --> appServices
  appServices --> d1Store
  appServices --> appViews
  appViews --> adminUi
  adminUi --> roleGuard
  roleGuard --> adminOverrides
  adminOverrides --> d1Store
  roleGuard --> auditLog
  d1Store --> assistiveAi
  assistiveAi --> adminApproval
  adminApproval --> d1Store
  adminApproval --> auditLog

  classDef identityCls fill:#e8f4f8,stroke:#6c8ebf,color:#111827
  classDef apiCls fill:#e8f4f8,stroke:#6c8ebf,color:#111827
  classDef serviceCls fill:#f5f5f5,stroke:#666666,color:#111827
  classDef storeCls fill:#e8f5e9,stroke:#82b366,color:#111827
  classDef adminCls fill:#fff0e6,stroke:#d79b00,color:#111827
  classDef frontendCls fill:#fff9e6,stroke:#d6b656,color:#111827

  class microsoftIdentity,avatarFallback identityCls
  class sportsSources,quotaWatch apiCls
  class syncJobs,normalizeData,providerTrace,appServices,roleGuard,assistiveAi serviceCls
  class d1Store storeCls
  class auditLog,adminOverrides,adminUi,adminApproval adminCls
  class appViews frontendCls
```
