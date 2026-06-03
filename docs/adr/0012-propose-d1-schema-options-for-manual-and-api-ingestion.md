# ADR-0012: Proponer opciones de schema D1 para ingesta manual y API-Football

## Estado

Proposed

No aprobado.

## Contexto

La decision vigente de plataforma indica que Cloudflare D1 es la base de datos
objetivo. Todavia no hay una decision cerrada sobre si los datos deportivos se
cargaran principalmente por admin o mediante ingestion de APIs externas.

ADR-0008 propone preferir APIs tradicionales para datos verificables, pero sigue
en estado `Proposed`. Por eso se documentan dos schemas comparables: uno de
ingesta manual/admin con asistencia de IA y otro con ingestion API-Football.

## Decision

Se propone documentar dos modelos D1:

- Modelo A: ingesta manual/admin, donde admin carga datos deportivos y aprueba
  resoluciones asistidas por IA.
- Modelo B: ingestion API-Football, donde API-Football alimenta fixtures, live,
  events, statistics y top scorers; OpenFootball alimenta seed inicial; Microsoft
  Graph alimenta identidad/avatar/Teams.

API-Football queda como proveedor favorito para el modelo API por costo y amplitud
de endpoints, pero no queda aprobado como proveedor final.

## Alternativas consideradas

### Solo schema manual

- Ventajas: menor complejidad e independencia de proveedores deportivos.
- Desventajas: mayor carga admin y mas riesgo de retrasos durante partidos.

### Solo schema con API externa

- Ventajas: reduce carga manual y mejora acceso a eventos/estadisticas.
- Desventajas: requiere cuotas, polling, normalizacion, mapeos y manejo de
  conflictos.

### Documentar ambos modelos comparables

- Ventajas: permite decidir por costo, operacion y madurez sin bloquear el diseno
  D1.
- Desventajas: aumenta volumen documental y puede requerir reconciliar modelos
  antes de implementar.

## Consecuencias

### Positivas

- El equipo puede comparar costo operativo manual contra costo de API.
- El schema canonico puede disenarse para soportar transicion de manual a API.
- Se evita convertir ADR-0008 en decision aceptada por accidente.

### Negativas

- Todavia queda pendiente elegir modelo final.
- El modelo API requiere mas tablas y procesos de sync.
- El modelo manual puede quedarse corto si se busca live scoring o estadisticas.

## Fuente de la decision/propuesta

- Criterio tecnico del usuario.
- ADR-0006: Cloudflare D1 es la base de datos objetivo.
- ADR-0008: propuesta de APIs tradicionales para datos verificables.
- Guia de costos y proveedores en `docs/integrations/football-data-apis.md`.

