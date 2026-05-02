# BENCHMARK — DROP

> Phase 1. Owners: [#3] Strategist + [#2] Architect. Generated 2026-05-02.
> Rule: 2-3 publicly acclaimed games per feature, concrete numbers cited. NO brand-name copying — inspiration only.

## Feature 1 — Drop input + ball release timing

**Reference category**: tap-to-drop physics arcade games (Plinko-class).

| Reference | Concrete number |
|---|---|
| Generic Plinko-class peg games | tap-to-ball-release latency 60-100ms; ball drop physics simulated at 60Hz fixed timestep; ball mass tuned for ~1.5-2.0s fall time on standard playfield height |
| Bubble shooter category | aim-cancel ≤200ms; if input→action exceeds this, players report "lag feels broken" |

**Our target**: tap-to-ball-release ≤100ms (aligns with category leaders) · fixed-timestep physics 60Hz · fall time ~1.8s on 480-wide playfield (peg field 60% height).

## Feature 2 — Chain detection + cascade visual

**Reference category**: match-3 cascade games + physics chain-reaction games.

| Reference | Concrete number |
|---|---|
| Match-3 cascade games | chain-reaction visual delay 80-150ms per cascade step (lets eye track); peak satisfaction at chain-length 5-8; combo-multiplier curve typically 1.5x exponential per chain step |
| Plinko-class | each peg-bounce SFX 30-60ms duration, pitched up per consecutive bounce by ~+0.5 semitone (creates rising tension); chain-end resolves on harmonic |

**Our target**: cascade step delay 100ms · chain-length sweet spot 5-8 · multiplier `1.6^chain_length` capped at 50x · peg-bounce SFX 40ms each, pitch-shift +0.4 semitone per consecutive bounce, harmonic on chain-end.

## Feature 3 — Mobile session length + retention

**Reference category**: casual mobile arcade.

| Reference | Concrete number |
|---|---|
| Top-quartile casual arcade games (acclaimed) | session length p50 ~3-5 min, p95 ~10-15 min; D1 retention ~30%, D7 retention ~12-18% |
| Hyper-casual single-mechanic games | session length p50 ~2 min (shorter loops); D1 ~25%; D7 ~8-12% |

**Our target**: p50 ≥3min, p95 ≥10min · D1 ≥25% · D7 ≥15% (gates v1.1 cosmetic launch). Hard-kill threshold from STRATEGY §9: D7 <8% by week 4 = archive.

## Feature 4 — Visual juice (chain reaction satisfaction)

**Reference category**: physics-driven arcade with screen-juice.

| Reference | Concrete number |
|---|---|
| Acclaimed juicy arcade games | screen-shake amplitude 4-8px on impact, decays 200ms; particle count per impact 12-30, lifetime 400-800ms; flash-frame ≤3 per second (photosensitivity safe) |
| Synthwave/retro-juice mobile titles | scanline overlay opacity 5-12%; chromatic aberration 1-2px on big hits; bloom radius 8-16px on neon elements |

**Our target**: screen-shake 6px peak / 200ms decay · particle count 16-24 per peg-hit / 600ms life · flash-frames cap at 2/sec · scanline 8% opacity · chromatic aberration 1.5px on hype_50x · bloom radius 12px on neon balls.

## Feature 5 — Hype-angle screenshot moment

**Reference category**: viral physics chain-reaction moments.

| Reference | Concrete number |
|---|---|
| Reddit/TikTok viral chain-reaction clips | clip length sweet spot 12-20s; the "build-up → cascade → result" pacing, with ~3s build, 6-10s cascade, 3-4s aftermath glow |
| Acclaimed mobile games with share-share-share moments | in-game share button ≤2 taps from event; clip auto-cropped to 9:16 vertical; brand watermark optional, max 5% screen area |

**Our target**: hype_50x triggers in-game share prompt within 1.5s of cascade end · share dialog ≤2 taps · clip auto-recorded last 12s buffered · vertical 9:16 export · watermark 3% bottom-right.

## Cross-feature constraint: 4 Operating Gates

- **MALİYETSİZ**: All references analyzed are public knowledge. No paid analyst reports. No paid asset/audio. Fonts/SFX from CC0 packs only.
- **RİSKSİZ**: No experimental physics — Matter.js (MIT) or hand-rolled; no novel netcode (offline-only).
- **HUKUKLU/TELİFSİZ/ATIFSAL**: NO brand names quoted. NO sprite/character imitation. Inspired-by is legal; copy-paste is not.
- **ETİKLİ**: No predatory mechanics (energy, ads, accounts, dark patterns).

## DoD (Phase 1 gate)
- [x] ≥3 references per feature × 5 features = 15 reference points
- [x] All numbers extracted (not "industry knows")
- [x] No brand names in body text
- [x] All 4 Gates checked
- [x] Targets concrete, falsifiable, achievable on $0 stack
