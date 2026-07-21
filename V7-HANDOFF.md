# V7 HANDOFF — for the terminal Claude Code session (delete before merging)

Island Forge v7: **THE ASTEROID RUSH** — asteroid mining expeditions + Earth
megastructures. Built and headlessly verified in a Claude Code web session on
branch `claude/island-forge-features-y05lqd`. Your job is the half that needs
the Mac + home network: merge → deploy → bundle → **Pi verification**.
Read `gameconsole/CLAUDE.md` first; the ship pipeline and conventions live there.

## What shipped (~2,100 added lines, all in index.html)

- **Survey Barge**: funded in 3 installments at the Mars Shipyard (gated on
  MARS BASE COMPLETE); +4-crate upgrade after the first expedition.
- **Expeditions**: board → warp → scan screen (3 seeded asteroid cards:
  resource, 1–3 stars, hazard; rare alien-tech card ~1-in-6 with pity ≤8) →
  temporary mission world (7 handcrafted templates × seeded variation, baked
  starfield, 2–4 hop-able chunks) → mine deposits (3 damage stages; big ones
  need the pop cannon or a MINI DRILL) → carry chunks → bank crates at the
  loader → both players board to leave. Ship-battery timer with warning
  phase; timeout or mid-mission quit = friendly RESCUE TUG (banked crates
  kept, never lose the ship).
- **Jet hop**: walking off a chunk edge arcs you to land within 6 tiles or
  bounces you back — stranding is impossible (64-direction sweep asserted).
- **Cargo → Earth**: crates ride with the crew; first haul does the full
  Mars→Moon→Earth relay, which unlocks the EXPRESS line; the Star Harbor
  megastructure later automates delivery entirely.
- **Earth endgame**: REFINERY converts astro iron + crystal → STARSTEEL;
  three megastructures fed in visible stages (scaffold/crane → floors →
  fireworks): ARCOLOGY 3x3 (pop +120, repeatable), CRYSTAL SPIRE 2x2
  (science, night beacon, repeatable), STAR HARBOR 3x3 (once; auto-cargo).
- **Long-arc tease**: rare asteroids hold ALIEN BEACONS; 3 beacons → star-map
  vignette → final goal reads "THE NINE PLANETS AWAIT — SOMEDAY!" (v8 hook).
- 4 new resources (HUD-gated until seen), 10 appended goals (57 total),
  5 mission hazards, MILO & CHIP gossip, strictly additive saves (v6 saves
  load clean; reforge resets v7, stars persist).

## Already verified headlessly (don't re-do; spot-check at will)

- 8 Playwright suites, 241 assertions, **0 failures, 0 page errors**, re-run
  independently after final polish edits. Coverage: full v6 regression,
  entire v7 ladder end-to-end via real input paths (goalI walks 47→57),
  rescue + mid-mission-reload safety, v6-save compat, pity, cargo overflow,
  reforge, 2P both-must-board + independent hops, keyboard-only full loop,
  touch smoke. Scripts lived in the web session's scratchpad (not committed).
- Perf, instrumented on-screen context, worst cases: Earth + 3 lit megas
  **75.7 drawImage/frame**, busiest mission (pebble shower + chunks + manned
  cannon) **72.5**, both **0 path ops** — well under the <150 budget.
- Screenshots audited by the orchestrator (see `shots/v7-*.png`).

## Your checklist

1. Merge this branch to `main`, push, poll the deploy:
   `curl -fsSL https://islandforge.onrender.com/ | grep -q 'RESCUE TUG INBOUND'`
2. gameconsole: `bash scripts/bundle-games.sh`, verify the island-forge
   bundle got v7 + `__arcade_back`/`__arcade_pad_exit`/`__arcade_lowfx`,
   commit + push, confirm live at ses.q5labs.co/games/island-forge/.
3. **Pi fps** (the real verdict — headless numbers miss compositing costs):
   tunnel per CLAUDE.md, `targets` first (never hijack a live game), nav to
   the game, `fps 4` on the TWO worst cases:
   - **Busy mission**: via CDP eval use `window.__test` — `setAge(7)`, `grant`
     big resources, `startExpedition(0)`, `fastChunks()`, then
     `forceDisaster` is Earth-only; hazards come with the template — the
     hooks `depositsInfo()/chunksInfo()` confirm a busy scene.
   - **Night skyline**: on Earth `grant({starsteel:500,crystal:100,alloy:50})`,
     place + `feedMegaAt` all three megas (see `megaInfo()`), full town lit.
   Want ~60 on both. If sub-60: check DOM/compositing suspects before canvas
   code, fix on this branch, redeploy, re-measure. **A sub-60 build does not
   reach the console menu.**
4. Human check on TV/iPad: fund the barge, one full expedition, scan-screen
   touch taps, jet hop feel, megastructure feed prompt.
5. Park the kiosk on https://ses.q5labs.co/, delete this file, report fps.

## Known rough edges (cosmetic, pre-triaged as shippable)

- Endgame HUD is dense (17–18 icons, fs 9) — eyeball legibility on the TV.
- Stats panel right column brushes the bottom when every line is active
  (pre-existing v6b crowding).
- Transient float-on-float stacking when several CHUNKS! floats spawn close
  together (matches existing game behavior).
- Boarding starts if ✕ is pressed within 78px of the cargo loader — visible
  countdown, ○ cancels.
