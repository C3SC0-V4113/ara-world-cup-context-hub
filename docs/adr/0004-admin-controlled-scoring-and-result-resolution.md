# ADR-0004: Control Admin de Scoring y Resolucion de Resultados

## Status

Accepted

## Context

La plataforma necesita calcular puntos por reglas objetivas y resolver categorias
subjetivas como seleccion sorpresa, seleccion decepcion y mejor defensa.

Walter advierte que estas categorias pueden depender de criterio, que una IA puede
alucinar y que conviene tener un panel administrativo o mecanismo de correccion
manual.

## Decision

Scoring, resultados y resolucion de categorias subjetivas deben poder
administrarse o corregirse manualmente.

La IA o informacion de internet puede usarse como asistencia, pero no sera fuente
autonoma final para cerrar resultados de scoring.

## Considered Options

### Resolver todo manualmente

- Pros: control total y menor riesgo de alucinacion.
- Cons: carga operativa alta y posible sesgo de quien administra.

### Resolver todo con IA o APIs externas

- Pros: menos trabajo manual y potencial automatizacion.
- Cons: riesgo de datos incorrectos, alucinaciones y controversia.

### Asistencia automatizada con control admin

- Pros: balancea eficiencia y control.
- Cons: requiere disenar flujo de revision y auditoria.

## Consequences

### Positive

- El juego puede corregir errores antes de impactar ranking.
- Las categorias subjetivas tienen una autoridad final clara.
- El sistema puede operar incluso si una API externa falla.

### Negative

- Admin necesita interfaz o flujo operativo.
- Hay que definir trazabilidad minima para cambios de resultados y reglas.

## Decision Source

- Alejandra expresa que no quiere depender de que una persona vea todos los
  partidos para decidir categorias.
- Walter propone IA/internet como apoyo, pero enfatiza administrabilidad y riesgo
  de alucinacion.
