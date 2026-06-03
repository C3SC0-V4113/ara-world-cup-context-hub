# Reglas de Puntuacion

## Estado general

La reunion dejo una tabla de puntos propuesta, pero no todas las reglas quedaron
cerradas con la misma confianza. Este documento separa lo confirmado de lo
ambiguo para evitar endurecer supuestos incorrectos.

## Tabla propuesta

| Mecanica | Puntos | Estado | Fuente |
| --- | ---: | --- | --- |
| Campeon acertado | 20 | Confirmada | Alejandra lo enumera y Walter lo valida como mockup util |
| Finalista | 12 | Confirmada con definicion pendiente | Alejandra |
| Seleccion sorpresa | 10 | Confirmada como valor, criterio pendiente | Alejandra, con alerta tecnica de Walter |
| Seleccion decepcion | 10 | Confirmada como valor, criterio pendiente | Alejandra aclara que son 10 y 10 |
| Ganador del partido | 2 | Confirmada | Alejandra |
| Empate acertado | 3 | Confirmada | Alejandra |
| Ambos equipos anotan | 1 | Confirmada | Alejandra |
| Marcador exacto | 5 | Confirmada | Alejandra |
| Partido con mas goles del dia | 3 | Confirmada con desempate pendiente | Alejandra |
| Penales acertados | 3 | Sugerida para eliminatorias | Alejandra |
| Seleccion con mas goles | Pendiente | Ambigua | Alejandra la menciona, no queda puntaje claro |
| Mejor defensa | Pendiente | Ambigua | Alejandra la menciona, no queda puntaje claro |

## Reglas interpretativas

- Las reglas deben poder administrarse sin tocar codigo.
- La tabla de puntos debe ser fuente de verdad para scoring y para la UI de "como
  ganas puntos".
- Cambios posteriores en reglas deben dejar claro si aplican retroactivamente o
  solo hacia adelante.
- Las categorias subjetivas no deben depender exclusivamente de una respuesta de
  IA.

## Preguntas abiertas

- Finalista significa subcampeon, cualquier equipo finalista o un equipo distinto
  al campeon predicho.
- Como resolver empate en partido con mas goles del dia.
- Si marcador exacto en eliminatorias se evalua en 90 minutos, 120 minutos o
  resultado oficial.
- Como se captura y evalua "penales acertados".
- Cuantos puntos otorgan seleccion con mas goles y mejor defensa.
- Que pasa si admin cambia reglas cuando ya existen predicciones calculadas.

## Recomendacion tecnica

Usar un modelo administrable de reglas de scoring y un flujo de recalculo
deterministico. La IA puede asistir en categorias subjetivas, pero el resultado
final debe quedar persistido y auditable por admin.
