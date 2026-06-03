# Propuestas UX y Arquitectura por Vista

## Estado

Proposed / no aprobado.

Fuente: criterio tecnico del usuario.

Este documento registra dudas y propuestas importantes por vista. No representa
decisiones cerradas. Cuando una propuesta tenga impacto en scoring, ranking,
integraciones o modelo de datos, debe evaluarse con ADR antes de implementarse.

## Predicciones

| Tema | Tipo | Propuesta | Estado |
| --- | --- | --- | --- |
| Orden de predicciones diarias | UX | Definir una forma mas granular de ordenar y priorizar predicciones diarias. Hoy se perciben juntas, sin orden ni prioridad clara. | Propuesta |
| Protagonismo de predicciones generales | UX | Reducir el espacio visual de predicciones generales despues del setup inicial. No deberian ocupar mas de la mitad de la vista si ya estan bloqueadas y no se vuelven a tocar. | Propuesta |
| Foco de scoring diario | Scoring | Refactorizar el sistema de puntos para que el peso de partidos diarios sea mayor. Varias reglas actuales son generales o dependen del final del torneo. | Propuesta |
| Comunicacion de reglas | UX / scoring | La vista debe explicar mejor que puntos se ganan hoy, que puntos son de largo plazo y que predicciones ya no requieren accion. | Pregunta abierta |

## Ranking

| Tema | Tipo | Propuesta | Estado |
| --- | --- | --- | --- |
| Ranking por grupos normalizado | Arquitectura / scoring | No usar solo suma bruta si los grupos tienen tamanos distintos. Evaluar normalizacion para que un grupo grande no tenga ventaja automatica sobre uno pequeno. | Propuesta |
| Formula de normalizacion | Pregunta abierta | Evaluar promedio por participante activo, promedio top N o ponderacion por participacion. No elegir formula hasta validar justicia y entendibilidad. | Pregunta abierta |
| Log de puntos | UX / arquitectura | Crear una vista o subvista propia para mostrar como el usuario gano puntos y mover ahi la card de "tu desempeno". | Propuesta |
| Separacion global/personal | UX | Separar ranking en dos subvistas: ranking global y desempeno personal. | Propuesta |
| Tabla avanzada | UX | Agregar paginado, sort y filtros dentro de la tabla de ranking. Antecedente: solicitud de Alejandra; refuerzo: criterio tecnico del usuario. | Propuesta |
| Boton recordar / Teams | Integracion | Conectar accion de recordar con un flujo de mensaje o aviso en Teams, si se aprueba el flujo de integracion. | Propuesta |

## Home

| Tema | Tipo | Propuesta | Estado |
| --- | --- | --- | --- |
| Recordar por Teams | Integracion / UX | Permitir que la opcion de recordar dispare un mensaje a grupo o canal de Teams mediante bot o Microsoft Graph. | Propuesta |
| Badges de selecciones favoritas | UX / datos | Decidir si todos los badges usan un estilo estandar o si cada pais conserva un color principal propio. | Pregunta abierta |
| Predicciones del dia desde Home | UX | Definir que Home no guarde predicciones como quick action. La accion deberia redirigir a la vista Predicciones para completar el flujo ahi. | Propuesta |

## Implicaciones tecnicas

- Las propuestas de predicciones se relacionan con ADR-0009.
- Las propuestas de ranking se relacionan con ADR-0010.
- Las propuestas de Teams y recordatorios se relacionan con ADR-0011.
- Ninguna propuesta debe implementarse como decision cerrada sin aprobacion
  posterior.

