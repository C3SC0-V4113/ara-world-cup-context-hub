# ADR-0003: Bloquear Predicciones Diarias Por Partido

## Status

Accepted

## Context

Una jornada puede tener varios partidos en horarios distintos. Si el sistema
bloquea toda la jornada cuando el primer partido se acerca, impediria editar
predicciones de partidos posteriores que todavia deberian estar abiertas.

Walter identifica este punto durante la revision y recomienda separar la
experiencia diaria de la logica tecnica de bloqueo por partido.

## Decision

Las predicciones diarias se bloquearan por partido individual 30 minutos antes
del kickoff de cada partido.

La vista diaria puede mostrar varios partidos juntos, pero cada partido debe
tener estado propio: editable o bloqueado.

## Considered Options

### Bloquear toda la jornada

- Pros: implementacion mas simple.
- Cons: cierra antes de tiempo partidos posteriores y contradice la regla de 30
  minutos por partido.

### Bloquear por partido

- Pros: respeta la ventana de edicion de cada partido y reduce frustracion del
  usuario.
- Cons: exige UI con estados mixtos dentro de la misma jornada.

## Consequences

### Positive

- Usuarios pueden ajustar partidos posteriores durante el dia.
- La regla de 30 minutos es consistente y defendible.
- La UI puede comunicar claramente que solo algunos partidos ya cerraron.

### Negative

- Guardar una jornada completa requiere ignorar o rechazar cambios a partidos ya
  bloqueados.
- Se necesitan pruebas con horarios distintos en el mismo dia.

## Decision Source

- Alejandra valida el caso de partidos a diferentes horas.
- Walter aporta la recomendacion tecnica de bloqueo independiente por partido y
  estado visual para partidos cerrados.
