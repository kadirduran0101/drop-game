# DROP

One-tap chain-cascade physics game. Modern + retro fusion.

## Quickstart

```bash
npm install
npm run dev    # serve www/ at http://localhost:3000
```

## What

Tap top of screen → drop ball → bounce through pegs → land in colored zone → trigger chain cascades. 60-second runs. No tutorials, no ads, no accounts.

## Why

See [STRATEGY.md](./STRATEGY.md). Built per `/game-build` orchestrator (Phase -1..16). Solo Tier (9 active roles per `~/.claude/skills/game-build/COMPANY_STRUCTURE.md`).

## Stack

- Vanilla JS + HTML Canvas 2D + WebAudio (no framework)
- Zero backend, all state local
- Vercel static deploy
- $0 cost
- See [ARCHITECTURE.md](./ARCHITECTURE.md)

## Test

```bash
npm test            # unit tests
npm run harness     # real-input harness (no .click() shortcuts)
npm run ship-gate   # full pre-deploy check
```

## Deploy

```bash
git push origin main   # Vercel auto-deploys
```

## Stop criteria

D7 retention <8% by week 4 → archive. See STRATEGY.md §9.
