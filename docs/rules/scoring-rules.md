# Reglas de Puntuacion

## Estado general

Accepted.

El sistema de puntos se reworkea para mantener predicciones globales relevantes,
pero aumentar las formas simples de ganar puntos por partido. Las reglas deben
seguir siendo administrables sin tocar codigo y todo calculo debe producir
`score_events` auditables.

## Predicciones globales

| Mecanica | Puntos | Estado | Fuente |
| --- | ---: | --- | --- |
| Campeon acertado | 25 | Accepted | Decision posterior del usuario |
| Finalista acertado | 15 | Accepted | Decision posterior del usuario |
| Seleccion con mas goles | 12 | Accepted | Dato objetivo derivado de goles acumulados |
| Mejor defensa | 12 | Accepted | Metrica compuesta simple |
| Clasificados desde grupos | 2 por acierto | Accepted | Opcion multiple por equipos clasificados |
| Seleccion sorpresa | Fuera de alcance | Accepted | Eliminada por ambiguedad |
| Seleccion decepcion | Fuera de alcance | Accepted | Eliminada por ambiguedad |

`Clasificados desde grupos` significa equipos que avanzan desde fase de grupos a
la siguiente ronda eliminatoria. No debe depender del nombre exacto de la ronda.

## Predicciones por partido

| Mecanica | Puntos | Estado |
| --- | ---: | --- |
| Marcador exacto | 5 | Accepted |
| Ganador acertado | 3 | Accepted |
| Empate acertado | 3 | Accepted |
| Ambos equipos anotan | 1 | Accepted |
| Al menos una tarjeta roja | 1 | Accepted |
| Rango de tarjetas amarillas totales | 1 | Accepted |
| Rango de tiros al arco totales | 1 | Accepted |
| Ganador por penales en eliminatorias, si aplica | 2 | Accepted |

## Mejor defensa

La seleccion con mejor defensa se calcula con una metrica compuesta simple:

1. Menor promedio de goles recibidos por partido.
2. Desempate por porcentaje de clean sheets.
3. Desempate por menor promedio de tiros al arco recibidos.
4. Desempate por menor penalizacion disciplinaria por tarjetas.

Si API-Football Free no entrega alguna metrica, se usa la siguiente disponible en
ese orden. Si solo hay goles recibidos disponibles, la categoria se resuelve con
ese dato y desempates administrables.

## Reglas interpretativas

- La tabla de puntos debe ser fuente de verdad para scoring y para la UI de
  "como ganas puntos".
- Cambios posteriores en reglas deben dejar claro si aplican retroactivamente o
  solo hacia adelante.
- Scoring debe leer datos normalizados desde D1, no payloads crudos de proveedor.
- Admin puede corregir datos importados mediante overrides auditables.
- No se usaran LLMs para resolver categorias de scoring.

## Fuera de alcance

- Seleccion sorpresa.
- Seleccion decepcion.
- Notificaciones o recordatorios como fuente de puntos.
- Scoring basado en narrativa o criterio no verificable.
