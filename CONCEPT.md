# CONCEPT — DROP

> Phase 0 output, derived from STRATEGY.md (no user prompt). Auto-generated 2026-05-02.

## 1-line niche thesis
60-second commute snack: tap-to-drop ball physics with chain-cascade combos, modern-juice + 8-bit-CRT visual fusion, zero-tutorial onboarding.

## Genre + sub-genre
- **Primary genre**: Casual physics arcade
- **Sub-genre**: Tap-to-drop / peg-cascade (Plinko-class)
- **Session length target**: 60-90s per round, 5-15 min per session

## Theme (cultural + visual angle)
- **Cultural**: Synthwave + arcade nostalgia ("80s-future") meets present-day mobile minimalism. Aesthetic that says "I grew up on Game Boy, now I drink cold brew."
- **Visual angle**: 8-bit pixel base sprites (balls, pegs, zone tiles) + modern soft particle bloom + CRT scanline overlay (subtle, ≤8% opacity) + neon HDR palette. Dark background, glowing physics objects, screen-warp on chain reactions.
- **Anti-pattern**: NOT pixel-only retro. NOT modern-flat-only. Fusion is the differentiator — both layers visible per frame.

## Target audience (1 line)
Casual mobile arcade players, 5+ short sessions/week, commute/break demographic, anti-tutorial, anti-IAP-wall, screenshot-share active on Reddit + TikTok.

## USP (what's different)
**One-tap-to-drop with chain-cascade scoring + retro-modern visual fusion.** No aim+release (just position-tap), no currency, no accounts, no ads. The chain-reaction screenshot moment is the marketing engine — not the ad budget.

## Monetization plan
- **Launch**: ZERO monetization. Free to play, no ads, no IAP, no accounts.
- **v1.1+ (post D7≥15%)**: Optional cosmetic packs (ball-skins, peg-glow themes, scanline filter variants). One-time purchase, no currency abstraction.
- **Never**: Energy gates, mid-task ads, real-money loot boxes, account walls, dark-pattern auto-renew.

## Stop criteria (mirrors STRATEGY §9)
- **Hard kill**: D7 retention <8% by week 4 → archive
- **Pivot trigger**: 2 consecutive weeks <50 organic installs/week + harsh feedback → pivot mechanic to deterministic-skill-shot variant
- **"We won"**: 1000+ organic installs OR D7 ≥25% by week 6 → ramp paid acquisition

## Stack constraint (from user infra)
- Web/PWA only for v1 (Capacitor APK in v1.2 if D7 hits target)
- Vercel free tier deploy
- Vanilla JS + HTML Canvas (no framework — bundle size critical for instant-load)
- Matter.js OR custom 2D physics (decided in Phase 1 ARCHITECTURE based on perf budget)
- $0 cost: no AI assets needed, all visuals procedural code-drawn or pixel sprites authored in pixel editor (PixilArt free)
- Zero backend in v1 — all state local via localStorage

## Avoid (from user memory + ledger)
- ❌ FLUX-dev or any NC-licensed AI gen (Gate 3 violation)
- ❌ "test on device + report bugs" closure (user is CEO not QA)
- ❌ YELLOW shipping (Quality-Bar Lead [#50] enforces)
- ❌ User-prompted phases between -1..16 (autonomous run)
- ❌ Re-mentioning killed approaches in retros
