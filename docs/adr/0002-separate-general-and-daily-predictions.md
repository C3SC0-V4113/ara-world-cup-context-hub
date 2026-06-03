# ADR-0002: Separar Predicciones Generales y Diarias

## Status

Accepted

## Context

La reunion define dos dinamicas distintas:

- predicciones generales del torneo, completadas una vez antes o al inicio del
  Mundial;
- predicciones diarias por partido, editables durante el dia hasta su cierre.

Estas dinamicas tienen tiempos, categorias, reglas de bloqueo y scoring
diferentes.

## Decision

Modelaremos predicciones generales y predicciones diarias como mecanicas separadas.

Las predicciones generales incluyen categorias de torneo completo. Las
predicciones diarias se asocian a partidos especificos y pueden incluir una
mecanica adicional de partido con mas goles del dia.

## Considered Options

### Un unico formulario de predicciones

- Pros: UI inicial mas simple.
- Cons: mezcla reglas de cierre, categorias y scoring incompatibles.

### Separacion por dos dinamicas

- Pros: refleja el dominio, facilita scoring y permite cierre diferenciado.
- Cons: requiere mas explicacion en UI y mas modelo de datos.

## Consequences

### Positive

- Las reglas de negocio quedan mas claras.
- El ranking puede sumar puntos de fuentes distintas.
- Admin puede operar cierres y scoring por mecanica.

### Negative

- El usuario debe entender dos tipos de participacion.
- Requiere cuidado para que la UI no parezca duplicada o confusa.

## Decision Source

- Alejandra describe dos formas de jugar: una general del torneo y otra dia a dia.
- Walter no objeta la separacion y enfoca sus observaciones tecnicas en como
  cerrar correctamente la mecanica diaria.
