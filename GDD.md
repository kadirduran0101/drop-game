# GDD — DROP

> Phase 2. Owners: [#G1] Game Designer + [#G2] Engagement Designer. Generated 2026-05-02.

## Core loop diagram (30s-2min action loop)

```
[ROUND START] (free 5 balls)
    ↓
[TAP at top to set drop position] ← player input (1-tap)
    ↓
[BALL falls, hits 4-12 pegs en route]
    ↓
[Chain detection: same-color pegs in sequence count up]
    ↓
[BALL lands in colored zone]
    ├─ matched zone color → +base_score × chain_multiplier
    ├─ 3+ same-color in same zone → CASCADE: spawn bonus ball at random peg
    └─ unmatched → +base_score (no multiplier)
    ↓
[Visual feedback: particle burst, screen shake, score popup]
    ↓
[Hype check: chain ≥10 / cascade ≥3 / combo ≥50x → hype event + screenshot prompt]
    ↓
[Repeat from TAP for next ball; 5 balls per round]
    ↓
[ROUND END: round_score, max_chain, max_cascade]
    ↓
[Player choice: NEW ROUND or VIEW STATS]
```

**Loop duration**: 5 balls × ~12s each = ~60s round. Decision-every-30s satisfied (each drop is a decision).

## Meta loop (session-to-session)

- Personal best tracker (round score, max chain, longest cascade) — local only
- v1.1+: Cosmetic unlocks (ball skin every 1000 cumulative-points, peg-glow theme every 5000, scanline filter every 10000) — non-blocking
- D1 hook: "yesterday's PB was X, beat it" toast on app_open if not played in 12+ hours

## Schell top-20 lenses (quick-pass)

1. **Lens of Essential Experience**: physics satisfaction + chain dopamine. ✓
2. **Lens of Surprise**: peg layout shifts each round → emergent chain reactions. ✓
3. **Lens of Fun**: drop, watch, react. Pure tactile. ✓
4. **Lens of Curiosity**: "what happens if I drop here?" ✓
5. **Lens of Endogenous Value**: score is intrinsically meaningful (combo-based, not currency). ✓
6. **Lens of Problem Solving**: optimal drop position is a puzzle. ✓
7. **Lens of Elegance**: 1 mechanic (drop), N emergent outcomes. ✓
8. **Lens of Character**: pegs/balls are visual characters via color personality (red=fire, cyan=ice, yellow=spark, magenta=void). ✓ (light-touch)
9. **Lens of Indirect Control**: player controls drop position; physics controls outcome — felt-agency without total-determinism. ✓
10. **Lens of Atmosphere**: synthwave + 8-bit fusion, neon glow, CRT scanlines. ✓
11. **Lens of Visible Progress**: chain counter live during fall, score popup on land. ✓
12. **Lens of Reward**: variable schedule (chain length unpredictable) + fixed schedule (every cascade = bonus ball). ✓
13. **Lens of Punishment**: chain breaking (ball misses sequence) = visible reset, "near-miss" event for hooked-loop. ✓
14. **Lens of Simplicity/Complexity**: simple input (tap), complex output (multi-peg cascade). ✓
15. **Lens of Imagination**: minimal — game is mechanical, no narrative. (Trade-off: smaller asset count.)
16. **Lens of Control**: 1-tap, no aim, no release timing. Cannot be simpler. ✓
17. **Lens of Goals**: round score → personal best → cosmetic unlock → leaderboard (future). ✓
18. **Lens of Failure**: loss = round end. Not punishing — instant restart, no progress lost. ✓
19. **Lens of Skill vs Chance**: ~60% skill (drop position), ~40% chance (peg interactions). Cycle-tested. ✓
20. **Lens of Toy**: even without scoring, watching balls fall is satisfying. ✓

## MDA cross-check

- **Mechanics**: drop, peg-bounce physics, chain-detection, zone-match, cascade-spawn.
- **Dynamics**: chain-chasing, cascade-anticipation, near-miss frustration, screenshot-flexing.
- **Aesthetics**: sensation (particle juice), challenge (combo optimization), discovery (new layouts), expression (cosmetic skins v1.1+), submission (commute escapism).

## Costikyan uncertainty

- ✓ Performative uncertainty: skill execution timing
- ✓ Solver uncertainty: optimal drop position
- ✓ Player unpredictability: own intuition mistakes
- ✗ Randomness: bounded — peg layout deterministic per round (seed-based)
- ✗ Hidden information: none

## Sid Meier "interesting decision every 30s"

Each ball drop = 1 decision = ~12s apart. EXCEEDS Sid Meier minimum (every 30s). ✓

## Hooked loop (Trigger / Action / Variable Reward / Investment)

- **Trigger** (external): app_open from home screen / push notification ("yesterday's PB was X")
- **Action**: tap to drop ball (single, low-effort)
- **Variable Reward**: chain length unpredictable; cascade depth unpredictable
- **Investment**: personal best (PB) tracking + cosmetic unlock progress + muscle memory of peg layouts

## Octalysis (≥4 drives mapped to features)

- **Driver 4 — Ownership/Possession**: PB tracking + cosmetic ownership (v1.1+)
- **Driver 6 — Scarcity**: limited 5 balls per round; can't replay a layout once gone
- **Driver 7 — Curiosity/Unpredictability**: peg layout changes; cascade chain-of-cascade is emergent
- **Driver 5 — Avoidance/Loss**: near-miss events ("almost had a 50x") drive retry compulsion
- (Optional 8th — Meaning): NOT mapped, no narrative angle

## Variable reward schedule

- **Ratio reinforcement**: chain length 3-10 most common (every drop ~75% gives some chain), 10-30 occasional (~20%), 30-50 rare (~4%), 50+ extremely rare (~1%)
- **Magnitude reinforcement**: combo_multiplier `1.6^chain_length` capped 50x; jackpot = cascade-of-cascade with ≥3 cascades chained

## Long-arc goal

- Local: beat PB
- v1.1+: complete cosmetic gallery (24 unlockables across 3 categories)
- v1.2+ (if D7 ≥25%): leaderboard + daily seed challenge

## Ball-saver / first-ball-spawn rule

**MANDATORY** (per FAILURE_MODE_LEDGER 2026-05-02 entry): First ball of every round must be already mid-drop (auto-launched) for visual demo, NOT awaiting tap.
- Round start → auto-drop demo ball at tap_x_norm=0.5 → player sees first chain → THEN player gets prompt for ball 2.
- Implementation: `state.balls[0].auto_demo = true`; player can't tap during demo (300ms grace).

**Spawn-to-flipper-arc rule (analog)**: Drop position MUST result in ≥2 peg contacts before zone landing. Layout designer ensures no "void columns" where ball falls 0-peg-bounce.

## DoD (Phase 2 gate)
- [x] All 20 lenses applied
- [x] ≥4 Octalysis drives mapped to specific features
- [x] Uncertainty preserved (skill ~60% + emergent physics ~40%)
- [x] Sid Meier 30s rule satisfied
- [x] Ball-saver-on-spawn spec'd
- [x] Spawn-to-action-zone rule spec'd
