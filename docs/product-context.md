# Contexto de Producto

## Proposito

Ara World Cup es una app interna para acompanar el Mundial con una dinamica de
predicciones, ranking y gamificacion entre personas y areas de la empresa.

El producto busca que las personas puedan:

- registrar predicciones generales del torneo;
- registrar predicciones del dia por partido;
- seguir su desempeno en ranking individual y por grupos;
- consultar partidos relevantes del dia;
- ver mensajes y elementos de gamificacion basados en datos reales.

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
- Mostrar una vista resumida del ranking con enlace al ranking completo.
- Mostrar accesos hacia predicciones y partidos de hoy.

Decision aceptada:

- Home no guarda predicciones como quick action.
- Toda accion de prediccion redirige a la vista Predicciones.
- Notificaciones y recordatorios quedan fuera de scope y deben mostrarse
  deshabilitados si aparecen en UI.

### Predicciones

Requerimiento de producto:

- Separar `Predicciones del dia` y `Predicciones generales` en tabs.
- Usar `Predicciones del dia` como tab principal.
- Permitir que las predicciones generales se completen una vez y luego queden
  bloqueadas.
- Permitir que las predicciones del dia se editen hasta 30 minutos antes del
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
- Mostrar ranking por grupos normalizado por promedio de participantes activos.
- Mostrar ranking por grupos sin normalizacion por suma bruta.
- Destacar top 3, pero permitir ver tabla completa.
- Agregar filtros y sort en tabla de ranking.
- Mostrar desempeno personal con log de puntos, desglose de aciertos y
  movimientos.

Guia tecnica:

- El ranking basico debe existir antes de graficas o visualizaciones avanzadas.
- La agregacion por grupo requiere definir claramente como se crean y nombran los
  grupos internos.
- Los mensajes de gamificacion deben derivarse de datos confiables.

## Prioridad funcional sugerida

1. Predicciones, tabs y reglas de bloqueo.
2. Scoring aceptado y score events auditables.
3. Ranking individual y ranking por grupos normalizado/sin normalizacion.
4. Home como resumen y redireccion a experiencias dedicadas.
5. Ingestion API-Football Free con D1 y frescura visible.

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
- Datos objetivos de futbol se alimentan con API-Football Free, seed opcional de
  OpenFootball y cache/fuente canonica en D1.
- No se usaran LLMs en esta version.

## Decisiones por vista y scoring

- ADR-0008 acepta APIs tradicionales para datos verificables.
- ADR-0009 acepta tabs en Predicciones y scoring global reforzado con mas
  mecanicas por partido.
- ADR-0010 acepta ranking individual, ranking grupal normalizado por promedio
  activo y ranking grupal sin normalizacion.
- ADR-0011 acepta Home como redireccion y notificaciones fuera de scope.
- ADR-0012 acepta schema D1 orientado a API-Football Free.

## Fuera de alcance actual

- Seleccion sorpresa.
- Seleccion decepcion.
- LLMs para scoring, narrativa o resoluciones.
- Notificaciones, recordatorios y Teams bot.
- Rankings por prediccion semanal, prediccion general, preccion general o
  prediccion del dia.

## Preguntas abiertas principales

- Como se definiran areas o equipos internos para ranking grupal.
- Como se resolvera la lista concreta de administradores: tabla interna, allowlist
  o grupo corporativo.
- Si los badges de selecciones favoritas deben ser estandarizados o usar color
  principal por pais.
- Que retencion tendran payloads crudos y sync runs de proveedores externos.
