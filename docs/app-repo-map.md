# Mapa del Repo de Aplicacion

Repo de referencia: `E:\Repositorios\ara-world-cup`

Este hub no implementa la app. El repo hermano se usa solo para contrastar estado
actual contra la intencion de producto y las recomendaciones tecnicas.

## Documentos existentes en la app

- `README.md`: resumen de stack, arquitectura local y comandos.
- `DESIGN.md`: estandar visual y design system.
- `PRD-predicciones-mundial.md`: requerimiento funcional ya derivado de la
  reunion y del estado del codigo.
- `docs/adr/`: ADRs tecnicos de stack, testing, design system y server actions.

## Vistas principales

- `app/page.tsx` y `components/home/HomeDashboard.tsx`: home con resumen, partidos
  de hoy, favoritos, predicciones, ranking, resultados y banner de Teams.
- `app/predicciones/page.tsx` y `components/predicciones/PredictionsBoard.tsx`:
  predicciones generales, predicciones del dia, reglas de puntos y bloqueo visual.
- `app/ranking/page.tsx` y `components/ranking/RankingBoard.tsx`: ranking
  individual, ranking por area, top 3, movimientos y badges.
- `app/admin/page.tsx` y `components/admin/AdminBoard.tsx`: operacion admin.

## Servicios y reglas

- `lib/services/predictions.ts`: lectura y guardado de predicciones.
- `lib/services/scoring.ts`: servicios de scoring.
- `lib/services/ranking.ts`: ranking individual y por area.
- `lib/services/admin.ts`: settings, reglas y acciones admin.
- `lib/predictionRules.ts`: bloqueo por partido y calculo parcial de puntos
  diarios.
- `lib/scoring.ts`: tabla visible de reglas de puntuacion.

## Identidad y acceso observado

- `auth.ts` usa `next-auth/providers/microsoft-entra-id`.
- El flujo solicita scopes `openid profile email User.Read`.
- La sesion mapea `id`, `name`, `email` e `image` cuando el proveedor los entrega.
- `types/index.ts` ya modela `UserRole = "user" | "admin"`.
- `User` ya contempla `avatarUrl` y `role`.

## Modelo de datos observado

La migracion `supabase/migrations/202605280001_initial_schema.sql` existe como
referencia historica del modelo previo basado en Supabase/PostgreSQL. Por ADR
0006, Supabase ya no es el destino arquitectonico; el schema debe migrarse a
Cloudflare D1.

La migracion previa incluye:

- `users`
- `teams`
- `matches`
- `general_predictions`
- `daily_predictions`
- `daily_extra_predictions`
- `scoring_rules`
- `admin_settings`
- `activity_log`
- preferencias de favoritos y notificaciones

## Brechas principales

- Migrar de Supabase/PostgreSQL a Cloudflare D1.
- Crear o completar configuracion Wrangler y bindings D1.
- Scoring completo de predicciones generales.
- Resolucion administrable de categorias subjetivas.
- Modelo explicito para penales.
- Desempates para partido con mas goles del dia.
- Ranking auditable con desglose de puntos.
- Remplazo de usuario placeholder por autenticacion corporativa real.
- Fuente controlada para asignar administradores.
- Definicion final de areas/equipos internos para ranking grupal.
- Posible capa de ingestion/cache desde APIs externas hacia Cloudflare D1 para
  equipos, calendario, resultados, eventos, standings y estadisticas, pendiente
  de aprobacion por ADR-0008.
- Posible modelo de overrides admin sobre datos importados de proveedores
  externos, pendiente de aprobacion por ADR-0008.
- Rediseno propuesto de Predicciones para priorizar predicciones diarias y
  reducir protagonismo de predicciones generales bloqueadas, pendiente de ADR-0009.
- Posible refactor de scoring para aumentar foco diario, pendiente de ADR-0009.
- Posible ranking por grupos normalizado, tabla avanzada y subvista personal con
  log de puntos, pendiente de ADR-0010.
- Posible flujo de recordatorios por Teams desde Home o botones "recordar",
  pendiente de ADR-0011.
- Decision pendiente sobre badges de selecciones favoritas: estilo unico o color
  principal por pais.
- Posible migracion hacia un schema D1 manual/admin o un schema D1 con ingestion
  API-Football, pendiente de ADR-0012.

## Relacion entre repos

- El repo de aplicacion conserva decisiones tecnicas de implementacion.
- Este hub conserva contexto de producto, decisiones funcionales y memoria
  compartida para agentes.
- Si una decision funcional del hub cambia la arquitectura de la app, debe
  considerarse un ADR tecnico en el repo de aplicacion.
- Para base de datos y despliegue, este hub registra como decision vigente:
  Cloudflare D1, Wrangler y despliegue primario en Cloudflare con Vercel como
  fallback.
- Para identidad y roles, este hub registra como decision vigente: cuentas
  corporativas Microsoft/Windows usadas en Teams y roles base `user`/`admin`.
- Para fuentes de datos externas, este hub registra una propuesta en evaluacion
  en ADR-0008: preferir APIs tradicionales para datos verificables,
  cachear/importar en D1 y limitar IA a categorias ambiguas o narrativa con
  validacion admin. No es una decision vigente.
- Para propuestas UX/arquitectura por vista, este hub registra propuestas en
  ADR-0009, ADR-0010 y ADR-0011. No son decisiones vigentes.
- Para schemas D1, este hub registra opciones propuestas en ADR-0012. No es una
  decision vigente ni una migracion ejecutable.
