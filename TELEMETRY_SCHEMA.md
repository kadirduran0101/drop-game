# TELEMETRY_SCHEMA — DROP

> Phase 0.5 LOCKED. Owners: [#29] Analytics Lead + [#3] Strategist + [#G6] Gameplay Engineer.
> Schema-version: v1.0. Lock date: 2026-05-02.
> Changes require: joint sign-off + version bump + migration plan for live events.

## Common envelope (every event)
```json
{
  "ev": "<event_name>",        // schema-defined
  "ts": <ms_epoch>,            // client-side milliseconds
  "sid": "<uuid_per_session>", // generated on app open, cleared on kill
  "build": "<git_sha>",        // build version
  "device": "ios-safari|android-chrome|desktop|...",
  "locale": "tr-TR|en-US|...",
  "schema_v": "1.0"
}
```

## PII rules
- **NO** name, email, IP, device-id, ad-id, location.
- **YES** anonymous session ID (uuid v4, ephemeral per session).
- **YES** build version, device class, locale (no specific model).

## Event taxonomy

### Lifecycle events
- `app_open` — first paint of game shell. No extras.
- `app_close` — sendBeacon or local ringbuffer flush. Extras: `session_duration_ms`.
- `app_resume` — from background. Extras: `gap_ms` (time since last interaction).
- `app_suspend` — to background. Extras: `session_partial_ms`.

### Game-loop events
- `round_start` — Extras: `round_index` (1-based count this session), `peg_layout_seed` (deterministic ID).
- `ball_drop` — Player tapped to drop. Extras: `tap_x_norm` (0-1 horizontal position), `round_ball_index` (n-th ball this round).
- `peg_hit` — Single ball-peg contact. Extras: `peg_color` (red|cyan|yellow|magenta), `chain_length_so_far` (current chain count).
- `chain_complete` — Chain ended (ball stopped or zone-landed). Extras: `chain_length_final`, `score_delta`, `combo_multiplier`.
- `zone_land` — Ball landed in colored zone. Extras: `zone_color`, `score_delta`, `match_count` (1-3 if 3+ same-color in same zone).
- `cascade_trigger` — Match-3 hit zone caused new ball spawn. Extras: `cascade_depth` (1=initial, 2+=cascade-of-cascade).
- `round_end` — Round-state finalized. Extras: `round_score`, `round_duration_ms`, `max_chain`, `max_cascade_depth`, `balls_used`.
- `game_over` — All balls used + animations complete. Extras: `final_score`, `personal_best` (boolean).

### Hype-fire events (KPI-critical)
- `hype_chain` — Chain ≥10. Extras: `chain_length`, `was_screenshot_eligible` (boolean — UI prompts share).
- `hype_cascade` — Cascade depth ≥3. Extras: `cascade_depth`.
- `hype_50x` — Combo multiplier ≥50x. Extras: `combo_multiplier`. (Counts toward STRATEGY §3 hype-angle hit-rate.)
- `screenshot_prompt_shown` — UI offered share. Extras: `event_trigger` (which hype event fired it).
- `screenshot_taken` — User used in-game share button. (Counts toward "would-screenshot" Layer B axis.)

### UX events
- `tutorial_implicit_complete` — First successful drop+chain happened. Extras: `time_to_first_chain_ms`, `drops_to_first_chain`.
- `near_miss` — Chain broke at 9 (just below hype_chain threshold). (Hooked-loop frustration signal.)
- `restart_after_loss` — Player restarted within 5s of game_over. (Want-back signal.)
- `share_completed` — Share dialog returned success. Extras: `channel` (twitter|tiktok|generic).

### Cosmetic events (v1.1+)
- `cosmetic_unlock_view` — User opened skin/theme menu.
- `cosmetic_select` — Equipped a cosmetic. Extras: `cosmetic_id`, `unlock_method` (free|purchase).
- `cosmetic_purchase` — One-time purchase completed. Extras: `cosmetic_id`, `price_local`, `currency`.

### Error events
- `js_error` — Caught by window.onerror. Extras: `error_message_first_120_chars`, `stack_top_5_frames`. PII-strip.
- `physics_invariant_violation` — NaN ball position, OOB peg, etc. Extras: `invariant_name`.
- `perf_long_task` — Frame >30ms. Extras: `frame_duration_ms`, `task_type` (physics|render|audio).

## KPI mapping (each event used by ≥1 KPI)

| KPI | Source events |
|---|---|
| D1/D7/D28 retention | `app_open` per sid + revisit detection (sid persistence) |
| Session length p50/p95 | `app_close.session_duration_ms` |
| Hype-fire rate | `hype_chain` + `hype_cascade` + `hype_50x` count / `round_start` count |
| Screenshot moment hit-rate | `screenshot_taken` / `hype_50x` |
| Time-to-first-hype (5-sec test extension) | `tutorial_implicit_complete.time_to_first_chain_ms` |
| Drain-to-restart ratio | `restart_after_loss` / `game_over` |
| Funnel drop-off | per-state count delta |
| Crash rate | `js_error` per session, `physics_invariant_violation` |
| Cosmetic conv (v1.1+) | `cosmetic_purchase` / `cosmetic_unlock_view` |

## Implementation notes
- Local ringbuffer (last 100 events) flushed on app_close OR every 30s during session.
- v1: local-only telemetry stored in IndexedDB; weekly export endpoint optional (in-skill viewer).
- v1.1: optional anonymous beacon to self-hosted Plausible-class endpoint (Vercel function), still no PII.
- Any new `tel("event")` call in code MUST be added to this schema FIRST. Code-side audit by [#29] before merge.

## DoD (Phase 0.5 gate)
- [x] Common envelope spec
- [x] PII rules explicit
- [x] All events ≥1 KPI consumer (no orphans)
- [x] Versioning field present
- [x] Schema locked — changes require version bump
