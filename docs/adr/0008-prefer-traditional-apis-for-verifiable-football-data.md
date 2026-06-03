# ADR-0008: Preferir APIs tradicionales para datos verificables de futbol

## Estado

Proposed

## Contexto

Ara World Cup necesita datos de futbol para catalogos, calendario, horarios,
resultados, standings, eventos de partido, estadisticas y categorias de scoring.
Hasta ahora el enfoque de implementacion se ha guiado por cargar y actualizar
valores internamente desde la vista admin, mientras que algunos datos ambiguos se
habian propuesto como candidatos a resolucion asistida por IA.

Esa estrategia es insuficiente para datos objetivos porque eleva el esfuerzo
manual, aumenta riesgo de error y puede hacer que la IA opere como fuente final
para hechos verificables. La decision vigente de plataforma indica que
Cloudflare D1 es la fuente canonica interna, por lo que cualquier dato externo
debe normalizarse e importarse o cachearse antes de alimentar UI, scoring o
ranking.

## Decision

Se propone usar APIs tradicionales y datasets verificables como fuente preferida
para datos objetivos del Mundial:

- catalogo de selecciones, grupos, sedes y banderas;
- fixtures, proximos partidos, kickoff y horarios;
- resultados, estado del partido y live scores;
- standings, grupos y brackets;
- eventos como goles, tarjetas, cambios y penales;
- estadisticas como tiros, corners, posesion, faltas y xG si el proveedor lo
  ofrece;
- top scorers, seleccion con mas goles y mejor defensa cuando pueda derivarse de
  datos objetivos.

Si esta propuesta se aprueba, los datos externos se deberian guardar o cachear en
**Cloudflare D1** antes de ser usados por vistas, scoring o ranking. La app no
deberia depender de llamadas directas desde UI a proveedores externos para
decisiones de puntuacion.

La vista admin deberia poder corregir datos importados. Las correcciones admin
ganarian sobre la API externa y deberian quedar auditables.

La IA quedaria limitada a asistencia para categorias ambiguas, narrativa o
explicacion, usando datos normalizados como evidencia. La IA no deberia escribir
resultados finales de scoring ni resolver datos objetivos sin validacion admin.

## Alternativas consideradas

### Solo carga manual/admin

- Ventajas: control completo y baja dependencia externa.
- Desventajas: mucho trabajo operativo durante el Mundial, riesgo de errores y
  retraso en resultados o rankings.

### IA como fuente primaria

- Ventajas: flexible para categorias subjetivas y texto narrativo.
- Desventajas: puede alucinar, mezclar fuentes, no garantiza consistencia y no
  es apropiada para hechos verificables que afectan puntos.

### APIs tradicionales + D1 + override admin

- Ventajas: reduce trabajo manual, mejora trazabilidad, permite cache local,
  mantiene control admin y limita IA a casos adecuados.
- Desventajas: requiere elegir proveedor, manejar cuotas, normalizar respuestas,
  crear jobs de ingestion y monitorear cambios de API.

## Consecuencias

### Positivas

- Scoring y ranking se basan en datos objetivos y trazables.
- D1 conserva una copia canonica que desacopla la app de caidas o cambios de
  proveedores externos.
- Admin puede corregir errores sin esperar al proveedor.
- La IA se usa donde aporta valor: categorias ambiguas y narrativa.

### Negativas

- Se debe implementar una capa de ingestion/sync.
- Hay costo potencial si se requieren livescores, eventos o estadisticas ricas.
- Se deben vigilar limites de rate, calidad de cobertura y cambios de contrato.
- Los IDs externos de equipos, partidos y competiciones deben mapearse contra el
  modelo interno.

## Notas de implementacion

- Usar OpenFootball para seed estatico gratuito cuando aplique.
- Evaluar `football-data.org` como baseline barato para fixtures, schedules,
  standings y live scores.
- Evaluar `API-Football` si se requieren eventos, estadisticas, lineups, top
  scorers o endpoints mas amplios a bajo costo.
- Evaluar Sportmonks si el presupuesto justifica datos mas profundos y robustos.
- No priorizar WorldCupAPI salvo que su especializacion compense el costo.
- Registrar proveedor, timestamp de sincronizacion, payload relevante o checksum,
  y cualquier override admin.

## Fuente de la decision/propuesta

- Requerimiento nuevo del usuario: buscar soluciones con APIs tradicionales
  siempre que los datos sean verificables. El usuario aclaro posteriormente que
  esta peticion aun no esta aprobada y debe tratarse como propuesta.
- ADR-0004: scoring y resoluciones deben poder administrarse o corregirse
  manualmente.
- ADR-0006: Cloudflare D1 es la base de datos objetivo y fuente canonica interna.
- Criterio tecnico del hub: evitar depender exclusivamente de IA para datos
  objetivos que impactan scoring y ranking.
