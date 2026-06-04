# ADR-0008: Usar APIs tradicionales para datos verificables de futbol

## Estado

Accepted

## Contexto

Ara World Cup necesita datos de futbol para catalogos, calendario, horarios,
resultados, standings, eventos de partido, estadisticas y categorias de scoring.
El enfoque manual/admin reduce dependencia externa, pero eleva carga operativa
durante el Mundial y aumenta riesgo de errores o retrasos al cerrar puntos.

La decision vigente de plataforma indica que **Cloudflare D1** es la fuente
canonica interna. Por eso los datos externos no deben alimentar scoring, ranking
o UI directamente desde el frontend. Deben importarse, normalizarse o cachearse
en D1 antes de usarse en la app.

Tambien se decidio no usar LLMs para esta version. Las categorias ambiguas como
seleccion sorpresa y seleccion decepcion quedan fuera de alcance.

## Decision

Usar APIs tradicionales y datasets verificables como fuente preferida para datos
objetivos del Mundial.

La arquitectura aprobada es:

- **API-Football Free** como proveedor deportivo inicial, con limite de 100
  requests/dia.
- **OpenFootball** como seed inicial opcional para datos estaticos cuando sea
  util.
- **Cloudflare D1** como fuente canonica interna para UI, scoring, ranking,
  overrides y auditoria.
- Frontend hablando frecuentemente con el backend, pero nunca llamando directo a
  API-Football.
- Backend sincronizando periodicamente con API-Football en intervalos
  conservadores para no exceder el limite free.
- Vistas que muestren datos importados deben exponer frescura con `synced_at`,
  "actualizado hace X" o badge equivalente.
- Admin puede corregir datos importados; el override admin gana sobre el dato de
  proveedor y debe quedar auditable.

Datos objetivos cubiertos por esta decision:

- catalogo de selecciones, grupos, sedes y banderas;
- fixtures, proximos partidos, kickoff y horarios;
- resultados, estado del partido y live scores si el limite free lo permite;
- standings, grupos y clasificados;
- eventos simples como goles, tarjetas, penales y otros datos disponibles;
- estadisticas simples como tiros al arco si el proveedor las entrega;
- seleccion con mas goles y mejor defensa cuando puedan derivarse de datos
  objetivos.

## Alternativas consideradas

### Solo carga manual/admin

- Ventajas: control completo y baja dependencia externa.
- Desventajas: mucho trabajo operativo durante el Mundial, riesgo de errores y
  retraso en resultados o rankings.

### LLMs o IA como fuente primaria

- Ventajas: flexibilidad para narrativa y categorias subjetivas.
- Desventajas: puede alucinar, mezclar fuentes y no garantiza consistencia para
  hechos verificables. Se rechazo para esta version.

### APIs tradicionales + D1 + override admin

- Ventajas: reduce trabajo manual, mejora trazabilidad, permite cache local y
  mantiene control admin.
- Desventajas: requiere controlar cuotas, normalizar respuestas, mapear IDs
  externos y monitorear errores de sync.

## Consecuencias

### Positivas

- Scoring y ranking se basan en datos objetivos y trazables.
- D1 desacopla la app de caidas o cambios de proveedor.
- El frontend puede consultar backend con frecuencia sin quemar cuota externa.
- Admin conserva control final mediante overrides auditables.
- Se elimina ambiguedad operativa al dejar fuera de alcance LLMs, sorpresa y
  decepcion.

### Negativas

- La version free de API-Football exige una estrategia estricta de bajo polling.
- Algunas metricas o eventos pueden no estar disponibles con suficiente
  cobertura.
- Los IDs externos de equipos, partidos y competiciones deben mapearse contra el
  modelo interno.
- Se necesitan `sync_runs`, frescura visible y manejo de errores para explicar
  datos desactualizados.

## Notas de implementacion

- Reservar margen de cuota: maximo recomendado de 80 requests automaticas/dia y
  20 requests para admin, pruebas o recuperacion manual.
- Usar polling conservador: por defecto cada 60 minutos en ventanas de partido y
  reconciliacion post-partido.
- Guardar `source_provider`, `source_id`, `synced_at` y estado de confianza en
  registros importados.
- Evitar que scoring lea payloads crudos; scoring debe leer modelos internos ya
  normalizados.
- Mostrar frescura en componentes que dependan de datos importados.

## Fuente de la decision

- Decision posterior del usuario: aprobar la propuesta de APIs externas usando
  API-Football Free, mantener OpenFootball como seed opcional, no usar LLMs y
  conservar D1 como fuente canonica local.
- ADR-0004: scoring y resoluciones deben poder administrarse o corregirse
  manualmente.
- ADR-0006: Cloudflare D1 es la base de datos objetivo y fuente canonica interna.
