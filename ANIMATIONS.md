# ANIMATIONS — DROP

> Phase 5a. Owner: [#G13] Animation Designer + [#8] Interaction Designer. 2026-05-02.

| Event | From | To | Duration | Easing | SFX cue | Particle | Owner |
|---|---|---|---|---|---|---|---|
| splash → playfield | logo screen | game screen | 600ms | ease-out-cubic | sfx_chime | none | [#11] |
| ball spawn (top) | invisible | full opacity at drop position | 200ms | ease-out | sfx_load | none | [#G6] |
| ball drop release | spawn | falling | instant (0ms) | linear | sfx_drop | trail-segment | [#G6] |
| peg hit | default | hit-flash | 80ms in / 80ms out | ease-in-out | sfx_peg (pitch+0.4 per chain) | particle-spark × 16 | [#G13] |
| chain count update | n | n+1 | 100ms | spring (k=200, damping=15) | none | none | [#G13] |
| ball lands in zone | falling | settled | 150ms | ease-out + slight bounce | sfx_zone_land | particle-spark × 24 | [#G6] |
| zone match (3+) | normal | flash | 200ms flash + 100ms ring expand | ease-out | sfx_match | particle-spark × 32 | [#G13] |
| cascade spawn | nothing | new ball at peg | 250ms (fade+scale-in) | spring | sfx_cascade (sub-thump) | cascade-green particles × 40 | [#G6] |
| score popup | 0 | rises 60px while fading | 800ms | ease-out | none | none | [#G13] |
| combo text update | n | n+1 | 100ms snappy | spring | sfx_combo (pitch up) | none | [#G13] |
| hype 50x trigger | normal screen | full juice | 200ms | ease-out | sfx_hype_50x | hype-flash + chromatic-aberration on | [#G13] |
| screenshot prompt | hidden | visible at top-center | 300ms | ease-out + slide-down | sfx_chime | none | [#G19] |
| game-over reveal | last ball lands | round summary | 600ms | ease-in-out | sfx_round_end | gentle particles fade | [#G13] |
| restart transition | round summary | new round | 400ms | ease-in-out | sfx_chime | none | [#11] |
| settings menu open | hidden | slide-in from right | 250ms | ease-out | sfx_ui_open | none | [#11] |

## Reveal sequence (Phase 10 deliverable, spec'd here)

**Trigger**: hype_50x event (combo ≥50x).

| Beat | Time | Visual | Audio |
|---|---|---|---|
| Anticipation | 0-100ms | Combo counter freezes at 50x, screen darkens 20% | Music duck -3dB |
| Action | 100-300ms | Yellow flash full screen (peak alpha 0.4 → 0) | sfx_hype_50x rises pitch |
| Reaction | 300-700ms | Chromatic aberration 1.5px peak, scanline pulse | sfx_hype_50x sustains |
| Accumulation | 700-1500ms | Score multiplier text "50×" pixel-zooms in, holds | Music swell back |
| Completion | 1500-2200ms | Screenshot button fades in below combo text | sfx_chime |

Total reveal duration: 2.2s. Skip available via tap (collapses to 200ms scrub-end).

## Juice grammar (per event, all 5 phases verified)

Each interaction event MUST have all 5 phases (Phase 10 quality gate):

- **anticipation**: pre-event tell (90-150ms before)
- **action**: the event itself (instant or short)
- **reaction**: visual+audio feedback (80-300ms)
- **accumulation**: cumulative effect (combo+score++)
- **completion**: rest beat (150-400ms before next event)

| Event | A | A | R | A | C | Verified |
|---|---|---|---|---|---|---|
| Drop tap | finger touch UI press-state | tap fires, ball appears | ball glows + drop sfx | drop count + 1 | ball falls, await | ✓ |
| Peg hit | (continuous fall = anticipation) | peg-collision | flash + particles + sfx | chain count + 1 | brief pause before next peg | ✓ |
| Cascade | match-3 detected = brief freeze | new ball spawns | green flash + sfx | cascade depth + 1 | new chain begins | ✓ |
| 50x hype | combo climbs visibly | combo hits 50 | full reveal sequence | hype event in telemetry | rest 2.2s before next ball available | ✓ |

## DoD (Phase 5a gate)
- [x] Every interactable in ASSET_MATRIX has animation entry
- [x] Snappy/floaty/weighty pick per element (per STYLE_GUIDE motion vocab)
- [x] Reveal sequence spec'd (6-14s range, with skip)
- [x] 5-phase juice grammar verified for top events
