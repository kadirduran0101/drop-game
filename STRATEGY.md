---
type: game
monetization: F2P-cosmetic
scale: solo
generated: 2026-05-02
strategist_version: v1.0
---

# STRATEGY — DROP

> Generated: 2026-05-02 · Type: game · Strategist v1.0

## 1. Why-download thesis (1 sentence)

A 60-second commute snack for casual mobile players who hate slow tutorials and want one-tap satisfaction with a chain-reaction "oh damn" moment that demands a screenshot.

## 2. Target audience (psychographic, not demographic)

- **Who**: People who play 5+ short mobile sessions per week, 2-7 minutes each, on commutes, ad-breaks, or waiting-room moments. They mute tutorials, swipe past splash screens, and abandon any game that asks for an account.
- **Pain they're scratching**: Casual arcade games either gate progress behind ads/timers or take >2 min to load a single round. They want immediate physics-driven satisfaction without commitment.
- **Their existing alternatives**: (a) tap-to-shoot bubble/peg arcade games, (b) physics drop/dropper one-tap genre, (c) idle progression games with brief active sessions.
- **Why our thing wins for THEM specifically**: Zero account, zero tutorial overlay (5-second test passes via visual demo), single core action (one tap = one drop), and a chain-reaction visual moment that's never the same twice — gives the "one more drop" loop without progress-gating.

## 3. Hype angle — the screenshottable / shareable moment

- **Visual**: A 3-second clip of one ball triggering a chain reaction across 8+ pegs that match-cascade through 3 colored zones, screen flashing CRT scanline + neon glow as combo counter rapidly climbs to 50x.
- **Verbal**: "yeni oyun denedim, şu kombo vurmaca tam manyak" / "you HAVE to see this combo"
- **Mechanic**: First combo (3-chain) fires within first 30 seconds of first-ever drop — physics teaches the hook before the player consciously understands it.

## 4. Differentiation — vs 3 generic competitor categories

- **Category 1 — "tap-to-drop physics arcade games"**: They're polished but use cartoon mascots and freemium currency walls. **Our edge**: No currency, no mascot — pure physics + retro-modern fusion (8-bit pixel balls + modern soft particle glow + CRT scanline filter). Falsifiable: zero in-app purchase prompts in first 30 sessions.
- **Category 2 — "casual peg/bubble shooter"**: They require aim+release combo (2 actions). **Our edge**: One-tap-to-drop, position is the only input. Falsifiable: time-from-app-open to first drop ≤3 seconds (5-second test).
- **Category 3 — "retro pixel arcade with leaderboards"**: Strong nostalgia but visually stuck in 1985. **Our edge**: Modern juice (soft particles, screen-warp on chain, HDR neon palette) layered on 8-bit base sprites — retro-modern fusion not retro-only. Falsifiable: VLM rubric "premium feel" ≥7/10 + "theme-coherent retro-modern" ≥7/10 on 3 in-game screenshots.

## 5. First-impression budget — the 5-second test

- **Second 0-1**: Splash dissolves into playfield with one demo ball already mid-drop, pegs glowing softly.
- **Second 1-3**: Demo ball lands, triggers a small 3-chain combo, screen pulses, score "+150" pops, then a second ball is loaded at top — player sees the loop.
- **Second 3-5**: First tap is the player's own drop position. Ball drops, hits at least 2 pegs. Visual+audio feedback on every peg contact (CRT-tone bleeps + soft particle bloom). They've now experienced the core loop.

If by second 5 they haven't dropped a ball AND seen at least one peg-bounce reaction, the layout failed. NO tutorial overlay — fix the demo ball if needed.

## 6. Trojan-horse hook — what gets them past minute 1

- **Surface promise**: "one-tap arcade game"
- **Hidden hook**: Variable reward (peg layout shifts each round; chain length unpredictable) + mastery curve (recognizing peg patterns to optimize drop position) + screenshot-share moment (50x+ combo) + "almost had it" near-miss frustration that demands a retry.
- **Reveal moment**: Around minute 4-6, when the player has had at least one 30x+ combo and lost it on a near-miss. The "I can do better" loop activates.

## 7. Velocity check — ship-in-N-weeks reality

- **Target time-to-launch**: 3 weeks (single-mechanic game, simple physics, asset count low — pixel sprites + procedural particles)
- **Solo dev hours/week**: ~30-40 (per recent cyber-pinball cadence)
- **Scope risk score**: LOW. Single mechanic (drop ball), no backend, no multiplayer, no accounts, no IAP wiring. Asset count <30 (pixel balls × 8 colors + peg variants × 4 + zone tiles × 4 + UI chrome). Audio: 6 SFX + 1 music bed (synthwave + 8-bit chip fusion).
- **Combo concern**: only the chain-detection algorithm is novel; physics is standard 2D rigid-body (Matter.js or similar).

## 8. Monetization plan — pick ONE primary model

- [x] **Free + optional cosmetics (no gameplay paywall)** — ball-skin packs, peg-glow themes, scanline filter variants. NO currency. NO ads. NO accounts. Cosmetics unlocked via play OR optional one-time purchase.
- Initial launch: ZERO monetization (portfolio + lead-gen). Cosmetic-store ships in v1.1 only after D7 retention ≥15%.

## 9. Stop criteria — explicit kill conditions

- **Hard kill**: D7 retention < 8% by week 4 post-launch → archive project (not pivot — kill).
- **Pivot trigger**: 2 consecutive weeks of <50 organic installs/week + bottom-quartile session-length (<60s avg) + harsh community feedback ("boring" or "too random") → PIVOT mechanic from chain-cascade to deterministic-skill-shot variant.
- **Definition of "we won"**: 1000+ organic installs OR D7 ≥ 25% by week 6 → ramp launch surface (paid experiments + influencer outreach).

## 10. Launch surface — where the first 100 users come from

- **Channel 1**: r/incremental_games + r/IncrementalGarden + r/AndroidGaming subreddits — soft post pattern: 30-second gameplay clip showing a 50x combo, no install link in body (link in comments, per subreddit rules). Targets users who already enjoy this category.
- **Channel 2**: TikTok #physicsgames + #retrogaming hashtag clusters — 15-30s vertical clips of chain reactions only, link in bio. Plus IndieGamesPlus / itch.io community Discord servers (focused on pixel-art arcade games).
- **Channel 3**: Paid experiment $30-50 cap on Reddit ads, targeting r/AndroidGaming subscribers, creative = the same 50x combo clip. Target ROAS not required at this scale; experiment validates message-channel fit.

## STRATEGIST VERDICT

✅ **GREEN** — All 10 sections filled with falsifiable claims. No predatory monetization. Velocity LOW-risk + LOW-scope (3-week solo single-mechanic). Hype angle clear (chain-reaction screenshot moment). Stop criteria explicit (D7<8% by w4 = kill, not pivot). Zero brand-name borrowing. Cost: $0 (Vercel free + procedural assets in code, no AI gen needed for v1). Phase 0 unlocked.
