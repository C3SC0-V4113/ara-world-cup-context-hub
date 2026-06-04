# ADR-0012: Usar schema D1 para ingestion API-Football Free

## Estado

Accepted

## Contexto

Cloudflare D1 es la base de datos objetivo. Ya no queda abierta la comparacion
principal entre ingesta manual y APIs externas: se decidio usar arquitectura con
API-Football Free, seed opcional de OpenFootball, cache local en D1 y sync
conservador.

El modelo manual/admin se conserva como fallback operativo y como mecanismo de
overrides, no como fuente primaria de datos deportivos.

## Decision

Usar un schema D1 orientado a ingestion externa con API-Football Free:

- tablas canonicas para usuarios, grupos internos, selecciones, sedes, partidos,
  predicciones, reglas, score events, rankings, overrides y auditoria;
- tablas de ingestion para proveedores, mapeos de IDs externos, sync runs,
  payloads relevantes, eventos, estadisticas, standings, top scorers y
  conflictos;
- campos de frescura como `synced_at`, `source_provider`, `source_id` y estado
  de confianza cuando aplique;
- admin overrides para corregir datos importados y resolver conflictos.

El plan free de API-Football obliga a limitar polling:

- maximo recomendado de 80 requests automaticas/dia;
- 20 requests/dia reservadas para admin, pruebas o recuperacion;
- sync por defecto cada 60 minutos en ventanas de partido;
- reconciliacion post-partido para resultados, eventos y standings;
- frontend consulta backend/D1, nunca API-Football directo.

No se usaran LLMs ni tablas de resolucion IA en el modelo objetivo.

## Alternativas consideradas

### Solo schema manual

- Ventajas: menor complejidad e independencia de proveedores deportivos.
- Desventajas: mayor carga admin y mas riesgo de retrasos durante partidos.

### Planes pagos de API-Football

- Ventajas: mayor margen de requests y polling mas frecuente.
- Desventajas: costo mensual. Se descarta para la decision actual porque se
  aprobo usar estrictamente el plan free.

### Schema API-Football Free + D1

- Ventajas: costo externo cero, datos objetivos verificables y menor carga
  manual que el modelo admin puro.
- Desventajas: restricciones fuertes de cuota, menor frescura y dependencia de
  cobertura del plan free.

## Consecuencias

### Positivas

- El equipo trabaja con el modelo de arquitectura elegido.
- D1 conserva los datos necesarios para UI, scoring y ranking.
- La app puede mostrar frescura de datos y seguir operando aunque la API externa
  este temporalmente inaccesible.
- Admin conserva capacidad de correccion mediante overrides.

### Negativas

- La cuota free puede limitar live scores y estadisticas ricas.
- El sync debe ser cuidadoso para no agotar 100 requests/dia.
- Payloads crudos deben guardarse solo cuando aporten auditoria/debug, con
  politica de retencion.
- Algunas mecanicas de scoring por partido pueden quedar condicionadas a datos
  disponibles por API-Football Free.

## Fuente de la decision

- Decision posterior del usuario: elegir arquitectura con APIs externas usando
  estrictamente API-Football Free, mantener seed opcional de OpenFootball, bajo
  polling y D1 como base local.
- ADR-0006: Cloudflare D1 es la base de datos objetivo.
- ADR-0008: APIs tradicionales aceptadas para datos verificables.
