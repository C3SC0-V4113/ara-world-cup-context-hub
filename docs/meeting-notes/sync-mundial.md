# Sintesis de Reunion: Sync Mundial

## Fuentes

- Transcripcion VTT local: `C:\Users\frank\Downloads\Sync mundial.vtt`
- Resumen ejecutivo provisto por el usuario.
- Repo de aplicacion usado como referencia de estado: `E:\Repositorios\ara-world-cup`

La transcripcion cruda no se versiona en este hub. Este documento conserva una
sintesis operacional.

## Participantes y rol interpretativo

- **Alejandra Alvarado**: fuente de requerimientos de producto, expectativas de
  UX y objetivos de negocio.
- **Walter Zetino**: fuente de recomendaciones tecnicas, riesgos, priorizacion y
  criterios de implementacion.

## Decisiones confirmadas

- Habra dos tipos de predicciones: generales del torneo y diarias por partido.
- Las predicciones generales se completan una vez y luego quedan bloqueadas.
- Las predicciones diarias se pueden modificar hasta 30 minutos antes del inicio
  de cada partido.
- El cierre diario debe evaluarse por partido individual, no por jornada completa.
- Las predicciones y puntos deben almacenarse por usuario.
- Debe existir ranking individual y ranking por areas o equipos internos.
- El ranking general debe mostrar mas que solo el top 3; el top 3 es un resumen
  destacado, no sustituto del ranking completo.
- La gamificacion es parte central de la experiencia.

## Requerimientos de producto expresados por Alejandra

- Predicciones generales: campeon, finalista, seleccion sorpresa, seleccion
  decepcion, seleccion con mas goles y mejor defensa.
- Predicciones del dia: ganador, empate, marcador y ambos equipos anotan.
- Mecanica diaria adicional: partido con mas goles del dia.
- Home con resumen de partidos, predicciones pendientes, selecciones favoritas,
  ranking resumido y resultados recientes.
- Posible recordatorio para partidos importantes o favoritos.
- Uso de selecciones favoritas para identificar partidos con mayor interes para
  las personas de la empresa.
- Ranking con dinamica tipo Kahoot: mensajes sobre aciertos, movimientos y
  desempeno.

## Sugerencias tecnicas de Walter

- Separar la UX de una jornada diaria de la logica de bloqueo por partido.
- Mostrar partidos ya cerrados como no editables y permitir editar los partidos
  futuros del mismo dia.
- Usar un panel administrativo para resolver resultados y categorias subjetivas,
  evitando depender exclusivamente de un API o de una persona.
- Considerar IA/internet como apoyo para categorias subjetivas, pero con
  supervision y correccion manual.
- Priorizar gamificacion, predicciones y ranking antes de calendario y
  recordatorios avanzados.
- Evitar que el boton "Recordarme" se interprete como permiso para bloquear
  agenda o ver partidos durante reuniones.
- Preferir mensajes de Teams o un grupo dedicado antes que integracion pesada con
  calendarios corporativos.

## Acciones derivadas

- Cerrar tabla final de puntuacion.
- Definir modelo de datos completo para usuarios, predicciones, reglas, resultados,
  ranking y areas.
- Confirmar origen y flujo de resultados del torneo.
- Definir resolucion de categorias subjetivas y responsables de correccion manual.
- Definir si areas/equipos internos seran configurables.
- Definir narrativa de gamificacion basada en eventos reales.
- Origen de foto de perfil: en la reunion quedo como tema por confirmar; ADR
  0007 lo resuelve posteriormente a favor de Microsoft con fallback visual.

## Riesgos

- Las categorias subjetivas pueden generar controversia si no hay criterio claro.
- IA/internet puede alucinar o producir respuestas inconsistentes.
- Un cierre visual por jornada puede confundir si la logica real es por partido.
- APIs externas pueden fallar o tener datos incompletos.
- Ranking por areas puede complicarse si los grupos son flexibles.
- Mensajes o recordatorios pueden malinterpretarse como permisos laborales.

## Notas de confianza

- Alta confianza: dos tipos de predicciones, bloqueo 30 minutos antes, ranking
  individual y por area, foco en gamificacion.
- Confianza media: valores especificos de puntuacion y alcance exacto de IA.
- Confianza baja: integraciones definitivas de calendario o recordatorios, porque
  se discutieron como ideas y no como foco inmediato.
