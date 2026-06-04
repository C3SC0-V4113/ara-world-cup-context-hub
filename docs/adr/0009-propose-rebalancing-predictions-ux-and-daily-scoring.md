# ADR-0009: Rebalancear UX de predicciones y scoring diario

## Estado

Accepted

## Contexto

La vista Predicciones contiene predicciones generales y predicciones del dia. Las
predicciones generales se completan al inicio y luego quedan bloqueadas, mientras
que las predicciones del dia sostienen la participacion recurrente durante el
Mundial.

Tambien se decidio quitar categorias ambiguas como seleccion sorpresa y
seleccion decepcion. Para compensar, las predicciones generales objetivas ganan
un peso moderado y se agrega una prediccion global de clasificados desde fase de
grupos.

## Decision

Separar la vista Predicciones en dos tabs:

- **Predicciones del dia** como tab principal.
- **Predicciones generales** como tab secundario.

Las predicciones del dia deben priorizar partidos por urgencia, estado y kickoff.
Home no debe guardar predicciones como quick action; debe redirigir a esta vista.

El scoring se rebalancea para mantener puntos globales relevantes, pero con mas
formas simples de ganar puntos por partido.

### Scoring global aceptado

| Mecanica | Puntos | Nota |
| --- | ---: | --- |
| Campeon acertado | 25 | Sube de 20 a 25. |
| Finalista acertado | 15 | Aplica por cada finalista acertado si se modela como seleccion multiple. |
| Seleccion con mas goles | 12 | Dato objetivo derivado de goles acumulados. |
| Mejor defensa | 12 | Metrica compuesta simple. |
| Clasificados desde grupos | 2 por acierto | Opcion multiple; puntua cada equipo clasificado correctamente. |
| Seleccion sorpresa | Fuera de alcance | Eliminada por ambiguedad. |
| Seleccion decepcion | Fuera de alcance | Eliminada por ambiguedad. |

`Clasificados desde grupos` significa equipos que avanzan desde fase de grupos a
la siguiente ronda eliminatoria. No debe acoplarse al nombre exacto de la ronda.

### Scoring por partido aceptado

| Mecanica | Puntos |
| --- | ---: |
| Marcador exacto | 5 |
| Ganador acertado | 3 |
| Empate acertado | 3 |
| Ambos equipos anotan | 1 |
| Al menos una tarjeta roja | 1 |
| Rango de tarjetas amarillas totales | 1 |
| Rango de tiros al arco totales | 1 |
| Ganador por penales en eliminatorias, si aplica | 2 |

La tabla de reglas debe seguir siendo administrable para permitir ajustes futuros
sin tocar codigo.

## Alternativas consideradas

### Mantener vista actual

- Ventajas: no requiere redisenar la experiencia actual.
- Desventajas: las predicciones generales seguirian ocupando demasiado espacio y
  las diarias no guiarian la accion principal.

### Separar generales y diarias en vistas independientes

- Ventajas: reduce mezcla conceptual.
- Desventajas: fragmenta una experiencia que debe mantenerse simple.

### Tabs dentro de Predicciones

- Ventajas: mantiene un destino unico, prioriza la accion diaria y conserva
  acceso claro a predicciones generales.
- Desventajas: requiere ajustar layout, copy, estados y reglas visibles.

## Consecuencias

### Positivas

- El usuario ve primero lo que puede editar y lo que cierra pronto.
- El juego se mantiene activo durante el torneo por mas mecanicas por partido.
- Las globales conservan impacto sin depender de categorias ambiguas.
- La UI puede explicar mejor que puntos son diarios y cuales son de largo plazo.

### Negativas

- El scoring debe recalcularse con reglas nuevas si ya existian mocks o datos
  previos.
- Las metricas por tarjetas y tiros al arco dependen de disponibilidad real de
  API-Football Free.
- Hay que evitar que demasiadas micro-reglas hagan dificil explicar el juego.

## Fuente de la decision

- Decision posterior del usuario: aprobar rebalance, usar tabs, eliminar
  sorpresa/decepcion, agregar clasificados y reforzar scoring global de forma
  moderada.
- Contexto previo: ADR-0002 separa predicciones generales y diarias.
- Contexto previo: ADR-0003 define bloqueo por partido para predicciones diarias.
