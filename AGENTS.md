# Agent Instructions

Este repositorio es documental. Mantener los cambios claros, pequenos y orientados a preservar contexto tecnico para el proyecto Ara World Cup.

## Reglas generales

- Usa `README.md` como entrada para personas y `AGENTS.md` como entrada operativa para agentes.
- No dupliques instrucciones entre `AGENTS.md` y `CLAUDE.md`; `CLAUDE.md` debe seguir siendo una referencia a este archivo.
- Mantener `.agents` como fuente canonica de skills.
- No editar `.claude` directamente; existe solo como enlace compatible hacia `.agents`.
- Mantener `skills-lock.json` versionado cuando cambien las skills.

## Architecture Decision Records

Usa la skill `architecture-decision-records` para redactar o revisar ADRs cuando haya decisiones tecnicas significativas, por ejemplo:

- seleccion de framework, plataforma, base de datos o proveedor;
- patrones de API, integracion, autenticacion o despliegue;
- decisiones con impacto en seguridad, escalabilidad, costo u operacion;
- cambios que futuras personas del equipo deberan entender en contexto.

No uses ADRs para cambios rutinarios, fixes menores o configuraciones de bajo impacto.

## Convencion sugerida para ADRs

Cuando se agreguen ADRs, usar una carpeta documental dedicada como:

```text
docs/adr/
```

Nombrar archivos con numero y titulo breve:

```text
0001-use-example-technology.md
```

Cada ADR debe incluir como minimo: estado, contexto, decision, alternativas consideradas y consecuencias.
