# Contexto de Producto

## Proposito

Ara World Cup es una app interna para acompanar el Mundial con una dinamica de
predicciones, ranking y gamificacion entre personas y areas de la empresa.

El producto busca que las personas puedan:

- registrar predicciones generales del torneo;
- registrar predicciones diarias por partido;
- seguir su desempeno en ranking individual y por area;
- consultar partidos relevantes del dia;
- ver mensajes y elementos de gamificacion que mantengan vivo el juego.

## Jerarquia de fuentes

Este hub interpreta la reunion con una jerarquia explicita:

- **Alejandra Alvarado** es fuente principal de intencion de producto, UX deseada
  y necesidades no tecnicas.
- **Walter Zetino** es fuente principal de criterio tecnico, arquitectura, riesgos,
  priorizacion y forma de implementacion.

Cuando Alejandra expresa una idea de manera vaga, se documenta como requerimiento
de producto por precisar. Cuando Walter propone un ajuste tecnico, se documenta
como recomendacion tecnica de alto peso.

## Vistas clave

### Home

Requerimiento de producto:

- Mostrar un resumen del dia: partidos, predicciones pendientes y cierre cercano.
- Resaltar si juega una seleccion favorita del usuario.
- Mostrar una vista resumida del ranking general con enlace al ranking completo.
- Mostrar predicciones generales del usuario y partidos de hoy.
- Incluir recordatorios o mensajes relacionados con partidos relevantes, si eso no
  genera confusion operativa.

Guia tecnica:

- Home debe ser complemento, no el centro de la implementacion inicial.
- El foco inicial recomendado es gamificacion, predicciones y ranking.
- Calendario y recordatorios deben quedar como capacidades secundarias porque el
  calendario del Mundial esta disponible por otras vias.

### Predicciones

Requerimiento de producto:

- Separar predicciones generales del torneo y predicciones diarias por partido.
- Permitir que las predicciones generales se completen una vez y luego queden
  bloqueadas.
- Permitir que las predicciones diarias se editen hasta 30 minutos antes del
  inicio de cada partido.
- Explicar dentro de la vista como se ganan puntos.

Guia tecnica:

- El bloqueo diario debe aplicarse por partido, no por jornada completa.
- Una vista diaria puede mostrar varios partidos, pero cada partido debe tener su
  propio estado editable o bloqueado.
- Los partidos cerrados deben mostrarse visualmente como no editables, por ejemplo
  en gris o con estado bloqueado.

### Ranking

Requerimiento de producto:

- Mostrar ranking individual.
- Mostrar ranking por area o equipo interno.
- Destacar un top 3, pero permitir ver el ranking completo.
- Agregar narrativa de gamificacion: rachas, cambios de posicion, aciertos y
  mensajes dinamicos.

Guia tecnica:

- El ranking basico debe existir antes de graficas o visualizaciones avanzadas.
- La agregacion por area requiere definir claramente como se crean y nombran los
  grupos internos.
- Los mensajes de gamificacion deben derivarse de datos confiables, no de frases
  hardcodeadas sin relacion con eventos reales.

## Prioridad funcional sugerida

1. Predicciones y reglas de bloqueo.
2. Scoring y ranking basico.
3. Ranking por area y narrativa de gamificacion.
4. Home como resumen de la experiencia.
5. Calendario, recordatorios y bot de Teams como extensiones.

## Decisiones arquitectonicas cerradas

- La base de datos objetivo es Cloudflare D1.
- Wrangler debe usarse como herramienta de plataforma del proyecto.
- El despliegue primario de la app web debe ser Cloudflare.
- Vercel queda como fallback si Cloudflare no permite avanzar de forma viable.
- Supabase/PostgreSQL queda como referencia historica del repo de aplicacion, no
  como backend preferido.
- El login debe usar cuentas corporativas Microsoft/Windows usadas en Teams.
- La app debe preferir nombre, correo y fotografia desde Microsoft; si la
  fotografia no esta disponible, debe usar un avatar fallback.
- Los roles iniciales son `user` para participantes normales y `admin` para
  operacion administrativa.

## Propuestas tecnicas no aprobadas

- ADR-0008 propone que los datos objetivos de futbol vengan de APIs
  tradicionales, datasets o fuentes estructuradas siempre que sea posible.
- ADR-0008 propone que Cloudflare D1 conserve la copia importada/cacheada para
  UI, scoring y ranking.
- ADR-0008 propone limitar IA a asistencia para categorias ambiguas y narrativa,
  no como fuente final para datos verificables.
- Esta estrategia de APIs externas aun no esta aprobada y no debe tratarse como
  decision cerrada.

## Propuestas por vista no aprobadas

Fuente: criterio tecnico del usuario. Ver
[Propuestas UX y arquitectura por vista](proposals/view-ux-architecture-proposals.md).

- Predicciones: ADR-0009 propone priorizar predicciones diarias, reducir el peso
  visual de predicciones generales bloqueadas y revisar scoring para aumentar el
  foco diario.
- Ranking: ADR-0010 propone evaluar normalizacion por tamano de grupo, separar
  ranking global de desempeno personal y agregar tabla avanzada con paginado,
  sort y filtros.
- Home: ADR-0011 propone que las predicciones del dia redirijan a la vista
  Predicciones y que "recordar" pueda conectarse con Teams si se aprueba la
  integracion.
- Datos/D1: ADR-0012 propone comparar dos schemas para Cloudflare D1: ingesta
  manual/admin con IA asistiva e ingestion externa con API-Football como
  proveedor favorito propuesto.
- Estas propuestas no estan aprobadas y no deben tratarse como decision cerrada.

## Preguntas abiertas principales

- Cual es la tabla final de puntuacion para todas las mecanicas.
- Como se resolveran categorias subjetivas como seleccion sorpresa y seleccion
  decepcion.
- Que proveedor externo se elegira para calendario, resultados en vivo, eventos y
  estadisticas durante el Mundial, si se aprueba la estrategia propuesta.
- Como se definiran areas o equipos internos para ranking grupal.
- Como se resolvera la lista concreta de administradores: tabla interna, allowlist
  o grupo corporativo.
- Que formula de normalizacion seria justa y facil de explicar para ranking por
  grupos, si se aprueba ADR-0010.
- Si los recordatorios por Teams deben ir a usuario, grupo o canal, si se aprueba
  ADR-0011.
- Si los badges de selecciones favoritas deben ser estandarizados o usar color
  principal por pais.
- Si el proyecto debe iniciar con schema manual/admin, schema API-Football o un
  camino incremental entre ambos.
