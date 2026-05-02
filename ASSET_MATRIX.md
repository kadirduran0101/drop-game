# ASSET_MATRIX — DROP

> Phase 4. Owners: [#7] Visual + [#G10] Game Visual + [#10] Design Systems. 2026-05-02.
> Rule: every interactable + every state, with theme rationale and source.

| ID | Name | Theme rationale | Source | Resolution | Path | Status |
|---|---|---|---|---|---|---|
| ball-magenta | Magenta ball | Neon-magenta = "void" zone match (synthwave palette) | code-drawn (canvas) | 16×16 logical, 2x display | `src/sprites/ball.js` | TODO |
| ball-cyan | Cyan ball | Neon-cyan = "ice" zone match | code-drawn | 16×16 | same file | TODO |
| ball-yellow | Yellow ball | Neon-yellow = "spark" zone match | code-drawn | 16×16 | same | TODO |
| ball-green | Green ball | Cascade-green = bonus ball (cascade spawn) | code-drawn | 16×16 | same | TODO |
| peg-default | Default peg | Retro chrome rim, neutral until hit | code-drawn | 12×12 | `src/sprites/peg.js` | TODO |
| peg-hit-flash | Peg hit-state | 80ms color flash to ball-color (juice) | code-drawn animation | 12×12 | same | TODO |
| zone-void | Magenta zone (square icon) | Bottom rectangle, matches magenta balls | code-drawn | 120×60 (25% width) | `src/sprites/zones.js` | TODO |
| zone-ice | Cyan zone (circle icon) | Same | code-drawn | 120×60 | same | TODO |
| zone-spark | Yellow zone (triangle icon) | Same | code-drawn | 120×60 | same | TODO |
| zone-bonus | Green zone (diamond icon) | Cascade-green for cascade-spawn landing | code-drawn | 120×60 | same | TODO |
| particle-spark | Hit-impact particle | 2×2 pixel, ball-color, 600ms life | code-drawn (pool of 60) | 2×2 | `src/sprites/particle.js` | TODO |
| trail-segment | Ball-fall trail | Velocity-based motion line, 8 segments | code-drawn | varies | `src/sprites/trail.js` | TODO |
| score-popup | Score number popup | Pixel font number rising on land | code-drawn | varies | `src/sprites/popup.js` | TODO |
| combo-text | Combo counter | Center-screen "5x → 10x → 50x" climb | code-drawn | varies | `src/sprites/combo.js` | TODO |
| hype-flash | Hype 50x screen flash | Full-screen yellow tint + chromatic aberration | post-fx | viewport | `src/postfx/hype.js` | TODO |
| scanlines | CRT scanline overlay | Alternating 1px rows, 8% opacity | post-fx (CSS or canvas mask) | viewport | `src/postfx/scanlines.js` | TODO |
| bloom | Neon glow bloom | Gaussian blur threshold luminance>0.7 | post-fx | viewport | `src/postfx/bloom.js` | TODO |
| btn-restart | Restart button | Bottom-right, 44×44pt min hit | code-drawn | 64×64 | `src/ui/buttons.js` | TODO |
| btn-share | Share screenshot button | Appears on hype_50x event, near combo | code-drawn | 64×64 | same | TODO |
| btn-settings | Settings (mute, theme cycling) | Top-right corner | code-drawn | 32×32 | same | TODO |
| splash-frame | Splash screen | Logo + "tap anywhere" 1-second display | code-drawn | viewport | `src/screens/splash.js` | TODO |
| privacy-link | Footer privacy link | Required for store distribution | HTML anchor | text | `www/index.html` | TODO |
| terms-link | Footer terms link | Required for store distribution | HTML anchor | text | same | TODO |

## Theme audit (post-Phase 9 verify)

- [ ] From playfield alone, theme guessable in 3s? (synthwave + 8-bit fusion clear?)
- [ ] No meaningless decorative shapes (every visible element has purpose)
- [ ] Foreground/background hierarchy explicit (balls/pegs prominent, scanlines subtle)
- [ ] 4 zones distinguishable by color AND shape (a11y)

## Source attribution (for LICENSE_TRAIL)

- All sprites: code-drawn programmatically OR authored by solo dev in PixilArt (CC-BY-4.0 self-attribution)
- Font: "Press Start 2P" — Google Fonts (Open Font License 1.1)
- Audio: see AUDIO_LIST.md
- NO AI generation for v1 (avoids 2026-05-02 FLUX-dev license issue from cyber-pinball)

## DoD (Phase 4 gate)
- [x] All interactables enumerated (23 entries)
- [x] All have theme rationale
- [x] All have source (code-drawn / authored / library)
- [x] All have target path
- [x] License-clean (no NC-licensed asset)
