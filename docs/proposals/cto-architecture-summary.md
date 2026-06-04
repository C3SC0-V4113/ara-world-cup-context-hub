# Summary para CTO: Propuesta de Arquitectura Ara World Cup

## Estado

Proposed / no aprobado.

Este documento resume la propuesta que se presentara al CTO. No cambia el estado
de ningun ADR y no define una decision cerrada de implementacion.

## Resumen ejecutivo

La arquitectura vigente de Ara World Cup ya tiene una base aceptada:

- **Cloudflare D1** como base de datos objetivo y fuente canonica interna.
- **Wrangler** para configuracion, migraciones y bindings.
- **Cloudflare** como despliegue primario de la web.
- **Vercel** solo como fallback si Cloudflare bloquea el avance.
- **Microsoft Entra ID / Graph** como identidad corporativa, perfil y avatar.
- Roles base **`user`** y **`admin`**, con acciones admin protegidas en servidor.

La decision pendiente es como alimentar y operar los datos deportivos del Mundial.
Hay dos modelos comparables:

- **Modelo A: ingesta manual/admin.** Admin carga equipos, partidos, resultados,
  reglas y resoluciones. La IA puede asistir categorias ambiguas o narrativa,
  pero admin aprueba antes de persistir.
- **Modelo B: ingestion con APIs.** APIs tradicionales alimentan fixtures,
  resultados, eventos, estadisticas y top scorers. D1 guarda datos normalizados,
  sync runs, payloads relevantes, conflictos y overrides admin.

**Recomendacion propuesta:** usar el **Modelo B como arquitectura objetivo** para
reducir carga operativa y mejorar trazabilidad de datos verificables, manteniendo
el **Modelo A como fase minima/fallback operativo** y como control admin final.

## Comparacion de propuestas

| Criterio | Modelo A: manual/admin | Modelo B: APIs + D1 |
| --- | --- | --- |
| Control operativo | Alto, todo pasa por admin. | Alto si se conservan overrides admin. |
| Carga durante partidos | Alta: resultados y cierres dependen de carga manual. | Media/baja: sync automatiza datos objetivos. |
| Riesgo de error | Alto si hay carga bajo presion. | Medio: depende de proveedor, mapeos y normalizacion. |
| Live scores/eventos | Limitado o manual. | Viable con proveedor y polling controlado. |
| Scoring/ranking | Auditable si admin mantiene disciplina operativa. | Mas trazable si scoring lee datos normalizados y score events. |
| Costo externo | Bajo, sin API deportiva obligatoria. | Bajo/medio; API-Football Pro esta propuesto desde USD 19/mes. |
| Complejidad tecnica | Menor al inicio. | Mayor: sync workers, cuotas, retries, mapeos y conflictos. |
| Escalabilidad operativa | Depende de capacidad admin. | Mejor para torneo en vivo, si se monitorean cuotas y errores. |

## Clasificacion del contexto

### Requerimientos de producto

- El usuario debe poder participar diariamente con predicciones de partidos.
- El ranking debe ser claro, competitivo y percibido como justo.
- La app debe integrarse naturalmente al contexto corporativo Microsoft.
- Admin debe poder operar resultados, reglas, cierres y correcciones.
- Los datos que afectan puntos deben ser explicables y auditables.

### Sugerencias tecnicas

- Usar APIs tradicionales o datasets verificables para hechos objetivos del
  Mundial.
- Guardar o cachear los datos externos en D1 antes de usarlos en UI, scoring o
  ranking.
- Usar OpenFootball como seed gratuito cuando aplique.
- Evaluar API-Football como proveedor principal por costo y amplitud de endpoints.
- Mantener payloads, sync runs, provider mappings y provider conflicts para
  trazabilidad y debug.
- Usar IA solo como asistencia para narrativa o categorias ambiguas, con
  validacion admin.

### Decisiones aceptadas que condicionan la propuesta

- ADR-0004 acepta control admin de scoring y resolucion de resultados.
- ADR-0006 acepta Cloudflare D1 y despliegue primario en Cloudflare.
- ADR-0007 acepta identidad corporativa Microsoft y acceso por roles.
- El hub documental sigue siendo la memoria de producto y arquitectura segun
  ADR-0001.

### Propuestas no aprobadas incluidas en ADRs

- **ADR-0008:** preferir APIs tradicionales para datos verificables de futbol y
  limitar IA a categorias ambiguas o narrativa.
- **ADR-0009:** rebalancear Predicciones para dar mas peso a la accion diaria y
  revisar el peso relativo del scoring diario.
- **ADR-0010:** normalizar ranking grupal y separar una vista de desempeno
  personal con desglose de puntos.
- **ADR-0011:** usar Home como resumen/navegacion y conectar recordatorios utiles
  a Teams, sin duplicar el flujo de predicciones.
- **ADR-0012:** comparar dos schemas D1: ingesta manual/admin e ingestion con
  API-Football; API-Football es favorito propuesto, no proveedor aprobado.

### Decisiones propuestas que debe definir el CTO

| ADR | Decision propuesta | Posible solucion | Definicion requerida del CTO |
| --- | --- | --- | --- |
| ADR-0009 | Rebalancear Predicciones para priorizar la accion diaria y revisar el peso del scoring diario. | Mantener una sola vista de Predicciones, pero ordenar predicciones diarias por urgencia, estado y kickoff; mover predicciones generales bloqueadas a resumen secundario. | Confirmar si la experiencia diaria debe dominar la vista y si el scoring diario debe ganar mas peso frente a predicciones generales. |
| ADR-0010 | Normalizar ranking grupal y separar el desempeno personal. | Mostrar ranking global con tabla, top 3, filtros, sort y paginado; usar una subvista de desempeno personal con log de puntos; evaluar promedio por participante activo o top N para grupos. | Elegir formula de ranking grupal y confirmar si el desglose personal se implementa como vista/subvista propia. |
| ADR-0011 | Definir recordatorios por Teams y flujo desde Home. | Home funciona como resumen y navegacion; las acciones de prediccion redirigen a Predicciones; recordatorios utiles se envian por Teams si se aprueba integracion Graph/bot. | Decidir si Teams entra en MVP o fase posterior, y si los mensajes van a usuario, grupo o canal. |

Estas tres propuestas no son cambios cosmeticos: afectan jerarquia de UX,
modelo de scoring, consultas de ranking, auditoria de puntos e integracion con
Microsoft Graph. Por eso deben tratarse como decisiones de producto-arquitectura
pendientes, no como detalles de implementacion.

### Riesgos y restricciones

- D1 no es PostgreSQL; migrar desde Supabase puede requerir ajustar SQL, tipos,
  servicios y patrones de consulta.
- NextAuth con Microsoft Entra ID puede requerir ajustes si el runtime de
  Cloudflare impone restricciones.
- El Modelo A puede generar retrasos o errores durante partidos si la operacion
  admin no es disciplinada.
- El Modelo B depende de cobertura real, cuota, latencia y estabilidad del
  proveedor deportivo elegido.
- API-Football Free solo sirve para prototipo; operacion real probablemente
  requiere plan pagado si se hace polling.
- Payloads crudos y syncs necesitan politica de retencion para controlar storage
  y mantener D1 limpio.
- IA no debe ser fuente final para marcadores, resultados, standings, top scorers,
  seleccion con mas goles o mejor defensa.

## Recomendacion para CTO

La recomendacion es aprobar una ruta en dos pasos:

1. **Disenar el schema canonico D1 para soportar ambos modelos.** Las entidades
   principales deben funcionar aunque el dato venga de admin o API: usuarios,
   grupos internos, equipos, partidos, predicciones, reglas, score events,
   snapshots de ranking, overrides y audit log.
2. **Implementar API ingestion como target si el presupuesto lo permite.** Usar
   OpenFootball como seed inicial y probar API-Football Pro para fixtures, live,
   eventos, estadisticas, standings y top scorers. Admin conserva overrides y
   resolucion final.
3. **Cerrar las propuestas de UX, scoring, ranking y Teams con el CTO.** En
   particular: priorizacion de predicciones diarias, formula de ranking grupal,
   vista de desempeno personal y alcance de recordatorios por Teams.

Esta ruta evita bloquear el proyecto por integracion externa, pero deja preparada
la arquitectura para reducir carga manual durante el Mundial.

## Preguntas abiertas para CTO

- Confirmar si la operacion inicial arranca con Modelo A y migra a Modelo B, o
  si se aprueba Modelo B desde el inicio.
- Aprobar o rechazar presupuesto para proveedor deportivo pagado.
- Definir si API-Football sera proveedor piloto o si se requiere evaluar
  football-data.org y Sportmonks antes de decidir.
- Definir nivel esperado de live: resultados finales, live scores, eventos,
  estadisticas o top scorers.
- Definir retencion de payloads crudos y sync runs.
- Confirmar si Predicciones debe priorizar las acciones diarias y relegar
  predicciones generales bloqueadas a resumen.
- Definir formula final de ranking grupal normalizado.
- Confirmar si el log de puntos/desempeno personal sera vista propia o seccion
  secundaria dentro de Ranking.
- Definir si recordatorios por Teams entran en MVP o quedan como fase posterior.
- Definir destinatario de Teams: usuario individual, grupo interno o canal.

## Material de soporte

- [ADR-0008](../adr/0008-prefer-traditional-apis-for-verifiable-football-data.md)
- [ADR-0009](../adr/0009-propose-rebalancing-predictions-ux-and-daily-scoring.md)
- [ADR-0010](../adr/0010-propose-normalized-group-ranking-and-personal-performance-view.md)
- [ADR-0011](../adr/0011-propose-teams-reminders-and-home-reminder-flow.md)
- [ADR-0012](../adr/0012-propose-d1-schema-options-for-manual-and-api-ingestion.md)
- [Opciones de schema D1](../architecture/d1-schema-options.md)
- [Propuesta de APIs tradicionales y fuentes de datos](../integrations/football-data-apis.md)
- [Arquitectura D1 con ingesta manual](../diagrams/d1-manual-ingestion-architecture.md)
- [Arquitectura D1 con API-Football](../diagrams/d1-api-football-ingestion-architecture.md)
