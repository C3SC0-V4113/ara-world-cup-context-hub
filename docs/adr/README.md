# Architecture Decision Records

Registro de decisiones funcionales y de arquitectura de producto para Ara World
Cup.

## Convencion

- Archivos `NNNN-titulo-corto.md` con numeracion secuencial de 4 digitos.
- Estados: `Proposed`, `Accepted`, `Superseded`, `Deprecated`, `Rejected`.
- Cada ADR debe incluir estado, contexto, decision, alternativas consideradas,
  consecuencias y fuente de la decision.

## Indice

| ADR | Titulo | Estado |
| --- | --- | --- |
| [0001](0001-use-context-hub-as-product-architecture-memory.md) | Usar este hub como memoria de producto y arquitectura | Accepted |
| [0002](0002-separate-general-and-daily-predictions.md) | Separar predicciones generales y diarias | Accepted |
| [0003](0003-lock-daily-predictions-per-match.md) | Bloquear predicciones diarias por partido | Accepted |
| [0004](0004-admin-controlled-scoring-and-result-resolution.md) | Control admin de scoring y resolucion de resultados | Accepted |
| [0005](0005-prioritize-gamification-ranking-before-calendar-reminders.md) | Priorizar gamificacion y ranking antes de recordatorios | Accepted |
| [0006](0006-use-cloudflare-d1-and-cloudflare-deployment.md) | Usar Cloudflare D1 y despliegue primario en Cloudflare | Accepted |
| [0007](0007-use-corporate-microsoft-identity-and-role-based-access.md) | Usar identidad corporativa Microsoft y acceso por roles | Accepted |
| [0008](0008-prefer-traditional-apis-for-verifiable-football-data.md) | Usar APIs tradicionales para datos verificables de futbol | Accepted |
| [0009](0009-propose-rebalancing-predictions-ux-and-daily-scoring.md) | Rebalancear UX de predicciones y scoring diario | Accepted |
| [0010](0010-propose-normalized-group-ranking-and-personal-performance-view.md) | Usar ranking individual y rankings grupales normalizados | Accepted |
| [0011](0011-propose-teams-reminders-and-home-reminder-flow.md) | Redirigir predicciones desde Home y dejar notificaciones fuera de scope | Accepted |
| [0012](0012-propose-d1-schema-options-for-manual-and-api-ingestion.md) | Usar schema D1 para ingestion API-Football Free | Accepted |

## Criterio de uso

Usar ADRs para decisiones que futuras personas o agentes deban entender antes de
modificar el producto, sus reglas o su arquitectura. No usar ADRs para cambios
rutinarios de redaccion o fixes menores.
