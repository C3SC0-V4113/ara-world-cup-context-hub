# Arquitectura D1 con Fallback Manual/Admin

Estado: fallback operativo. La arquitectura objetivo aceptada usa ingestion con
API-Football Free. Este flujo conserva el camino manual para overrides,
recuperacion operativa y correcciones admin.

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
    adminUi["Admin UI<br/>overrides y correcciones"]
  end

  subgraph servicesSection["Servicios"]
    direction TB
    appServices["Servicios app<br/>predicciones y consulta"]
    roleGuard["Proteccion por rol<br/>user / admin en servidor"]
    adminServices["Servicios admin<br/>validacion y overrides"]
    scoringEngine["Scoring engine<br/>score events"]
    rankingEngine["Ranking engine<br/>snapshots"]
  end

  subgraph databaseSection["Base de datos"]
    direction TB
    d1Store[("Cloudflare D1<br/>fuente canonica")]
    adminOverrides["Admin overrides<br/>correcciones manuales"]
    scoreEvents["Score events<br/>ledger auditable"]
    rankingSnapshots["Ranking snapshots<br/>individual y grupos"]
    auditLog["Audit log<br/>acciones y recalculos"]
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
  adminUi --> roleGuard
  roleGuard --> adminServices
  appServices --> d1Store
  d1Store --> appServices
  adminServices --> adminOverrides
  adminOverrides --> d1Store
  adminServices --> auditLog
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
  classDef serviceCls fill:#f5f5f5,stroke:#666666,color:#111827
  classDef storeCls fill:#e8f5e9,stroke:#82b366,color:#111827
  classDef adminCls fill:#fff0e6,stroke:#d79b00,color:#111827

  class msEntra,msGraphProfile,avatarFallback identityCls
  class appShell,userViews,adminUi frontendCls
  class appServices,roleGuard,adminServices,scoringEngine,rankingEngine serviceCls
  class d1Store,scoreEvents,rankingSnapshots storeCls
  class adminOverrides,auditLog adminCls
```

## Uso esperado

- Correcciones manuales cuando API-Football falle o entregue datos discutibles.
- Recuperacion operativa si se agota la cuota free.
- Recalculos o overrides administrativos.
- No incluye LLMs ni resoluciones subjetivas.
