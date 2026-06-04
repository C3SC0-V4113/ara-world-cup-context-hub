# Schema D1 para Ara World Cup

## Estado

Accepted.

Fuente: ADR-0012. El modelo objetivo usa Cloudflare D1 con ingestion
API-Football Free, seed opcional OpenFootball, sync conservador y overrides admin.
El modelo manual/admin queda como fallback operativo, no como fuente primaria.

## Principios D1 aceptados

- Usar IDs `TEXT` para entidades principales.
- Guardar fechas como UTC ISO-8601 en campos `TEXT`.
- Guardar booleanos como `INTEGER` con valores `0` o `1`.
- Guardar payloads JSON como `TEXT` solo cuando aporten auditoria/debug.
- Crear indices para `user_id`, `match_id`, `kickoff_at`, `provider`, `entity`
  y campos usados por ranking/scoring.
- Evitar full scans en ranking y scoring; preferir snapshots, score events e
  indices compuestos.
- Tratar D1 como fuente canonica interna aunque el dato venga de admin o API.
- No incluir tablas de LLM/IA en el modelo objetivo.

## Tablas canonicas internas

| Tabla | Proposito | Campos clave |
| --- | --- | --- |
| `users` | Participantes y administradores | `id`, `provider_id`, `email`, `name`, `avatar_url`, `role`, `internal_group_id`, `created_at` |
| `internal_groups` | Areas/equipos internos | `id`, `name`, `slug`, `is_active` |
| `teams` | Selecciones del Mundial | `id`, `name`, `code`, `flag_url`, `badge_color`, `is_active` |
| `venues` | Sedes/estadios | `id`, `name`, `city`, `country`, `timezone` |
| `matches` | Partidos y resultados | `id`, `home_team_id`, `away_team_id`, `venue_id`, `kickoff_at`, `stage`, `status`, `home_score`, `away_score`, `winner_team_id`, `result_source`, `synced_at` |
| `general_predictions` | Predicciones de torneo por usuario | `id`, `user_id`, `champion_team_id`, `top_scoring_team_id`, `best_defense_team_id`, `locked_at` |
| `general_prediction_finalists` | Finalistas elegidos por usuario | `id`, `general_prediction_id`, `team_id` |
| `general_prediction_qualified_teams` | Clasificados elegidos desde grupos | `id`, `general_prediction_id`, `team_id` |
| `daily_predictions` | Predicciones por partido | `id`, `user_id`, `match_id`, `predicted_home_score`, `predicted_away_score`, `predicted_winner`, `both_teams_score`, `red_card_expected`, `yellow_cards_range`, `shots_on_target_range`, `penalty_winner_team_id`, `locked_at`, `submitted_at` |
| `scoring_rules` | Reglas administrables | `id`, `rule_key`, `points`, `scope`, `is_active`, `effective_from`, `effective_to` |
| `score_events` | Log auditable de puntos | `id`, `user_id`, `match_id`, `prediction_id`, `rule_key`, `points`, `reason`, `calculated_at` |
| `rankings_snapshot` | Snapshots de ranking | `id`, `scope`, `entity_id`, `rank`, `points`, `computed_at`, `metadata_json` |
| `admin_overrides` | Correcciones manuales | `id`, `entity_type`, `entity_id`, `field_name`, `old_value`, `new_value`, `reason`, `created_by`, `created_at` |
| `audit_log` | Auditoria general | `id`, `actor_user_id`, `action`, `entity_type`, `entity_id`, `before_json`, `after_json`, `created_at` |

## Tablas de ingestion

| Tabla | Proposito | Campos clave |
| --- | --- | --- |
| `data_providers` | Catalogo de proveedores | `id`, `name`, `provider_type`, `base_url`, `plan_name`, `daily_request_limit`, `is_active` |
| `provider_entities` | Mapeo ID proveedor a ID interno | `id`, `provider_id`, `entity_type`, `provider_entity_id`, `internal_entity_id`, `last_seen_at` |
| `sync_runs` | Ejecuciones de ingestion | `id`, `provider_id`, `sync_type`, `status`, `started_at`, `finished_at`, `requests_used`, `rows_read`, `rows_written`, `error_message` |
| `raw_provider_payloads` | Payload relevante para auditoria/debug | `id`, `provider_id`, `sync_run_id`, `entity_type`, `provider_entity_id`, `payload_json`, `received_at`, `expires_at` |
| `match_events` | Goles, tarjetas, cambios, penales | `id`, `match_id`, `event_type`, `team_id`, `player_name`, `minute`, `extra_minute`, `provider_event_id`, `source_provider_id`, `synced_at` |
| `match_statistics` | Estadisticas por partido/equipo | `id`, `match_id`, `team_id`, `stat_key`, `stat_value`, `source_provider_id`, `synced_at` |
| `standings` | Posiciones por grupo/fase | `id`, `competition_phase`, `group_name`, `team_id`, `position`, `points`, `played`, `wins`, `draws`, `losses`, `goals_for`, `goals_against`, `synced_at` |
| `top_scorers` | Tabla de goleadores | `id`, `player_name`, `team_id`, `goals`, `assists`, `source_provider_id`, `synced_at` |
| `provider_conflicts` | Diferencias API vs D1/admin | `id`, `entity_type`, `entity_id`, `field_name`, `provider_value`, `current_value`, `status`, `resolved_by`, `resolved_at` |

## Indices minimos

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
CREATE INDEX idx_provider_entities_lookup ON provider_entities(provider_id, entity_type, provider_entity_id);
CREATE INDEX idx_sync_runs_provider ON sync_runs(provider_id, sync_type, started_at);
CREATE INDEX idx_match_events_match ON match_events(match_id, minute);
CREATE INDEX idx_match_statistics_match_team ON match_statistics(match_id, team_id);
CREATE INDEX idx_standings_group ON standings(competition_phase, group_name, position);
CREATE INDEX idx_top_scorers_goals ON top_scorers(goals);
CREATE INDEX idx_provider_conflicts_status ON provider_conflicts(status, entity_type);
```

## Costos y cuota

| Servicio | Costo aproximado | Incluye | Nota |
| --- | --- | --- | --- |
| Cloudflare D1 Free | USD 0 | 5M reads/dia, 100K writes/dia, 5GB total | Bajo riesgo si ranking usa indices y snapshots. |
| Cloudflare D1 Paid | Incluido en Workers Paid; extras por uso | 25B reads/mes, 50M writes/mes, 5GB incluidos | Fallback si uso real supera free. |
| API-Football Free | USD 0/mes | 100 requests/dia | Proveedor aceptado para fase inicial. |
| OpenFootball | USD 0 | Dataset seed | Sin live API ni estadisticas en tiempo real. |
| Microsoft Graph | Depende del licenciamiento Microsoft 365 | Perfil y avatar | No se usara para notificaciones en esta fase. |

## Limites

- API-Football Free puede no cubrir todas las estadisticas requeridas para tiros
  al arco o tarjetas con el detalle esperado.
- El backend debe controlar cuota y no permitir que frontend dispare llamadas al
  proveedor.
- Payloads crudos necesitan retencion; no deben crecer indefinidamente.
- El modelo manual/admin debe existir para overrides y recuperacion operativa.
