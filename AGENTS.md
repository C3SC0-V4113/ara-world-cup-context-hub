# Agent Instructions

Este repositorio es documental. Mantener los cambios claros, pequenos y
orientados a preservar contexto tecnico y de producto para Ara World Cup.

## Reglas generales

- Usa `README.md` como entrada para personas y `AGENTS.md` como entrada operativa
  para agentes.
- No dupliques instrucciones entre `AGENTS.md` y `CLAUDE.md`; `CLAUDE.md` debe
  seguir siendo una referencia a este archivo.
- Mantener `.agents` como fuente canonica de skills.
- No editar `.claude` directamente; existe solo como enlace compatible hacia
  `.agents`.
- Mantener `skills-lock.json` versionado cuando cambien las skills.
- No editar el repo de aplicacion `E:\Repositorios\ara-world-cup` desde este hub
  salvo solicitud explicita.

## Jerarquia de fuentes del proyecto

Cuando documentes contexto derivado de la reunion:

- Trata a **Alejandra Alvarado** como fuente principal de intencion de producto,
  UX deseada y necesidades no tecnicas.
- Trata a **Walter Zetino** como fuente principal de criterio tecnico,
  arquitectura, riesgos, priorizacion y forma de implementacion.
- Las propuestas de Alejandra no deben descartarse por ser vagas; deben
  traducirse a requerimientos claros y marcarse como abiertas si falta precision.
- Las observaciones de Walter tienen mayor peso para decisiones tecnicas,
  tradeoffs, riesgos y orden recomendado de implementacion.

## Clasificacion obligatoria de contexto

Al crear o actualizar documentos, separa explicitamente:

- requerimiento de producto;
- sugerencia tecnica;
- decision aceptada;
- pregunta abierta;
- riesgo o restriccion.

No mezcles decisiones cerradas con ideas exploratorias. Si una definicion aun
depende de negocio o de implementacion, marcala como pregunta abierta o supuesto.

## Transcripciones y fuentes crudas

- No versionar transcripciones crudas, audios o notas extensas de reunion salvo
  peticion explicita.
- Convertir transcripciones en sintesis depuradas, decisiones, acciones,
  glosarios y diagramas.
- Mantener evidencia de fuente a nivel de tema, no copiar conversaciones completas.

## Architecture Decision Records

Usa la skill `architecture-decision-records` para redactar o revisar ADRs cuando
haya decisiones tecnicas o funcionales significativas, por ejemplo:

- seleccion de framework, plataforma, base de datos o proveedor;
- patrones de API, integracion, autenticacion o despliegue;
- reglas de scoring, ranking, bloqueo o administracion del juego;
- decisiones con impacto en seguridad, escalabilidad, costo u operacion;
- cambios que futuras personas del equipo deberan entender en contexto.

No uses ADRs para cambios rutinarios, fixes menores o configuraciones de bajo
impacto.

## Convencion para ADRs

Los ADRs viven en:

```text
docs/adr/
```

Nombrar archivos con numero y titulo breve:

```text
0001-use-example-technology.md
```

Cada ADR debe incluir como minimo:

- estado;
- contexto;
- decision;
- alternativas consideradas;
- consecuencias;
- fuente de la decision.

Actualizar `docs/adr/README.md` cuando se agreguen, retiren o cambien ADRs.

## Diagramas y documentos

- Usar Markdown para documentos.
- Usar Mermaid para diagramas editables.
- Mantener documentos en espanol.
- Actualizar `README.md` cuando se agregue documentacion principal o diagramas.
- Preferir documentos pequenos y enfocados antes que documentos largos con temas
  mezclados.

## Relacion con el repo de aplicacion

El repo `E:\Repositorios\ara-world-cup` puede usarse como referencia de lectura
para mapear estado tecnico, vistas, servicios y brechas.

Este hub no debe reemplazar los ADRs tecnicos del repo de aplicacion. Si una
decision funcional aqui implica cambios de stack, datos o arquitectura de codigo,
documenta el contexto en este hub y recomienda un ADR tecnico en el repo de
aplicacion.

## Plataforma y base de datos

- Tratar Supabase/PostgreSQL como estado anterior, legado o fuente de migracion,
  no como backend preferido.
- La base de datos objetivo es Cloudflare D1.
- Wrangler es la herramienta esperada para configurar y operar recursos de
  Cloudflare.
- El despliegue primario de la web es Cloudflare.
- Vercel solo debe documentarse como fallback si Cloudflare no resulta viable.
- Si una fuente futura contradice esta decision, crear un nuevo ADR que la
  superseda en lugar de editar silenciosamente el ADR aceptado.

## Identidad y acceso

- Tratar Microsoft Entra ID o las cuentas corporativas Microsoft/Windows usadas
  en Teams como proveedor de identidad objetivo.
- Preservar los roles base `user` y `admin` salvo que un ADR posterior los
  superseda.
- Documentar nombre, correo y fotografia/avatar como datos deseados del perfil
  corporativo; la fotografia siempre debe tener fallback.
- Las acciones administrativas deben protegerse por rol en servidor o servicios,
  no solo ocultando controles en UI.
- Si el runtime de Cloudflare obliga a ajustar NextAuth, mantener la decision de
  identidad corporativa Microsoft salvo ADR contrario.

## Fuentes externas y APIs

- La estrategia de APIs tradicionales para datos verificables esta documentada en
  ADR-0008 con estado `Proposed`; no tratarla como decision aceptada.
- Cuando se investiguen datos objetivos de futbol, usar la propuesta como guia de
  evaluacion: buscar APIs tradicionales, datasets estructurados o fuentes
  oficiales antes de proponer IA.
- Tratar Cloudflare D1 como fuente canonica interna solo para decisiones ya
  aceptadas; la ingestion/cache de APIs externas hacia D1 sigue pendiente de
  aprobacion.
- No documentar IA como fuente final propuesta para marcadores, resultados,
  standings, top scorers, seleccion con mas goles, mejor defensa u otros datos
  objetivos sin marcarlo como riesgo o alternativa rechazada.
- IA puede proponerse como asistencia en categorias ambiguas, narrativa y
  explicaciones, siempre con evidencia de datos normalizados y validacion admin.
- Las correcciones admin sobre datos externos son una propuesta de control
  operativo; si se aprueban, deberan documentarse como overrides auditables.
- Al recomendar proveedores, priorizar opciones gratis o baratas y documentar
  fecha, costo aproximado, cobertura, limites y riesgos de cuota.
