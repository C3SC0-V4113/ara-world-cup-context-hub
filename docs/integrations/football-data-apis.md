# Propuesta de APIs Tradicionales y Fuentes de Datos de Futbol

## Estado

Propuesta no aprobada. Este documento conserva investigacion y opciones para
evaluacion, pero no define una decision vigente de implementacion. Ver
[ADR-0008](../adr/0008-prefer-traditional-apis-for-verifiable-football-data.md).

## Principio rector

Si se aprueba la propuesta, Cloudflare D1 seguiria siendo la fuente canonica
interna de Ara World Cup. Las APIs externas servirian para alimentar datos
importados o cacheados; no deberian ser llamadas desde la UI como fuente directa
para scoring, ranking o cierres.

Reglas propuestas:

- Para datos objetivos y verificables, buscar primero API tradicional, dataset o
  fuente estructurada.
- Guardar en D1 los datos necesarios para vistas, scoring, ranking y auditoria.
- Permitir correccion admin cuando el proveedor falle, llegue tarde o entregue
  un dato discutible.
- Usar IA solo para asistencia en categorias ambiguas o narrativa, tomando como
  evidencia los datos ya normalizados.
- Validar costos, cobertura exacta y terminos antes de contratar porque los
  planes pueden cambiar.

## Matriz por caso de uso

| Caso de uso | Fuente recomendada | Uso en app | Notas |
| --- | --- | --- | --- |
| Catalogo de equipos, grupos, sedes y banderas | OpenFootball para seed; API-Football o football-data.org para IDs externos | Home, predicciones, ranking y admin | El seed debe mapearse a IDs internos estables y a IDs de proveedor. |
| Fixtures, proximos juegos y horarios | football-data.org como baseline; API-Football si se necesita mas metadata | Home, predicciones diarias y calendario | Guardar kickoff en UTC y mostrar segun zona del usuario o empresa. |
| Resultados y estado en vivo | football-data.org con livescores o API-Football Pro/Ultra | Scoring, ranking y admin | Cachear con polling controlado para no depender de UI ni exceder cuotas. |
| Standings, grupos y brackets | football-data.org, API-Football o Sportmonks | Ranking narrativo, home y admin | Puede alimentar mensajes de avance, eliminacion y desempeno. |
| Eventos: goles, tarjetas, cambios y penales | API-Football, football-data.org Deep Data o Sportmonks | Scoring avanzado y auditoria | Necesario si se puntuan goleadores, penales, tarjetas o eventos finos. |
| Estadisticas: posesion, tiros, corners, faltas, xG | API-Football, football-data.org Statistic Add-On o Sportmonks | Categorias derivadas y narrativa | xG no esta garantizado en todos los proveedores; validar cobertura. |
| Top scorers, mas goles, mejor defensa | API-Football, football-data.org Deep Data o Sportmonks | Predicciones generales y scoring | Debe derivarse de datos objetivos antes de usar IA. |
| Perfil corporativo y foto | Microsoft Graph | Identidad, avatar y perfil | La foto es opcional; si Graph devuelve 404 se usa fallback. |
| Mensajes o avisos en Teams | Microsoft Graph | Recordatorios o anuncios puntuales | Teams debe usarse para mensajes utiles, no como log completo de eventos. |
| Sorpresa y decepcion | Datos normalizados + criterio admin + IA asistiva | Resolucion subjetiva | IA puede proponer argumentos, pero admin decide el resultado final. |

## Shortlist priorizado

### OpenFootball

Uso recomendado: seed estatico gratuito para Mundial 2026 y mundiales
historicos.

- Costo aproximado: gratuito, dataset publico en GitHub.
- Fortalezas: simple, versionable, util para inicializar equipos, grupos,
  fixtures base o datos historicos.
- Limites: no es live API, no resuelve resultados en tiempo real ni estadisticas
  avanzadas.
- Referencia: [OpenFootball World Cup dataset](https://github.com/openfootball/worldcup).

### football-data.org

Uso recomendado: primera opcion gratis/barata para fixtures, schedules, league
tables y live scores si la cobertura del Mundial satisface el alcance.

- Costo aproximado: Free EUR 0/mes; livescores desde EUR 12/mes; Deep Data desde
  EUR 29/mes; Statistic Add-On EUR 15/mes encima de un plan regular.
- Fortalezas: Worldcup aparece en Free Tier, fixtures/schedules/tables en plan
  gratis, live scores baratos, add-on de estadisticas con posesion, tiros,
  corners, faltas y tarjetas.
- Limites: el plan gratis tiene scores y schedules delayed; eventos/lineups y
  estadisticas requieren planes o add-ons.
- Referencias: [coverage](https://www.football-data.org/coverage) y
  [pricing](https://www.football-data.org/pricing).

### API-Football

Uso recomendado: candidato principal si hacen falta eventos, estadisticas,
lineups, top scorers o mayor amplitud de endpoints a bajo costo.

- Costo aproximado: Free USD 0/mes con 100 requests/dia; Pro USD 19/mes con
  7,500 requests/dia; Ultra USD 29/mes; Mega USD 39/mes.
- Fortalezas: World Cup aparece en cobertura; todos los planes listan endpoints
  como standings, teams, livescore, fixtures, events, lineups, top scorers,
  statistics y predictions.
- Limites: el plan free esta limitado por requests y temporadas disponibles; se
  debe probar cobertura exacta de Mundial 2026 antes de depender de eventos o
  estadisticas.
- Referencias: [coverage](https://www.api-football.com/coverage) y
  [pricing](https://www.api-football.com/pricing).

### TheSportsDB

Uso recomendado: prototipo, artwork, busquedas y livescore economico si la
cobertura exacta del Mundial es suficiente.

- Costo aproximado: Free USD 0/mes con limites; Single Developer USD 9/mes con
  2 min livescore y mayor limite.
- Fortalezas: API JSON gratuita, datos y artwork, opcion premium barata.
- Limites: validar cobertura del Mundial, exactitud de eventos y suficiencia para
  scoring antes de usarlo como proveedor primario.
- Referencias: [API](https://www.thesportsdb.com/api.php) y
  [pricing](https://www.thesportsdb.com/pricing).

### Sportmonks

Uso recomendado: opcion mas robusta si el presupuesto permite datos profundos,
brackets, squads, eventos, estadisticas y mayor soporte.

- Costo aproximado: planes generales desde EUR 29/mes; API World Cup desde
  EUR 69/mes; widgets World Cup desde EUR 78/mes.
- Fortalezas: planes profesionales, 14 dias de trial, cobertura de fixtures,
  live scores, events, standings, brackets, player stats y estadisticas
  avanzadas.
- Limites: mas caro que las opciones baseline; validar si se necesita full API o
  widgets.
- Referencia: [Sportmonks plans](https://www.sportmonks.com/football-api/world-plan/).

### WorldCupAPI

Uso recomendado: no prioritaria por costo, pero util como referencia de proveedor
especializado.

- Costo aproximado: EUR 499/mes.
- Fortalezas: enfoque especializado en World Cup 2026, near real time live
  scores, schedule, commentary, events, statistics, lineups, squads, standings,
  top scorers y datos historicos.
- Limites: costo alto para una app interna si existen alternativas suficientes.
- Referencia: [WorldCupAPI pricing](https://worldcupapi.com/pricing/).

### Microsoft Graph

Uso recomendado: perfil corporativo, foto de usuario y avisos puntuales en Teams.

- Costo aproximado: depende del licenciamiento Microsoft 365 existente.
- Fortalezas: fuente natural para identidad corporativa, foto de perfil y canal
  de comunicacion Teams.
- Limites: requiere permisos correctos del tenant, consentimiento y manejo de
  errores cuando no haya foto.
- Referencias: [profile photo](https://learn.microsoft.com/en-us/graph/api/profilephoto-get?view=graph-rest-1.0)
  y [Teams channel message](https://learn.microsoft.com/en-us/graph/api/channel-post-messages?view=graph-rest-1.0).

## Recomendacion propuesta por defecto

Baseline barato:

1. Usar OpenFootball para seed inicial versionable.
2. Usar football-data.org para fixtures, schedules, standings y resultados si la
   cobertura y latencia son suficientes.
3. Mantener vista admin para correcciones, cierres y overrides.

Si hacen falta eventos y estadisticas reales:

1. Probar API-Football como candidato principal por costo y amplitud de endpoints.
2. Cachear fixtures, events, statistics, top scorers y standings en D1.
3. Definir polling distinto para dias sin partidos, prepartido, partido en vivo y
   postpartido.

Si prima confiabilidad/profundidad del Mundial:

1. Evaluar Sportmonks por trial y costo mensual durante el torneo.
2. Contratar solo si la diferencia de cobertura justifica el gasto.

WorldCupAPI queda como alternativa especializada de alto costo, no como primera
opcion.

## Uso de IA

IA no debe resolver datos objetivos como marcador final, ganador, top scorer,
equipo con mas goles o mejor defensa si estos pueden derivarse de API o admin.

IA puede asistir en:

- redactar narrativa de gamificacion basada en datos reales;
- proponer candidatos a seleccion sorpresa o decepcion;
- explicar por que un equipo califica como sorpresa/decepcion;
- resumir cambios de ranking o rachas.

Para sorpresa/decepcion, la resolucion recomendada es:

1. Calcular senales: ranking inicial, expectativa previa, fase alcanzada,
   diferencia de desempeno, resultados contra favoritos y odds si estan
   disponibles.
2. Pedir a IA una propuesta explicada usando esas senales.
3. Mostrar propuesta y evidencia en admin.
4. Guardar resolucion final aprobada por admin en D1.

## Implicaciones de implementacion futura

- Crear tablas D1 para proveedores externos, IDs externos, sync runs, snapshots y
  overrides admin.
- Normalizar respuestas de proveedor hacia entidades internas: teams, matches,
  standings, events y statistics.
- Guardar `source_provider`, `source_id`, `synced_at` y estado de confianza por
  registro importado.
- Proteger la ingestion con quotas, retries, backoff y observabilidad minima.
- Evitar que scoring lea payloads crudos; debe leer modelos internos ya
  normalizados.
- Documentar cualquier proveedor elegido con ADR tecnico en el repo de
  aplicacion si cambia arquitectura o costos.
