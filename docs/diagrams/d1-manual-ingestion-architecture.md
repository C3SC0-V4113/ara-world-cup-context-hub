# Arquitectura D1 con Ingesta Manual

Estado: Proposed / no aprobado.

Esta propuesta asume login corporativo Microsoft como requisito transversal. La
ingesta deportiva principal es operada por admin. El LLM forma parte de la
arquitectura como asistencia para categorias subjetivas y narrativa, pero no como
fuente final de verdad.

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
    adminUi["Admin UI<br/>carga manual y revision"]
  end

  subgraph servicesSection["Servicios"]
    direction TB
    appServices["Servicios app<br/>predicciones y consulta"]
    roleGuard["Proteccion por rol<br/>user / admin en servidor"]
    adminServices["Servicios admin<br/>validacion, cierres, overrides"]
    scoringEngine["Scoring engine<br/>score events"]
    rankingEngine["Ranking engine<br/>snapshots"]
  end

  subgraph databaseSection["Base de datos"]
    direction TB
    d1Store[("Cloudflare D1<br/>fuente canonica")]
    adminOverrides["Admin overrides<br/>correcciones manuales"]
    scoreEvents["Score events<br/>ledger auditable"]
    rankingSnapshots["Ranking snapshots<br/>individual y grupo"]
    auditLog["Audit log<br/>acciones y recalculos"]
  end

  subgraph llmSection["LLM / IA asistiva"]
    direction TB
    llmProvider["LLM asistivo<br/>proveedor no aprobado"]
    llmDraft["Propuesta IA<br/>sorpresa, decepcion, narrativa"]
    adminReview["Revision admin<br/>aprueba o rechaza"]
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
  adminServices --> d1Store
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
  d1Store --> llmProvider
  llmProvider --> llmDraft
  llmDraft --> adminReview
  adminReview --> adminServices
  adminReview --> auditLog
  wranglerOps --> d1Store
  wranglerOps --> cloudflareDeploy
  cloudflareDeploy --> appShell

  classDef identityCls fill:#e8f4f8,stroke:#6c8ebf,color:#111827
  classDef frontendCls fill:#fff9e6,stroke:#d6b656,color:#111827
  classDef serviceCls fill:#f5f5f5,stroke:#666666,color:#111827
  classDef storeCls fill:#e8f5e9,stroke:#82b366,color:#111827
  classDef adminCls fill:#fff0e6,stroke:#d79b00,color:#111827
  classDef llmCls fill:#fce4ec,stroke:#b85450,color:#111827

  class msEntra,msGraphProfile,avatarFallback identityCls
  class appShell,userViews,adminUi frontendCls
  class appServices,roleGuard,adminServices,scoringEngine,rankingEngine serviceCls
  class d1Store,scoreEvents,rankingSnapshots storeCls
  class adminOverrides,auditLog,adminReview adminCls
  class llmProvider,llmDraft llmCls
```

## Costos estimados

Fecha de verificacion: 2026-06-04. Estos costos son referencia documental para
comparar opciones; no aprueban proveedor ni plan final.

| Recurso | Uso en esta propuesta | Costo base | Variable de consumo | Riesgo de costo |
| --- | --- | --- | --- | --- |
| Cloudflare Workers / Pages | Hosting de la app y servicios server-side. | Workers Paid referencia: USD 5/mes; assets estaticos sin cobro por request segun Workers pricing. | Requests dinamicos y CPU si se excede lo incluido. | Medio si scoring/recalculos o admin generan CPU alta. |
| Cloudflare D1 | Base canonica, predicciones, resultados, overrides, audit log y rankings. | Free: 5M rows read/dia, 100K rows written/dia, 5GB total. Paid: primeros 25B reads/mes, 50M writes/mes y 5GB incluidos. | Excedentes Paid: USD 0.001/M reads, USD 1.00/M writes, USD 0.75/GB-mes. | Bajo al inicio; sube si ranking/scoring hacen full scans sin indices. |
| Microsoft Entra ID / Graph | Login corporativo, nombre, correo y foto. | Depende del licenciamiento Microsoft 365 existente. | Scopes, permisos admin y llamadas Graph para foto/perfil. | Bajo para perfil; validar permisos del tenant y fallback de foto. |
| Microsoft Graph / Teams | Recordatorios o mensajes si se activan despues. | Microsoft indica que desde 2025-08-25 las APIs de Teams ya no son metered. | Otras APIs Graph metered ajenas a Teams pueden requerir Azure subscription. | Bajo para Teams basico; medio si se agregan APIs Graph protegidas o metered. |
| LLM asistivo | Proponer categorias subjetivas y narrativa para revision admin. | Proveedor no aprobado. OpenAI se usa solo como referencia: precios por 1M tokens segun modelo. | Tokens de entrada/salida por solicitud, contexto enviado y reintentos. | Medio; limitar contexto, registrar uso y evitar usarlo para datos objetivos. |
| Wrangler | Configuracion, migraciones, bindings y deploy. | Sin costo directo como CLI. | Depende de recursos Cloudflare operados. | Bajo; riesgo operativo si no se versiona configuracion. |

Fuentes: [Cloudflare D1 pricing](https://developers.cloudflare.com/d1/platform/pricing/),
[Cloudflare Workers pricing](https://developers.cloudflare.com/workers/platform/pricing/),
[Microsoft Graph metered APIs](https://learn.microsoft.com/en-us/graph/metered-api-list) y
[OpenAI API pricing](https://developers.openai.com/api/docs/pricing).
