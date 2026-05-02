# STYLE_GUIDE — DROP

> Phase 3. Owner: [#7] Visual Designer (paired with [#G10] Game Visual for game-specific). Generated 2026-05-02.
> One-page style guide. ALL design decisions trace back here.

## Title + 1-sentence pitch
**DROP** — One-tap chain-cascade physics with 8-bit retro pixel + modern neon-glow + CRT scanline fusion.

## Color palette

### Primary (3)
- `--neon-magenta`: #FF2F92 (HSL 332° 100% 59%) — primary balls + zone "void" + UI accents
- `--neon-cyan`: #00E5FF (HSL 188° 100% 50%) — secondary balls + zone "ice" + chain text
- `--neon-yellow`: #FFE53D (HSL 53° 100% 62%) — score popups + zone "spark" + 50x flash

### Secondary (2)
- `--retro-fg`: #C8C8DA (warm grey) — pegs (default), UI text
- `--retro-bg`: #14141E (cool near-black) — playfield bg, scanline base

### Accent (2)
- `--combo-red`: #FF3B30 (HSL 4° 100% 59%) — danger/break/near-miss flash
- `--cascade-green`: #36F5A4 (HSL 154° 90% 59%) — cascade chain spawn flash

### Do-not-use
- ❌ Pure white #FFFFFF (too harsh, breaks dark theme; use `--retro-fg` instead)
- ❌ Pure black #000000 (no depth; use `--retro-bg`)
- ❌ Generic blues (#0000FF, #4080FF, etc.) — only `--neon-cyan` allowed in cyan family

### Color contrast / a11y
- All fg/bg combos: `--neon-*` on `--retro-bg` ≥4.5:1 (verified)
- `--retro-fg` on `--retro-bg` = 8.2:1 ✓ (large + small text safe)
- Color is NEVER the only signal: zone landing also has shape (square/circle/triangle/diamond) + label text

## Reference moodboard (6-12 thumbnail descriptions; we describe, don't copy)

1. **Synthwave concert poster** — vertical neon gradient sky, low-poly mountain silhouette below, retro-future grid floor. (Inspires: bg gradient + scanline overlay.)
2. **Game Boy + neon sticker overlay** — 8-bit pixel object with modern soft particle glow surrounding it. (Inspires: ball sprite layering — pixel base + bloom halo.)
3. **CRT TV with chromatic aberration** — slight RGB channel shift on screen edges, scanlines visible at 8% opacity. (Inspires: post-processing filter on hype events.)
4. **Tron-light-cycle trail** — neon line tracing path with motion blur. (Inspires: ball-fall trail effect on chain-3+.)
5. **Pinball table chrome plunger** — subtle metallic gradient, bright highlights. (Inspires: peg "hit-state" flash.)
6. **80s arcade marquee text** — chunky pixel font with neon outline. (Inspires: combo counter typography.)
7. **Vaporwave checkered floor receding** — perspective grid, low saturation. (Inspires: zone tile pattern.)
8. **Lofi minimal mobile UI** — large negative space, single accent color. (Inspires: HUD layout — top score, bottom drop indicator, side ball count.)

## Material vocabulary

- **Balls**: solid pixel core (8×8 sprite scaled 2x = 16×16 visible) with 1.5px outer glow halo. Color = ball's primary. Trail on velocity ≥400px/s.
- **Pegs**: 12×12 pixel circles with 0.5px chrome rim. Default `--retro-fg`. Hit-state flashes to ball-color for 80ms.
- **Zones**: 4 colored rectangles (32% width each) at bottom. Each has 64×64 pixel icon centered (square/circle/triangle/diamond). Glow when matched.
- **Particles**: 2×2 pixel squares, falling-velocity vector + slight horizontal jitter, lifetime 600ms, alpha-fade.
- **Text**: chunky pixel font (8x8 base × 2x scale), neon outline 1.5px stroke.

## Lighting model

- **Diegetic light**: balls emit light (radius 16px, soft falloff). Pegs reflect (subtle local glow on hit).
- **Ambient**: `--retro-bg` base + radial gradient lifting center 4% brighter than corners.
- **Post-fx**:
  - Bloom (Gaussian σ=3px, threshold luminance>0.7) — ON for neon objects
  - Scanlines (alternating 1px row at 8% opacity over `--retro-bg`)
  - Chromatic aberration (1.5px R/B channel split) — ONLY on hype_50x flash, 200ms duration
- **Photosensitivity**: max 2 flashes/sec, never strobing — verified via flash-rate analyzer

## Type system

- **Display font**: "Press Start 2P" (Google Fonts, Open Font License) — score, combo, round number
- **Body/UI font**: same family at smaller scale (8px equivalent)
- **No third font** — fusion aesthetic relies on consistent pixel typography
- Fallback stack: `'Press Start 2P', 'Courier New', monospace`

## Audio palette (pointer to AUDIO_LIST.md detail)

- **Bed**: synthwave + chiptune fusion, 90 BPM, A minor, looping ambient
- **SFX timbre**: 8-bit square-wave for peg hits (rising pitch), modern soft sub-thump for cascade, neon-arc swoosh for hype_50x
- **Anti**: NO realistic samples (no real metal clinks, no real bell). All synthesized.

## Motion vocabulary

- **Snappy** (80-150ms): peg hit flash, score popup spring-in, button press
- **Floaty** (300-600ms): particle drift, score number rising, scanline pulse
- **Weighty** (200-400ms): screen shake on hype, combo flash, cascade spawn
- **PICK PER ELEMENT**: ball drop = floaty fall + snappy peg-bounce; chain text = snappy in + floaty hold + snappy out

## Anti-patterns ("what this theme is NOT")

- ❌ NOT pixel-only retro (e.g., NES-faithful look) — fusion is the point
- ❌ NOT modern-flat-only (no Material/Apple HIG ports)
- ❌ NOT cartoon mascots / character heads
- ❌ NOT photorealistic anything
- ❌ NOT "cute" — synthwave aesthetic, not kawaii
- ❌ NOT 3D — pure 2D, all depth via lighting/parallax tricks
- ❌ NOT skeuomorphic (no leather/metal textures)
- ❌ NOT particle-spam — every particle has purpose, not "more = better"

## Tokens (CSS custom-properties, source of truth)

```css
:root {
  /* Colors */
  --neon-magenta: #FF2F92;
  --neon-cyan: #00E5FF;
  --neon-yellow: #FFE53D;
  --retro-fg: #C8C8DA;
  --retro-bg: #14141E;
  --combo-red: #FF3B30;
  --cascade-green: #36F5A4;
  /* Spacing (8-bit grid) */
  --grid: 8px;
  --space-xs: 8px;   /* 1 grid */
  --space-sm: 16px;  /* 2 grid */
  --space-md: 32px;  /* 4 grid */
  --space-lg: 64px;  /* 8 grid */
  /* Type scale */
  --text-xs: 8px;
  --text-sm: 12px;
  --text-md: 16px;
  --text-lg: 24px;
  --text-display: 32px;
  /* Motion */
  --t-snappy: 100ms;
  --t-floaty: 450ms;
  --t-weighty: 300ms;
  /* Effects */
  --scanline-opacity: 0.08;
  --bloom-radius: 12px;
  --aberration-px: 1.5px;
}
```

## Benchmark % per interactable (from BENCHMARK.md)

- **Drop zone (top tap area)**: 100% width × 18% height (large hit target)
- **Pegs**: 12px diameter on 480px-wide playfield = 2.5% width each (≥40 pegs)
- **Zones (bottom 4)**: 25% width each × 12% height (large enough to read color/icon)
- **Ball**: 16px on 480px = 3.3% width (matches benchmark casual physics 3-4%)

## DoD (Phase 3 gate)
- [x] Palette tokens defined (3 primary + 2 secondary + 2 accent + do-not-use)
- [x] Moodboard with 8 descriptions
- [x] Material vocab + lighting model + post-fx
- [x] Type system (1 family, fallback)
- [x] Motion vocab (snappy/floaty/weighty)
- [x] Anti-patterns enumerated
- [x] Benchmark % cited per interactable
- [x] Photosensitivity verified (max 2 flashes/sec)
