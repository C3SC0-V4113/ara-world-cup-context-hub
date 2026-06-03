# Reglas de Bloqueo de Predicciones

## Predicciones generales

Requerimiento de producto:

- El usuario completa las predicciones generales una vez.
- Las predicciones generales se pueden guardar mientras esten abiertas.
- Cuando se cierran o bloquean, ya no deben editarse.

Guia tecnica:

- El estado de cierre debe depender de una configuracion administrativa y/o una
  fecha limite.
- El bloqueo debe aplicarse por usuario a su unica prediccion general.
- La UI debe comunicar si todavia es editable o si ya quedo cerrada.

## Predicciones diarias

Requerimiento de producto:

- El usuario puede predecir partidos del dia.
- Puede cambiar una prediccion hasta 30 minutos antes del inicio del partido.
- Despues del cierre, la prediccion no se puede tocar.

Guia tecnica fuerte de Walter:

- El cierre debe evaluarse por partido individual, no por jornada completa.
- Si un dia hay varios partidos, cerrar el primero no debe cerrar los demas.
- La vista puede permitir guardar una jornada completa, pero la persistencia debe
  ignorar o rechazar cambios sobre partidos ya bloqueados.

## UX esperada

- Partido abierto: controles editables y mensaje "Cierra 30 min antes".
- Partido bloqueado: controles deshabilitados, estado visible y estilo atenuado.
- Jornada con mezcla de estados: algunos partidos pueden estar bloqueados y otros
  seguir abiertos.
- El usuario debe poder entender por que un partido ya no es editable.

## Casos de prueba funcionales

- A las 12:29 p.m., un partido de 1:00 p.m. ya esta bloqueado.
- A las 12:29 p.m., un partido de 4:00 p.m. sigue editable.
- Un partido finalizado siempre esta bloqueado.
- Una prediccion general cerrada por admin no puede modificarse aunque la fecha
  limite no haya pasado.
- Una prediccion diaria bloqueada no debe cambiar aunque el cliente envie datos.
