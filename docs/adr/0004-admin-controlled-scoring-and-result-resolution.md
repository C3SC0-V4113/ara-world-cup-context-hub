# ADR-0004: Control Admin de Scoring y Resolucion de Resultados

## Status

Accepted

## Context

La plataforma necesita calcular puntos por reglas objetivas y permitir
correcciones admin antes de impactar ranking.

Walter advierte que algunas categorias pueden depender de criterio y que conviene
tener un panel administrativo o mecanismo de correccion manual.

## Decision

Scoring, resultados y resolucion de categorias subjetivas deben poder
administrarse o corregirse manualmente.

La informacion verificable externa puede usarse como asistencia, pero no sera
fuente autonoma final para cerrar resultados de scoring sin persistencia,
normalizacion y posibilidad de override admin.

Nota posterior: ADR-0008 y ADR-0009 restringen esta posibilidad para la version
actual. No se usaran LLMs y las categorias seleccion sorpresa y seleccion
decepcion quedan fuera de alcance. El control admin se conserva para overrides,
correcciones, cierres y recalculos sobre datos objetivos.

## Considered Options

### Resolver todo manualmente

- Pros: control total y menor riesgo de alucinacion.
- Cons: carga operativa alta y posible sesgo de quien administra.

### Resolver todo con automatizacion externa

- Pros: menos trabajo manual y potencial automatizacion.
- Cons: riesgo de datos incorrectos, alucinaciones y controversia.

### Datos externos con control admin

- Pros: balancea eficiencia y control.
- Cons: requiere disenar flujo de revision y auditoria.

## Consequences

### Positive

- El juego puede corregir errores antes de impactar ranking.
- Los overrides y correcciones tienen una autoridad final clara.
- El sistema puede operar incluso si una API externa falla.

### Negative

- Admin necesita interfaz o flujo operativo.
- Hay que definir trazabilidad minima para cambios de resultados y reglas.

## Decision Source

- Alejandra expresa que no quiere depender de que una persona vea todos los
  partidos para decidir categorias.
- Walter propuso IA/internet como apoyo en la conversacion original, pero enfatizo
  administrabilidad y riesgo de alucinacion. ADR-0008 y ADR-0009 dejan LLMs fuera
  de alcance para la version actual.
