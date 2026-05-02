# ARCHITECTURE — DROP

> Phase 6 (pre-build scaffold prereq). Owner: [#2] Architect. 2026-05-02.

## Stack pick

| Layer | Choice | Why |
|---|---|---|
| Runtime | Browser (vanilla JS, ES2022) | Zero install, instant load, $0 hosting |
| Render | HTML Canvas 2D | No framework deps; full control over juice; <50KB total bundle |
| Physics | Hand-rolled 2D rigid-body (circles + AABB pegs) | Matter.js is 100KB+ overkill for ~20 simultaneous balls; custom = 5KB |
| Audio | WebAudio API | All synthesized; no asset weight |
| Persistence | localStorage | Score, PB, cosmetics, settings. No backend. |
| Telemetry | IndexedDB ringbuffer (last 100 events) | Local-only v1; no PII; no network |
| Build | None (vanilla) | Single HTML file with embedded JS; rollup-bundled for production |
| Deploy | Vercel free tier | Static files, $0, instant CDN |
| Mobile wrapper | Capacitor (v1.2 if metrics hit) | Re-uses web build for iOS/Android stores |

## File structure

```
ball-game-v1/
├── www/                          # Built artifact (deployed)
│   ├── index.html               # Single-file game
│   ├── privacy.html
│   └── terms.html
├── src/                          # Source modules (rolled up to www/index.html)
│   ├── main.js                  # Entry point + game loop (fixed timestep)
│   ├── state.js                 # Single state object + freshState() factory
│   ├── physics.js               # Custom 2D circle-AABB collision
│   ├── ball.js                  # Ball spawning, pool
│   ├── peg.js                   # Peg layout generator (seed-based)
│   ├── zone.js                  # Zone matching + cascade detection
│   ├── chain.js                 # Chain detection algorithm
│   ├── render.js                # Canvas rendering pipeline
│   ├── postfx.js                # Bloom + scanlines + chromatic aberration
│   ├── audio.js                 # WebAudio synth + SFX bank
│   ├── input.js                 # Touch + click + keyboard
│   ├── ui.js                    # HUD, buttons, popups
│   ├── particles.js             # Particle pool
│   ├── telemetry.js             # Event ringbuffer + IndexedDB
│   ├── persist.js               # localStorage wrapper (versioned)
│   └── crash.js                 # window.onerror → ringbuffer
├── automation/
│   └── scripts/
│       ├── real-game-test.mjs   # Real-input harness (no .click() shortcuts)
│       ├── role-driven-qa.mjs   # 7-role harness
│       └── visual-ux-testing.mjs
├── tests/                        # Unit tests (vitest or node test runner)
├── package.json                  # build + test scripts
├── .gitignore
├── .husky/pre-push              # Runs harness before push
├── README.md
├── CLAUDE.md                    # Project constitution (this stack + DoD)
├── LICENSE                      # MIT
├── CHANGELOG.md
├── STRATEGY.md ✓
├── RoleActivation.json ✓
├── CONCEPT.md ✓
├── TELEMETRY_SCHEMA.md ✓
├── ACCEPTANCE_CRITERIA.md ✓
├── BENCHMARK.md ✓
├── GDD.md ✓
├── STYLE_GUIDE.md ✓
├── ASSET_MATRIX.md ✓
├── ANIMATIONS.md ✓
├── AUDIO_LIST.md ✓
├── ARCHITECTURE.md (this file) ✓
└── ADRs/
    ├── 0001-vanilla-js-no-framework.md
    ├── 0002-canvas-2d-not-webgl.md
    ├── 0003-custom-physics-not-matter-js.md
    └── 0004-vercel-static-deploy.md
```

## Game loop architecture

```
main.js:
  init() → load persist + audio-unlock-on-first-gesture
  RAF loop:
    1. accumulator += dt
    2. while accumulator >= 16.67ms:
         tick(16.67ms)            // fixed-timestep physics
         accumulator -= 16.67
    3. render(alpha = accumulator/16.67) // interpolated render
    4. requestAnimationFrame(loop)

tick(dt):
  for each ball:
    update position + velocity
    check peg collisions → emit peg_hit events
    check zone landing → emit zone_land events
  update particles
  detect chain ends → emit chain_complete
  detect cascades → spawn bonus balls
  detect hype thresholds → emit hype_* events

render(alpha):
  clear canvas
  draw zones (bg layer)
  draw pegs (with hit-flash if recently hit)
  draw balls (with bloom for neon)
  draw particles
  apply post-fx (scanlines, chromatic aberration on hype)
  draw HUD (score, combo, ball count)
```

## Data flow

```
input (tap) → input.js → state.balls.push(newBall) → tick consumes → render reflects
              ↓
              telemetry.emit('ball_drop')
              ↓
              IndexedDB ringbuffer (last 100)

physics tick → emit events → audio.js plays SFX, ui.js shows popups, telemetry.emit
                            → state.score += delta → persist.save() (debounced 500ms)
```

## Save versioning

```js
// persist.js
const SAVE_VERSION = 1;
function save(state) {
  const blob = { v: SAVE_VERSION, ts: Date.now(), data: state };
  localStorage.setItem('drop:save', JSON.stringify(blob));
}
function load() {
  const raw = localStorage.getItem('drop:save');
  if (!raw) return null;
  const blob = JSON.parse(raw);
  if (blob.v !== SAVE_VERSION) return migrate(blob);
  return blob.data;
}
```

## Crash reporter

```js
// crash.js
window.onerror = (msg, src, line, col, err) => {
  telemetry.emit('js_error', {
    msg: String(msg).slice(0, 120),
    stack: (err?.stack || '').split('\n').slice(0, 5).join('\n')
  });
  return false; // don't suppress
};
window.addEventListener('unhandledrejection', (e) => {
  telemetry.emit('js_error', { msg: 'unhandled_rejection: ' + String(e.reason).slice(0, 120) });
});
```

## ADRs (placeholder, full bodies in ADRs/ folder)

- **ADR-0001**: Vanilla JS, no framework. Reason: bundle size critical (≤200KB gzip), instant load priority. Trade-off: more boilerplate, no reactive UI.
- **ADR-0002**: Canvas 2D, not WebGL. Reason: 60fps achievable for ~20 balls + particles; WebGL adds shader complexity without visual gain at this scale.
- **ADR-0003**: Custom 2D physics, not Matter.js. Reason: 100KB+ overkill; we need only circle-AABB collision; custom = 5KB; deterministic seedable for replays.
- **ADR-0004**: Vercel static deploy. Reason: $0, instant CDN, zero ops; no server-side need for v1.

## Stop-criteria reachability

From STRATEGY §9: D7 retention <8% by week 4 → archive. Reachability:
- D7 retention measurable via `app_open` event sid persistence (TELEMETRY_SCHEMA event).
- Local IndexedDB ringbuffer flushes weekly (manual export endpoint or v1.1 anonymous beacon).
- Dashboard built in Phase 16 from telemetry export. ✓ instrumentation reachable.

## Privacy + License gates (per Phase -0.5 read)

- ✓ NO PII in any event (TELEMETRY_SCHEMA enforces)
- ✓ NO AI gen for v1 (avoids FLUX-dev license issue)
- ✓ All deps MIT-compatible (vanilla JS, no transitive risk)
- ✓ "Press Start 2P" font OFL 1.1 — commercial OK
- ✓ Audio synthesized at runtime — no sample license

## DoD (Phase 6 prereq gate)
- [x] Stack pick + rationale per layer
- [x] File structure
- [x] Game-loop architecture (fixed-timestep + interpolated render)
- [x] Data flow + telemetry path
- [x] Save versioning envelope
- [x] Crash reporter wired
- [x] 4 ADRs stubbed
- [x] Stop-criteria instrumentation reachable
- [x] Privacy + License gates verified
