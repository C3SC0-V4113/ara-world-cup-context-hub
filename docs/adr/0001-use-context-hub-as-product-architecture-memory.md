# ADR-0001: Usar Este Hub Como Memoria de Producto y Arquitectura

## Status

Accepted

## Context

Ara World Cup tiene un repo de aplicacion con codigo, PRD, ADRs tecnicos y
documentacion de design system. Tambien existe contexto de reunion que incluye
intencion de producto, decisiones funcionales, recomendaciones tecnicas y riesgos.

Ese contexto necesita una casa documental separada para que agentes y personas
puedan entender el producto sin mezclarlo con cambios de implementacion.

## Decision

Usaremos `ara-world-cup-context-hub` como memoria documental de producto,
decisiones funcionales y arquitectura de alto nivel.

Este repo no sera repositorio de implementacion de la app. El repo
`E:\Repositorios\ara-world-cup` seguira siendo la fuente del codigo.

## Considered Options

### Documentar todo en el repo de aplicacion

- Pros: contexto cerca del codigo.
- Cons: mezcla memoria de producto con implementacion y aumenta ruido en un repo
  activo.

### Mantener solo transcripciones y resumenes sueltos

- Pros: bajo esfuerzo inicial.
- Cons: dificil de consultar, poco accionable y propenso a ambiguedades.

### Crear hub documental separado

- Pros: preserva contexto, decisiones y glosario en un lugar estable.
- Cons: requiere disciplina para mantener enlaces y sincronizar cambios relevantes.

## Consequences

### Positive

- Agentes tienen una entrada clara para entender producto y decisiones.
- Las decisiones funcionales quedan separadas de detalles de stack.
- La transcripcion cruda no necesita versionarse para conservar contexto util.

### Negative

- Hay que mantener consistencia entre hub y repo de aplicacion.
- Cambios importantes pueden requerir actualizar dos repositorios documentales.

## Decision Source

- Necesidad expresada por el usuario de crear ADRs, diagramas, glosarios y
  actualizar `README.md` y `AGENTS.md`.
- Criterio operativo del repo: `README.md` para personas y `AGENTS.md` para
  agentes.
