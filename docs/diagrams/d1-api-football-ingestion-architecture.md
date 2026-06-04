# Arquitectura D1 con Ingestion API-Football Free

Estado: Accepted.

Esta arquitectura asume login corporativo Microsoft como requisito transversal.
API-Football Free es el proveedor deportivo inicial; OpenFootball puede alimentar
seed estatico. D1 es la fuente canonica interna.

```mermaid
flowchart LR
  subgraph identitySection["Identidad"]
    direction TB
    msEntra["Microsoft Entra ID<br/>login corporativo"]
    msGraphProfile["Microsoft Graph<br/>nombre, correo, foto"]
    avatarFallback["Avatar fallback<br/>iniciales si no hay foto"]
  end

  subgraph frontendSection["Frontend"]
    direction TB
    appShell["App web en Cloudflare<br/>Next.js runtime compatible"]
    userViews["Vistas usuario<br/>home, predicciones, ranking"]
    freshnessUi["Frescura visible<br/>actualizado hace X"]
    adminUi["Admin UI<br/>conflictos y overrides"]
  end

  subgraph externalApiSection["APIs externas"]
    direction TB
    openFootballSeed["OpenFootball<br/>seed gratuito opcional"]
    apiFootballProvider["API-Football Free<br/>100 requests/dia"]
    quotaGuard["Control de cuota<br/>80 sync + 20 reserva"]
  end

  subgraph ingestionSection["Ingestion y normalizacion"]
    direction TB
    initialImport["Import inicial<br/>teams y matches"]
    syncWorker["Sync worker / polling<br/>cada 60 min en ventana"]
    providerMappings["Provider mappings<br/>IDs externos -> internos"]
    rawPayloads["Payloads y sync runs<br/>trazabilidad y debug"]
    normalizeData["Normalizacion<br/>eventos, standings, estadisticas"]
  end

  subgraph servicesSection["Servicios"]
    direction TB
    appServices["Servicios app<br/>predicciones y consulta"]
    roleGuard["Proteccion por rol<br/>acciones admin en servidor"]
    scoringEngine["Scoring engine<br/>score events"]
    rankingEngine["Ranking engine<br/>individual y grupos"]
  end

  subgraph databaseSection["Base de datos"]
    direction TB
    d1Store[("Cloudflare D1<br/>canonico normalizado")]
    providerConflicts["Provider conflicts<br/>API vs D1/admin"]
    adminOverrides["Admin overrides<br/>correcciones manuales"]
    scoreEvents["Score events<br/>ledger auditable"]
    rankingSnapshots["Ranking snapshots<br/>individual y grupos"]
    auditLog["Audit log<br/>syncs, overrides y recalculos"]
  end

  subgraph opsSection["Operacion Cloudflare"]
    direction TB
    wranglerOps["Wrangler<br/>migraciones y bindings"]
    cloudflareDeploy["Cloudflare<br/>deploy primario"]
  end

  msEntra --> appShell
  msGraphProfile --> appShell
  msGraphProfile --> avatarFallback
  avatarFallback --> appShell
  appShell --> userViews
  appShell --> adminUi
  userViews --> appServices
  appServices --> freshnessUi
  adminUi --> roleGuard
  roleGuard --> adminOverrides
  roleGuard --> auditLog
  openFootballSeed --> initialImport
  apiFootballProvider --> syncWorker
  quotaGuard --> syncWorker
  initialImport --> providerMappings
  syncWorker --> rawPayloads
  syncWorker --> providerMappings
  providerMappings --> normalizeData
  rawPayloads --> normalizeData
  normalizeData --> d1Store
  normalizeData --> providerConflicts
  providerConflicts --> adminUi
  adminOverrides --> d1Store
  d1Store --> appServices
  appServices --> d1Store
  d1Store --> scoringEngine
  scoringEngine --> scoreEvents
  scoreEvents --> d1Store
  scoringEngine --> auditLog
  d1Store --> rankingEngine
  rankingEngine --> rankingSnapshots
  rankingSnapshots --> d1Store
  wranglerOps --> d1Store
  wranglerOps --> cloudflareDeploy
  cloudflareDeploy --> appShell

  classDef identityCls fill:#e8f4f8,stroke:#6c8ebf,color:#111827
  classDef frontendCls fill:#fff9e6,stroke:#d6b656,color:#111827
  classDef apiCls fill:#e8f4f8,stroke:#6c8ebf,color:#111827
  classDef serviceCls fill:#f5f5f5,stroke:#666666,color:#111827
  classDef storeCls fill:#e8f5e9,stroke:#82b366,color:#111827
  classDef adminCls fill:#fff0e6,stroke:#d79b00,color:#111827

  class msEntra,msGraphProfile,avatarFallback identityCls
  class appShell,userViews,freshnessUi,adminUi frontendCls
  class openFootballSeed,apiFootballProvider,quotaGuard apiCls
  class initialImport,syncWorker,providerMappings,rawPayloads,normalizeData,appServices,roleGuard,scoringEngine,rankingEngine serviceCls
  class d1Store,scoreEvents,rankingSnapshots storeCls
  class providerConflicts,adminOverrides,auditLog adminCls
```

## Costos y limites estimados

Fecha de verificacion documental: 2026-06-04. Estos costos son referencia; los
planes pueden cambiar.

| Recurso | Uso | Costo base | Variable de consumo | Riesgo |
| --- | --- | --- | --- | --- |
| Cloudflare Workers / Pages | Hosting, servicios y sync workers. | Workers Paid referencia: USD 5/mes. | Requests dinamicos, cron/polling y CPU. | Medio si sync o recalculos son frecuentes. |
| Cloudflare D1 | Fuente canonica, cache normalizado, sync runs, conflicts y ranking. | Free: 5M reads/dia, 100K writes/dia, 5GB total. | Reads/writes y storage de payloads. | Medio si payloads no tienen retencion. |
| API-Football Free | Fixtures, resultados, eventos, estadisticas y standings disponibles. | USD 0/mes. | 100 requests/dia. | Alto si polling excede cuota o falta cobertura. |
| OpenFootball | Seed inicial gratuito. | USD 0. | Sin live API. | Bajo; requiere mapeo contra IDs internos/API-Football. |
| Microsoft Graph | Login corporativo, perfil, correo y avatar. | Depende del licenciamiento Microsoft 365 existente. | Llamadas Graph para perfil/foto. | Bajo; validar scopes y fallback de foto. |

Fuentes: [Cloudflare D1 pricing](https://developers.cloudflare.com/d1/platform/pricing/),
[Cloudflare Workers pricing](https://developers.cloudflare.com/workers/platform/pricing/),
[API-Football pricing](https://www.api-football.com/pricing) y
[Microsoft Graph metered APIs](https://learn.microsoft.com/en-us/graph/metered-api-list).
