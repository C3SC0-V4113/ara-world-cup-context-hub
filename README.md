# Ara World Cup Context Hub

Repositorio documental para centralizar contexto, decisiones tecnicas y guias de trabajo de la aplicacion web Ara World Cup.

## Proposito

Este repositorio no contiene codigo de aplicacion. Su funcion es mantener documentacion util para agentes y personas que trabajen en el producto:

- contexto del proyecto y convenciones de colaboracion;
- Architecture Decision Records (ADRs);
- skills de agente necesarias para mantener documentacion tecnica consistente.

## Skills instaladas

La skill `architecture-decision-records` esta instalada desde `wshobson/agents` para ayudar a redactar y mantener ADRs.

Instalacion reproducible:

```powershell
npx skills@latest add https://github.com/wshobson/agents --skill architecture-decision-records --agent codex --yes
```

La instalacion canonica vive en:

```text
.agents/skills/architecture-decision-records/
```

El archivo `skills-lock.json` registra la skill instalada y debe versionarse junto con el repositorio.

## Compatibilidad con Claude Code

`.agents` es la fuente canonica de skills del proyecto. `.claude` apunta a `.agents` para que Claude Code resuelva las mismas skills sin duplicar archivos.

En esta maquina Windows, `.claude` puede quedar como junction si el sistema bloquea symlinks sin privilegios de administrador. No edites `.claude` directamente; edita `.agents`.

## Documentacion para agentes

Las instrucciones operativas viven en `AGENTS.md`.

`CLAUDE.md` solo referencia `AGENTS.md` para evitar reglas duplicadas.
