# Opciones de Schema D1 para Ara World Cup

## Estado

Proposed / no aprobado.

Fuente: criterio tecnico del usuario. Este documento compara dos opciones de
schema para Cloudflare D1. No define una migracion ejecutable ni una decision
cerrada.

## Principios D1 propuestos

- Usar IDs `TEXT` para entidades principales.
- Guardar fechas como UTC ISO-8601 en campos `TEXT`.
- Guardar booleanos como `INTEGER` con valores `0` o `1`.
- Guardar payloads JSON como `TEXT` cuando se necesite conservar respuesta cruda
  o metadatos variables.
- Crear indices para `user_id`, `match_id`, `kickoff_at`, `provider`, `entity`
  y campos usados por ranking/scoring.
- Evitar full scans en ranking y scoring; preferir snapshots, score events e
  indices compuestos.
- Tratar D1 como fuente canonica interna aunque el dato venga de admin, IA o API.

## Modelo A: ingesta manual/admin

Enfoque: admin carga equipos, sedes, partidos, resultados, reglas y resoluciones.
IA puede asistir categorias subjetivas, pero admin revisa y aprueba antes de
persistir un resultado final.

### Tablas propuestas

| Tabla | Proposito | Campos clave |
| --- | --- | --- |
| `users` | Participantes y administradores | `id`, `provider_id`, `email`, `name`, `avatar_url`, `role`, `internal_group_id`, `created_at` |
| `internal_groups` | Areas/equipos internos | `id`, `name`, `slug`, `is_active` |
| `teams` | Selecciones del Mundial | `id`, `name`, `code`, `flag_url`, `badge_color`, `is_active` |
| `venues` | Sedes/estadios | `id`, `name`, `city`, `country`, `timezone` |
| `matches` | Partidos y resultados | `id`, `home_team_id`, `away_team_id`, `venue_id`, `kickoff_at`, `stage`, `status`, `home_score`, `away_score`, `winner_team_id`, `result_source` |
| `general_predictions` | Predicciones de torneo por usuario | `id`, `user_id`, `champion_team_id`, `finalist_team_id`, `surprise_team_id`, `disappointment_team_id`, `top_scoring_team_id`, `best_defense_team_id`, `locked_at` |
| `daily_predictions` | Predicciones por partido | `id`, `user_id`, `match_id`, `predicted_home_score`, `predicted_away_score`, `predicted_winner`, `both_teams_score`, `locked_at`, `submitted_at` |
| `scoring_rules` | Reglas administrables | `id`, `rule_key`, `points`, `scope`, `is_active`, `effective_from`, `effective_to` |
| `score_events` | Log auditable de puntos | `id`, `user_id`, `match_id`, `prediction_id`, `rule_key`, `points`, `reason`, `calculated_at` |
| `rankings_snapshot` | Snapshots de ranking | `id`, `scope`, `entity_id`, `rank`, `points`, `computed_at`, `metadata_json` |
| `admin_overrides` | Correcciones manuales | `id`, `entity_type`, `entity_id`, `field_name`, `old_value`, `new_value`, `reason`, `created_by`, `created_at` |
| `ai_resolution_requests` | Solicitudes IA para subjetivos | `id`, `category`, `input_json`, `ai_response_json`, `status`, `approved_value`, `approved_by`, `approved_at` |
| `audit_log` | Auditoria general | `id`, `actor_user_id`, `action`, `entity_type`, `entity_id`, `before_json`, `after_json`, `created_at` |

### Indices minimos

```sql
CREATE INDEX idx_users_group ON users(internal_group_id);
CREATE INDEX idx_matches_kickoff ON matches(kickoff_at);
CREATE INDEX idx_matches_status ON matches(status);
CREATE INDEX idx_daily_predictions_user_match ON daily_predictions(user_id, match_id);
CREATE INDEX idx_general_predictions_user ON general_predictions(user_id);
CREATE INDEX idx_score_events_user ON score_events(user_id, calculated_at);
CREATE INDEX idx_score_events_match ON score_events(match_id);
CREATE INDEX idx_rankings_snapshot_scope ON rankings_snapshot(scope, computed_at);
CREATE INDEX idx_admin_overrides_entity ON admin_overrides(entity_type, entity_id);
```

### Ventajas

- Menor dependencia externa.
- Control total desde admin.
- Costos de API deportiva cercanos a cero.
- Buen punto de partida si el alcance operativo es pequeno.

### Limites

- Mayor carga manual durante el Mundial.
- Riesgo de retrasos o errores al cargar resultados.
- IA no debe resolver hechos objetivos; solo asistir categorias ambiguas.
- Scoring/ranking depende de disciplina operativa admin.

## Modelo B: ingestion API-Football

Enfoque: API-Football alimenta fixtures, live scores, eventos, estadisticas y top
scorers. OpenFootball puede usarse como seed gratuito inicial. Microsoft Graph
alimenta identidad, avatar y mensajes Teams. Admin conserva overrides sobre datos
normalizados.

### Tablas canonicas internas

El modelo conserva las tablas canonicas del Modelo A:

- `users`
- `internal_groups`
- `teams`
- `venues`
- `matches`
- `general_predictions`
- `daily_predictions`
- `scoring_rules`
- `score_events`
- `rankings_snapshot`
- `admin_overrides`
- `audit_log`

### Tablas adicionales de ingestion

| Tabla | Proposito | Campos clave |
| --- | --- | --- |
| `data_providers` | Catalogo de proveedores | `id`, `name`, `provider_type`, `base_url`, `plan_name`, `is_active` |
| `provider_entities` | Mapeo ID proveedor a ID interno | `id`, `provider_id`, `entity_type`, `provider_entity_id`, `internal_entity_id`, `last_seen_at` |
| `sync_runs` | Ejecuciones de ingestion | `id`, `provider_id`, `sync_type`, `status`, `started_at`, `finished_at`, `rows_read`, `rows_written`, `error_message` |
| `raw_provider_payloads` | Payload crudo para auditoria/debug | `id`, `provider_id`, `sync_run_id`, `entity_type`, `provider_entity_id`, `payload_json`, `received_at` |
| `match_events` | Goles, tarjetas, cambios, penales | `id`, `match_id`, `event_type`, `team_id`, `player_name`, `minute`, `extra_minute`, `provider_event_id`, `source_provider_id` |
| `match_statistics` | Estadisticas por partido/equipo | `id`, `match_id`, `team_id`, `stat_key`, `stat_value`, `source_provider_id`, `synced_at` |
| `standings` | Posiciones por grupo/fase | `id`, `competition_phase`, `group_name`, `team_id`, `position`, `points`, `played`, `wins`, `draws`, `losses`, `goals_for`, `goals_against`, `synced_at` |
| `top_scorers` | Tabla de goleadores | `id`, `player_name`, `team_id`, `goals`, `assists`, `source_provider_id`, `synced_at` |
| `provider_conflicts` | Diferencias API vs D1/admin | `id`, `entity_type`, `entity_id`, `field_name`, `provider_value`, `current_value`, `status`, `resolved_by`, `resolved_at` |

### Indices adicionales

```sql
CREATE INDEX idx_provider_entities_lookup ON provider_entities(provider_id, entity_type, provider_entity_id);
CREATE INDEX idx_provider_entities_internal ON provider_entities(entity_type, internal_entity_id);
CREATE INDEX idx_sync_runs_provider ON sync_runs(provider_id, sync_type, started_at);
CREATE INDEX idx_raw_payloads_entity ON raw_provider_payloads(provider_id, entity_type, provider_entity_id);
CREATE INDEX idx_match_events_match ON match_events(match_id, minute);
CREATE INDEX idx_match_statistics_match_team ON match_statistics(match_id, team_id);
CREATE INDEX idx_standings_group ON standings(competition_phase, group_name, position);
CREATE INDEX idx_top_scorers_goals ON top_scorers(goals);
CREATE INDEX idx_provider_conflicts_status ON provider_conflicts(status, entity_type);
```

### Ventajas

- Reduce trabajo manual de calendario, resultados, eventos y estadisticas.
- Permite derivar top scorers, equipo con mas goles y mejor defensa desde datos
  objetivos.
- Mantiene trazabilidad de payloads, syncs y conflictos.
- Admin conserva control final con overrides.

### Limites

- Requiere jobs de sync, mapeo de IDs externos y monitoreo de cuotas.
- API-Football sigue siendo propuesta, no proveedor aprobado.
- Free puede servir para prototipo, pero no para operacion completa con polling.
- Payloads crudos aumentan storage y deben tener politica de retencion.

## Costos verificados al 2026-06-03

| Servicio | Costo aproximado | Incluye | Nota |
| --- | --- | --- | --- |
| Cloudflare D1 Free | USD 0 | 5M reads/dia, 100K writes/dia, 5GB total | Para prototipo; limites diarios pueden bloquear queries si se exceden. |
| Cloudflare D1 Paid | Incluido en Workers Paid; extras por uso | 25B reads/mes, 50M writes/mes, 5GB incluidos | Excedentes: USD 0.001/M reads, USD 1.00/M writes, USD 0.75/GB-mes. |
| API-Football Free | USD 0/mes | 100 requests/dia | Solo prototipo o pruebas puntuales. |
| API-Football Pro | USD 19/mes | 7,500 requests/dia | Recomendacion propuesta para iniciar si se aprueba ingestion externa. |
| API-Football Ultra | USD 29/mes | 75,000 requests/dia | Subir si polling o pruebas superan Pro. |
| API-Football Mega | USD 39/mes | 150,000 requests/dia | Opcion de mayor margen operativo. |
| OpenFootball | USD 0 | Dataset seed | Sin live API ni estadisticas en tiempo real. |
| Microsoft Graph | Depende del licenciamiento Microsoft 365 | Perfil, avatar y Teams | No es proveedor deportivo. |
| football-data.org | Desde EUR 12/mes con live scores | Fixtures, schedules, tables, live scores | Comparativo secundario. |
| Sportmonks World Cup API | Desde EUR 69/mes | Fixtures, live, eventos, standings, player stats | Comparativo mas robusto/caro. |
| WorldCupAPI | EUR 499/mes | API especializada World Cup 2026 | No prioritaria por costo. |

## Recomendacion propuesta

Si se aprueba ingestion externa, usar **API-Football Pro** como punto de partida.
El plan Free queda para prototipo. Subir a Ultra solo si el polling real, pruebas
de carga o ventanas de partidos simultaneos superan 7,500 requests/dia.

OpenFootball debe usarse como seed inicial gratuito cuando sea util. Microsoft
Graph debe mantenerse solo para identidad, avatar y Teams.

## Preguntas abiertas

- Si el proyecto operara inicialmente con Modelo A y migrara luego a Modelo B.
- Que retencion tendran `raw_provider_payloads`.
- Que formula final se usara para ranking grupal normalizado.
- Que nivel de polling se permitira durante partidos en vivo.
- Que datos exactos se expondran en el log de puntos del usuario.

