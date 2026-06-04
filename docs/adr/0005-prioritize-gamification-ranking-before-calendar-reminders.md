# ADR-0005: Priorizar Gamificacion y Ranking Antes de Recordatorios

## Status

Accepted

## Context

Durante la revision aparecen ideas de calendario, boton "Recordarme", Google
Calendar, mensajes de Teams y notificaciones sobre partidos.

Walter recomienda cuidar el esfuerzo y concentrarse primero en la parte que hace
unico al producto: predicciones, ranking y gamificacion. El calendario de partidos
puede consultarse por muchas otras vias.

## Decision

Priorizaremos la gamificacion, las predicciones y el ranking basico antes que
calendario avanzado, recordatorios o integraciones de agenda.

Recordatorios y Teams bot quedan como complementos posteriores, no como requisito
central inicial.

Nota posterior: ADR-0011 deja notificaciones, recordatorios y Teams bot fuera de
scope para la version actual. Si se reactivan, requeriran una decision posterior.

## Considered Options

### Integrar calendario y recordatorios desde el inicio

- Pros: experiencia mas completa alrededor de partidos.
- Cons: puede desviar esfuerzo del core del juego y generar confusion con agenda
  laboral.

### Enfocar primero predicciones, ranking y gamificacion

- Pros: concentra esfuerzo en la propuesta de valor principal.
- Cons: home y calendario quedan menos sofisticados al inicio.

## Consequences

### Positive

- El primer incremento funcional puede validar el juego interno.
- Ranking y scoring reciben atencion temprana.
- Se reduce riesgo de malinterpretar recordatorios como permisos para ver partidos.

### Negative

- Algunas ideas de engagement por calendario quedan pendientes.
- Teams bot queda fuera de scope por ADR-0011; necesitara una decision tecnica
  posterior si se implementa.

## Decision Source

- Alejandra plantea recordatorios y foco en selecciones favoritas.
- Walter recomienda no gastar esfuerzo ahi inicialmente y priorizar gamificacion,
  predicciones, ranking y luego home/calendario.
