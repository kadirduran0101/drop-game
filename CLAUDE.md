# CLAUDE.md — DROP project constitution

> Auto-generated from `/game-build` Phase 6. DO NOT delete; orchestrator reads this on every session.

## Stack (locked, see ARCHITECTURE.md)
- Vanilla JS, no framework
- HTML Canvas 2D
- WebAudio API (synthesized, no samples)
- localStorage + IndexedDB
- Single-file `www/index.html` for v1

## DoD (every commit)
- [ ] Tests pass: `npm test`
- [ ] Real-input harness GREEN: `npm run harness`
- [ ] Bundle ≤200KB gzip (verified: ARCHITECTURE Phase 6)
- [ ] No console errors during 60s play
- [ ] No .click() shortcuts in tests (real-input only)
- [ ] No PII in telemetry events
- [ ] No NC-licensed assets (LICENSE_TRAIL clean)

## Anti-patterns (auto-block)
1. Adding framework (React/Vue/etc) — refuse, defer to v2 if scale forces
2. Adding backend / accounts / IAP without ADR — refuse for v1
3. Mid-task interstitial ads or energy gates — refuse (4 Gates ETİKLİ)
4. AI generation for production assets without license check — refuse (per FAILURE_MODE_LEDGER 2026-05-02)
5. Calling user a tester ("test on device + report bugs") — refuse (per memory feedback_no_user_testing.md)
6. Shipping YELLOW QA verdict — convert to GREEN or RED, never YELLOW (per Quality-Bar Lead [#50])

## Hard rules
- Solo Tier active roles: 9 (per RoleActivation.json). NEVER 10+ without escalating scale.
- Phase ownership table in `~/.claude/skills/game-build/SKILL.md` — every artifact has canonical owner role-id.
- ACCEPTANCE_CRITERIA.md is the gate. Every PR/commit must reference an AC it advances.

## File map (where things live)
- `src/` — JS modules, rolled to `www/index.html`
- `www/` — built deployable artifact
- `automation/scripts/` — harnesses (real-game-test.mjs, role-driven-qa.mjs)
- `tests/` — unit
- 12 design docs at root (STRATEGY through AUDIO_LIST)

## Re-entry checklist (start of every session)
1. Read this file
2. Read RoleActivation.json (active 9 roles)
3. Read ACCEPTANCE_CRITERIA.md (current gates)
4. Read FAILURE_MODE_LEDGER.md at `~/.claude/skills/game-build/` (past P0 patterns)
5. Determine current phase from `git log` + artifact presence
6. Continue from current phase per Phase ownership table
