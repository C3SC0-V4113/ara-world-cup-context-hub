# Ara World Cup Context Hub

Repositorio documental para centralizar contexto de producto, decisiones
funcionales, ADRs y guias de trabajo de Ara World Cup.

Este repositorio no contiene codigo de aplicacion. La implementacion vive en:

```text
E:\Repositorios\ara-world-cup
```

## Proposito

El hub preserva memoria tecnica y de producto para que personas y agentes puedan
entender el juego interno de predicciones del Mundial sin depender de una
transcripcion cruda o de conocimiento oral.

Sirve para:

- documentar decisiones funcionales y tecnicas de alto nivel;
- explicar reglas del juego, scoring, ranking y bloqueo de predicciones;
- mantener glosarios y diagramas editables;
- orientar futuras implementaciones en el repo de aplicacion.

Decision arquitectonica vigente: la base de datos objetivo es **Cloudflare D1**,
operada con **Wrangler**. El despliegue primario de la app web debe ser
**Cloudflare**, con **Vercel** solo como fallback si Cloudflare bloquea el avance.
La identidad objetivo usa cuentas corporativas Microsoft/Windows usadas en Teams,
con roles base `user` y `admin`.

Propuesta en evaluacion: para datos objetivos de futbol, se investiga preferir
APIs tradicionales o datasets verificables, cachearlos en D1 y reservar IA para
categorias ambiguas o narrativa con validacion admin. Esta propuesta aun no esta
aprobada.

## Jerarquia de fuentes

La reunion de referencia se interpreta con esta regla:

- **Alejandra Alvarado** es fuente principal de intencion de producto, UX deseada
  y necesidades no tecnicas.
- **Walter Zetino** es fuente principal de criterio tecnico, arquitectura, riesgos,
  priorizacion y forma de implementacion.

Las ideas de Alejandra deben traducirse a requerimientos claros aunque hayan sido
expresadas de forma vaga. Las observaciones de Walter tienen mayor peso para
decisiones tecnicas, riesgos y priorizacion.

## Documentacion principal

- [Contexto de producto](docs/product-context.md)
- [Sintesis de reunion](docs/meeting-notes/sync-mundial.md)
- [Glosario](docs/glossary.md)
- [Reglas de puntuacion](docs/rules/scoring-rules.md)
- [Reglas de bloqueo](docs/rules/prediction-locking.md)
- [Plataforma y base de datos](docs/architecture/platform-and-database.md)
- [Propuesta de opciones de schema D1](docs/architecture/d1-schema-options.md)
- [Identidad y acceso](docs/architecture/identity-and-access.md)
- [Propuesta de APIs tradicionales y fuentes de datos](docs/integrations/football-data-apis.md)
- [Summary CTO de propuesta de arquitectura](docs/proposals/cto-architecture-summary.md)
- [Propuestas UX y arquitectura por vista](docs/proposals/view-ux-architecture-proposals.md)
- [Mapa del repo de aplicacion](docs/app-repo-map.md)
- [Architecture Decision Records](docs/adr/README.md)

## Diagramas

- [Ciclo de vida de predicciones](docs/diagrams/prediction-lifecycle.md)
- [Flujo de scoring y ranking](docs/diagrams/scoring-ranking-flow.md)
- [Modelo conceptual de datos](docs/diagrams/conceptual-data-model.md)
- [Navegacion Home, Ranking y Gamificacion](docs/diagrams/home-ranking-navigation.md)
- [Propuesta de ingestion de datos externos](docs/diagrams/external-data-ingestion.md)
- [Propuesta D1 con ingesta manual](docs/diagrams/d1-manual-ingestion-architecture.md)
- [Propuesta D1 con API-Football](docs/diagrams/d1-api-football-ingestion-architecture.md)

Los diagramas estan en Mermaid para que puedan mantenerse como texto versionado.

## Skills instaladas

La skill `architecture-decision-records` esta instalada desde `wshobson/agents`
para ayudar a redactar y mantener ADRs.

Instalacion reproducible:

```powershell
npx skills@latest add https://github.com/wshobson/agents --skill architecture-decision-records --agent codex --yes
```

La instalacion canonica vive en:

```text
.agents/skills/architecture-decision-records/
```

El archivo `skills-lock.json` registra la skill instalada y debe versionarse junto
con el repositorio.

## Compatibilidad con Claude Code

`.agents` es la fuente canonica de skills del proyecto. `.claude` apunta a
`.agents` para que Claude Code resuelva las mismas skills sin duplicar archivos.

En esta maquina Windows, `.claude` puede quedar como junction si el sistema
bloquea symlinks sin privilegios de administrador. No edites `.claude`
directamente; edita `.agents`.

## Documentacion para agentes

Las instrucciones operativas viven en `AGENTS.md`.

`CLAUDE.md` solo referencia `AGENTS.md` para evitar reglas duplicadas.
