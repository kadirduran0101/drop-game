# ACCEPTANCE_CRITERIA — DROP

> Phase 0.6 LOCKED. Owners: [#3] Strategist + [#19] QA Functional Lead + [#30] Data Analyst + [#43] Delivery Manager (joint).
> Lock date: 2026-05-02.
> Rule: every criterion has WHO + WHAT (measurable threshold) + HOW (measurement) + WHEN (which phase gate) + TOOL.
> No section ships YELLOW. Loop until GREEN.

## A. Functional (Layer A) — owned by [#19]

| AC | WHAT | HOW | WHEN | TOOL |
|---|---|---|---|---|
| A1 | Zero console errors during 60s play | grep `Error:` in playwright console log | Phase 12 | `automation/scripts/real-game-test.mjs` |
| A2 | Ball physics: no NaN positions, no OOB | invariant check every frame in headless run | Phase 8+12 | physics tick assertions |
| A3 | Drop input → ball falls within 100ms | tap-to-firstball-y-change measurement | Phase 12 | playwright `touchscreen.tap` + state probe |
| A4 | Round-end → restart-cleanup atomic | state inspection after game_over → start | Phase 12 | persona harness `restart-spammer` |
| A5 | All 4 zone colors trigger zone_land event | each color drop test, event capture | Phase 12 | telemetry replay |
| A6 | Cascade chain doesn't softlock (depth ≤10 cap) | force-cascade test, watch for infinite loop | Phase 8+12 | physics-stress harness |
| A7 | First drop → chain within 30s for ≥80% players | persona harness `fresh-spawn-survival` | Phase 12 | multi-persona harness |
| A8 | localStorage save survives kill+reload | inject ball, kill page, reload, check state | Phase 12 | playwright reload assert |

## B. Performance (Layer A subset) — owned by [#15] (dormant for solo, fold into [#19])

| AC | WHAT | HOW | WHEN | TOOL |
|---|---|---|---|---|
| B1 | p99 frame ≤16.6ms on mid-tier mobile (Pixel 5 chrome) | playwright performance.measure during 60s play | Phase 12+15 | chrome devtools perf trace |
| B2 | No allocation in hot path (pool all balls/particles) | heap snapshot before/after 30s play, delta <2MB | Phase 8+12 | chrome heap diff |
| B3 | Bundle size ≤200KB initial JS (gzip) | next build size or rollup analysis | Phase 6+15 | bundle-analyzer |
| B4 | First Contentful Paint ≤1.5s on 4G throttled | lighthouse mobile run | Phase 15 | lighthouse |
| B5 | iOS Safari 60fps for 60s without throttle drop | RAF timestamp delta check | Phase 12 | webkit playwright |

## C. Subjective UX (Layer B) — owned by [#20]

| AC | WHAT | HOW | WHEN | TOOL |
|---|---|---|---|---|
| C1 | "Premium feel" VLM rubric ≥7/10 on 3 screenshots (start, mid-play, post-50x) | VLM call (gpt-4o-mini class) on captured screenshots | Phase 11+12 | VLM-rubric harness |
| C2 | "Theme-coherent retro-modern fusion" ≥7/10 | same VLM call, separate axis | Phase 11+12 | VLM-rubric harness |
| C3 | "Would-screenshot moment" ≥7/10 (does the 50x combo image read as share-worthy?) | VLM rubric + actual `screenshot_taken` event count in persona run | Phase 12 | VLM + telemetry |
| C4 | "Hook-activation" ≥7/10 — first chain ≤30s for skilled persona, ≤60s for casual | telemetry `time_to_first_chain_ms` p50 | Phase 12 | persona harness |
| C5 | "Hype-angle delivery" — at least one persona hits hype_50x within 5 minutes | telemetry filter for hype_50x event | Phase 12 | persona harness |
| C6 | No "feels cheap/placeholder/annoying" issues from VLM open-ended | VLM open-ended issue list | Phase 11+12 | VLM rubric (ignore-list applied) |

## D. Accessibility — owned by [#21] (paired into [#19] for solo)

| AC | WHAT | HOW | WHEN | TOOL |
|---|---|---|---|---|
| D1 | All hit targets ≥44×44pt (drop zone, restart button, share button) | DOM measurement | Phase 11 | playwright bounding-box probe |
| D2 | ΔE76 ≤20 between any rendered color and STYLE_GUIDE palette | screenshot color sampling | Phase 11 | image-color-diff |
| D3 | Photosensitivity: ≤3 flashes/sec across all juice | flash detector on captured 60s clip | Phase 11+12 | flash-rate analyzer |
| D4 | Color is not the only signal (zone landing also has shape/text) | visual inspection of zone graphics | Phase 3+11 | manual + VLM |
| D5 | Keyboard tab order valid (drop position via arrow keys + space-to-drop fallback) | keyboard-nav harness | Phase 11 | playwright keyboard |

## E. Engagement / KPI (Layer B subset) — owned by [#30]

| AC | WHAT | HOW | WHEN | TOOL |
|---|---|---|---|---|
| E1 | Hype-fire rate ≥30% rounds (chain ≥10 triggers) | telemetry `hype_chain` / `round_start` | Post-launch wk1 | KPI dashboard |
| E2 | Screenshot moment hit-rate ≥5% of hype_50x events | telemetry `screenshot_taken` / `hype_50x` | Post-launch wk2 | KPI dashboard |
| E3 | Drain-to-restart ratio ≥40% (want-back signal) | `restart_after_loss` / `game_over` | Post-launch wk1 | KPI dashboard |
| E4 | Session length p50 ≥3 min, p95 ≥10 min | `app_close.session_duration_ms` distribution | Post-launch wk2 | KPI dashboard |
| E5 | D7 retention ≥15% (gate for v1.1 cosmetic launch) | sid revisit count | Post-launch wk2 | KPI dashboard |

## F. Privacy / Legal — owned by [#33] + [#34] (dormant for solo, fold into [#3])

| AC | WHAT | HOW | WHEN | TOOL |
|---|---|---|---|---|
| F1 | privacy.html present, GDPR/KVKK compliant | manual review + linter | Phase 14 | textlint legal-pack |
| F2 | terms.html present, plain-language ≤9th grade | Hemingway readability score | Phase 14 | hemingway score |
| F3 | NO PII in telemetry payload | event payload audit | Phase 12+15 | telemetry replay scrubber |
| F4 | LICENSE_TRAIL.md complete (Matter.js or custom physics, sprites authored, no AI gen) | dep audit + asset audit | Phase 13 | npm audit + manual asset list |
| F5 | All 4 Operating Constitution Gates GREEN: MALİYETSİZ + RİSKSİZ + HUKUKLU + ETİKLİ | self-attest checklist | Phase 14 | manual gate review |

## G. Ship-gate composite — owned by [#25]

| AC | WHAT | HOW | WHEN | TOOL |
|---|---|---|---|---|
| G1 | All A1-A8 GREEN (functional) | check Layer A verdict | Phase 14 | `/ship-gate` |
| G2 | All B1-B5 GREEN (perf) | check perf report | Phase 14 | `/ship-gate` |
| G3 | All C1-C6 ≥7/10 (Layer B) | check engagement.json | Phase 14 | `/ship-gate` |
| G4 | All D1-D5 GREEN (a11y) | check wcag-violations.json | Phase 14 | `/ship-gate` |
| G5 | All F1-F5 GREEN (privacy/legal) | check 4-gates report | Phase 14 | `/ship-gate` |
| G6 | E1-E5 instrumented (post-launch validates) | telemetry events firing in dev | Phase 12 | telemetry replay |

**Ship-block rule**: ANY single AC RED or YELLOW = no ship. Convert YELLOW to GREEN by fixing OR to RED by escalating real issue (Quality-Bar Lead [#50] rule).

## Loop policy
If post-Phase-12 ANY Layer B (C1-C6) score <7 → Phase 12.5 Strategist Mode B fires. Max 3 iterations before mandatory user check-in. After 3 failed loops → RED verdict, surface to user.

## DoD (Phase 0.6 gate)
- [x] Every criterion measurable (no "looks good")
- [x] Every criterion has WHO/WHAT/HOW/WHEN/TOOL
- [x] No YELLOW path defined (only GREEN or RED)
- [x] Joint sign-off recorded (above)
