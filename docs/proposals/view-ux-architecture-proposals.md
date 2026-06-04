# Decisiones UX y Arquitectura por Vista

## Estado

Accepted.

Fuente: ADR-0009, ADR-0010 y ADR-0011.

## Predicciones

| Tema | Tipo | Decision | Estado |
| --- | --- | --- | --- |
| Tabs de predicciones | UX | Separar la vista en `Predicciones del dia` como tab principal y `Predicciones generales` como tab secundario. | Accepted |
| Orden de predicciones del dia | UX | Ordenar y priorizar por urgencia, estado y kickoff. | Accepted |
| Protagonismo de predicciones generales | UX | Mantenerlas accesibles, pero secundarias despues del setup inicial. | Accepted |
| Foco de scoring diario | Scoring | Aumentar formas simples de ganar puntos por partido. | Accepted |
| Comunicacion de reglas | UX / scoring | Explicar puntos diarios, puntos globales y estado bloqueado/editable. | Accepted |

## Ranking

| Tema | Tipo | Decision | Estado |
| --- | --- | --- | --- |
| Ranking individual | UX / scoring | Mantener como vista principal de competencia individual. | Accepted |
| Ranking por grupos normalizado | Arquitectura / scoring | Usar promedio de puntos por participante activo. | Accepted |
| Ranking por grupos sin normalizacion | UX / scoring | Mostrar suma bruta de puntos como vista complementaria. | Accepted |
| Log de puntos | UX / arquitectura | Crear vista o subvista personal para explicar puntos y aciertos. | Accepted |
| Tabla avanzada | UX | Agregar filtros y sort dentro de la tabla de ranking. | Accepted |
| Rankings por tipo de prediccion | UX | Eliminar rankings por prediccion semanal, general, preccion general o prediccion del dia. | Accepted |

## Home

| Tema | Tipo | Decision | Estado |
| --- | --- | --- | --- |
| Predicciones del dia desde Home | UX | Home redirige a la vista Predicciones; no guarda predicciones como quick action. | Accepted |
| Notificaciones / Teams | Integracion | Fuera de scope; controles relacionados quedan deshabilitados. | Accepted |
| Badges de selecciones favoritas | UX / datos | Pendiente: estilo estandar o color principal por pais. | Pregunta abierta |

## Implicaciones tecnicas

- Las decisiones de predicciones se relacionan con ADR-0009.
- Las decisiones de ranking se relacionan con ADR-0010.
- Las decisiones de Home/notificaciones se relacionan con ADR-0011.
- Estas decisiones requieren actualizar el repo de aplicacion en una fase
  posterior; este hub solo documenta el contexto aceptado.
