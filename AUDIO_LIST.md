# AUDIO_LIST — DROP

> Phase 5b. Owner: [#G12] Audio Designer. 2026-05-02.
> All audio synthesized in Web Audio API at runtime. Zero external samples (license-clean).

## Music tracks

| ID | Purpose | Length | BPM | Key | Notes |
|---|---|---|---|---|---|
| music_main | Game-loop bed | 32s loop | 90 | A minor | Synthwave + chiptune fusion. Lo-fi pad + 8-bit lead arp + sub bass. Volume bed -18dB. Ducks -3dB during hype events. |
| music_menu | Menu/splash | 16s loop | 90 | A minor | Stripped version (pad + arp only, no bass). |

Implementation: WebAudio OscillatorNode + GainNode + BiquadFilter. Procedural composition. No sample files.

## SFX (event → sound)

| ID | Event | Duration | Pitch | Synth | Notes |
|---|---|---|---|---|---|
| sfx_chime | UI open / restart / hype prompt | 200ms | A4 + E5 (perfect 5th) | Sine + envelope (attack 5ms, release 195ms) | Soft, non-intrusive |
| sfx_drop | Ball release | 80ms | descending C5→A4 | Square + low-pass filter | "Whoosh" of release |
| sfx_peg | Peg hit | 40ms | base C5, +0.4 semitone per consecutive chain peg | Square wave + short envelope | Pitch ramps higher each chain peg (rising tension) |
| sfx_zone_land | Ball lands in zone | 120ms | E4 + B4 (perfect 5th) | Sine + sub thump | Resolution feel |
| sfx_match | 3+ same-color match in zone | 250ms | E5 → A5 → C6 (rising arpeggio) | Square arp | Reward chime |
| sfx_cascade | Cascade ball spawn | 300ms | sub at 80Hz + harmonic at 240Hz | Sine sub + saw harmonic | Weighty thump |
| sfx_combo | Combo counter increment | 60ms | base D5, +0.3 semitone per increment, capped at 50x | Square + short envelope | Rising tension chip-tone |
| sfx_hype_50x | 50x combo achieved | 1200ms | C5 → C6 sweep + arpeggio | Saw sweep + chip arpeggio | Big celebratory moment |
| sfx_round_end | Round summary appears | 600ms | A minor scale descending (A4-G4-F4-E4) | Sine + decay | Calm resolution |
| sfx_near_miss | Chain breaks at 9 (just below hype threshold) | 200ms | minor 2nd dissonance F4-F#4 | Square + dissonance | Frustration cue (small) |
| sfx_ui_press | Button press | 60ms | E5 | Sine | Generic UI |
| sfx_load | Ball loaded at top | 100ms | C5 | Sine | Subtle "ready" |
| sfx_ui_open | Settings open | 150ms | A4 + E5 chord | Sine | Pleasant |

## VO (voiceover)

NONE in v1. Aesthetic is purely sonic-juice + music; no spoken/sung content.

## Stingers (level transitions / big events)

| ID | Event | Length | Notes |
|---|---|---|---|
| stinger_pb | New personal best on round-end | 1.5s | Major chord arpeggio C-E-G-C, harmonic resolution |
| stinger_cascade_chain | 3+ cascades in same round | 800ms | Cascade descending bass + ascending lead |
| stinger_first_play | Very first round start (per device) | 600ms | Welcome chord, only fires once |

## Audio mixing rules

- Master volume: -3dB ceiling (no clipping)
- Music bed: -18dB (always present, ducks for events)
- SFX: -10dB peak (loud relative to bed for impact)
- Stingers: -8dB peak (most prominent)
- Mute toggle: top-right settings button, persistent in localStorage
- iOS Safari: audio must start on user gesture (first tap unlocks AudioContext)

## Implementation notes

- WebAudio API only (no `<audio>` tags — too high latency for SFX)
- Synth on-the-fly, no sample loading (zero asset weight)
- Pre-warm AudioContext on first user gesture
- Polyphony cap: 8 simultaneous SFX (older mobile CPU friendly)
- Music: 2 tracks total (game + menu), fade-cross-mix on transition

## DoD (Phase 5b gate)
- [x] Every interactable in ASSET_MATRIX has SFX entry (peg, zone, ball drop, etc.)
- [x] Music BPM + key consistent across tracks (90 / A minor)
- [x] No license-encumbered samples
- [x] iOS Safari audio-unlock-on-gesture spec'd
- [x] Mute toggle + persistence
- [x] Pitch-shift rule per chain peg (BENCHMARK Feature 2 alignment)
