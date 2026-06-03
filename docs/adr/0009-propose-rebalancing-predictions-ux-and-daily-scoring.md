# ADR-0009: Proponer rebalance de UX de predicciones y scoring diario

## Estado

Proposed

## Contexto

La vista Predicciones contiene predicciones generales y predicciones diarias. Las
predicciones generales se completan al inicio y luego quedan bloqueadas, pero en
el estado actual pueden ocupar demasiado protagonismo visual. Al mismo tiempo, las
predicciones diarias se perciben juntas, sin suficiente orden ni prioridad.

El scoring actual tambien tiene varias mecanicas enfocadas al final del torneo.
Eso puede reducir el peso percibido de la participacion diaria, que es la parte
que mantiene activo el juego durante el Mundial.

## Decision

Se propone rebalancear la vista Predicciones para dar mas peso visual y funcional
a las predicciones diarias.

La propuesta incluye:

- ordenar y priorizar predicciones diarias por urgencia, estado y kickoff;
- reducir protagonismo de predicciones generales despues del setup inicial;
- tratar predicciones generales bloqueadas como resumen o seccion secundaria;
- revisar scoring para aumentar el foco relativo de partidos diarios;
- explicar en la vista que puntos pueden ganarse hoy y cuales son de largo plazo.

## Alternativas consideradas

### Mantener vista actual

- Ventajas: no requiere redisenar la experiencia actual.
- Desventajas: las predicciones generales siguen ocupando demasiado espacio y las
  diarias no guian la accion principal.

### Separar generales y diarias en vistas independientes

- Ventajas: reduce mezcla conceptual.
- Desventajas: puede fragmentar demasiado una experiencia que deberia ser simple.

### Rebalancear dentro de la misma vista

- Ventajas: mantiene un solo destino para predicciones y prioriza el uso diario.
- Desventajas: requiere redisenar layout, copy y reglas visibles.

## Consecuencias

### Positivas

- La vista se alinea mejor con la accion recurrente del usuario.
- El usuario ve primero lo que puede editar y lo que cierra pronto.
- El juego puede sentirse mas activo durante todo el torneo.

### Negativas

- Se debe revisar copy, jerarquia visual y componentes existentes.
- El refactor de scoring puede requerir recalculo o explicacion adicional.
- Si el scoring diario aumenta demasiado, puede desbalancear predicciones
  generales.

## Fuente de la decision/propuesta

- Criterio tecnico del usuario.
- Contexto previo: ADR-0002 separa predicciones generales y diarias.
- Contexto previo: ADR-0003 define bloqueo por partido para predicciones diarias.

