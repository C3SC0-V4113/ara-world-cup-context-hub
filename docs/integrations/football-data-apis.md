# APIs Tradicionales y Fuentes de Datos de Futbol

## Estado

Accepted. Ver
[ADR-0008](../adr/0008-prefer-traditional-apis-for-verifiable-football-data.md)
y [ADR-0012](../adr/0012-propose-d1-schema-options-for-manual-and-api-ingestion.md).

## Principio rector

Cloudflare D1 es la fuente canonica interna de Ara World Cup. Las APIs externas
alimentan datos importados o cacheados; no deben ser llamadas desde la UI como
fuente directa para scoring, ranking o cierres.

Reglas aceptadas:

- API-Football Free es el proveedor deportivo inicial.
- OpenFootball puede usarse como seed estatico gratuito.
- Guardar en D1 los datos necesarios para vistas, scoring, ranking y auditoria.
- Permitir correccion admin cuando el proveedor falle, llegue tarde o entregue
  un dato discutible.
- Mostrar frescura de datos importados en frontend.
- No usar LLMs para scoring, narrativa o resoluciones en esta version.

## Matriz por caso de uso

| Caso de uso | Fuente aceptada | Uso en app | Notas |
| --- | --- | --- | --- |
| Catalogo de equipos, grupos, sedes y banderas | OpenFootball seed; API-Football para IDs externos | Home, predicciones, ranking y admin | Mapear IDs externos a IDs internos estables. |
| Fixtures, proximos juegos y horarios | API-Football Free | Home y Predicciones del dia | Guardar kickoff en UTC y `synced_at`. |
| Resultados y estado | API-Football Free + admin override | Scoring, ranking y admin | Polling conservador y reconciliacion post-partido. |
| Standings, grupos y clasificados | API-Football Free | Predicciones generales y ranking | Usar para clasificados desde grupos. |
| Eventos: goles, tarjetas y penales | API-Football Free si esta disponible | Scoring por partido | Si falta cobertura, degradar la regla o resolver por admin. |
| Estadisticas: tiros al arco | API-Football Free si esta disponible | Scoring y mejor defensa | Usar solo si la metrica esta disponible. |
| Seleccion con mas goles | API-Football Free / D1 | Predicciones generales | Dato objetivo por goles acumulados. |
| Mejor defensa | API-Football Free / D1 | Predicciones generales | Metrica compuesta simple con fallback por disponibilidad. |
| Perfil corporativo y foto | Microsoft Graph | Identidad, avatar y perfil | La foto es opcional; si Graph devuelve 404 se usa fallback. |
| Mensajes o avisos Teams | Fuera de scope | No activo | Controles de notificacion quedan deshabilitados. |
| Sorpresa y decepcion | Fuera de scope | No activo | Eliminadas por ambiguedad. |

## Proveedor aceptado

### API-Football Free

Uso aceptado: proveedor deportivo inicial para fixtures, resultados, standings,
eventos y estadisticas disponibles dentro del plan free.

- Costo aproximado: USD 0/mes.
- Limite: 100 requests/dia.
- Fortalezas: cobertura amplia y endpoints para fixtures, livescore, events,
  statistics, standings y top scorers.
- Limites: requests muy restringidas; se debe probar cobertura exacta del Mundial
  y disponibilidad de estadisticas antes de depender de reglas finas.
- Referencias: [coverage](https://www.api-football.com/coverage) y
  [pricing](https://www.api-football.com/pricing).

### OpenFootball

Uso aceptado: seed estatico gratuito para datos iniciales cuando sea util.

- Costo aproximado: gratuito.
- Fortalezas: simple y versionable.
- Limites: no es live API, no resuelve resultados ni estadisticas en tiempo real.
- Referencia: [OpenFootball World Cup dataset](https://github.com/openfootball/worldcup).

## Proveedores no elegidos para esta fase

- `football-data.org`: queda como referencia alternativa si API-Football Free no
  cubre el alcance minimo.
- `Sportmonks`: queda como alternativa mas robusta si se aprueba presupuesto
  futuro.
- `WorldCupAPI`: no prioritaria por costo.
- `TheSportsDB`: puede servir como referencia/prototipo, no como proveedor
  principal aceptado.

## Politica de sync y cuota

- Maximo recomendado: 80 requests automaticas/dia.
- Reserva recomendada: 20 requests/dia para admin, pruebas o recuperacion.
- Frontend consulta backend/D1; backend consulta API-Football.
- Sync por defecto cada 60 minutos en ventanas de partido.
- Reconciliacion post-partido para resultados, eventos y standings.
- Componentes que muestren datos importados deben mostrar frescura: `synced_at`,
  "actualizado hace X" o badge equivalente.

## Implicaciones de implementacion futura

- Crear tablas D1 para proveedores externos, IDs externos, sync runs, snapshots y
  overrides admin.
- Normalizar respuestas de proveedor hacia entidades internas: teams, matches,
  standings, events y statistics.
- Guardar `source_provider`, `source_id`, `synced_at` y estado de confianza por
  registro importado.
- Proteger la ingestion con cuotas, retries, backoff y observabilidad minima.
- Evitar que scoring lea payloads crudos; debe leer modelos internos ya
  normalizados.
