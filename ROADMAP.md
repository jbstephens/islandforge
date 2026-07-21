# Island Forge — roadmap after v7 (THE ASTEROID RUSH)

This is the agreed phase plan from the v7 design sessions (July 2026).
Rule for every phase, per the arcade's CLAUDE.md: **decide the design first,
in writing, with John** — then one implementation agent with the full
verification loop as its return condition (headless suites + op budgets +
screenshots), additive saves only, append-only goals, and the final perf
verdict from the Pi. Do not start a phase in the same session that is
mid-way through shipping the previous one.

## v8 — THE NINE PLANETS

**Vision (decided):** with three megastructures raised and the beacon
star-map assembled (`mapAssembled` — v7 already sets it), Earth can build an
INTERPLANETARY EXPLORATION STATION (megastructure-scale, staged construction
like the others) and an exploration ship. That unlocks a planet-picker — the
v7 star map becomes an interactive screen — and travel to ANY of the nine
planets (Pluto is a planet here, non-negotiable). Each planet is a small
persistent colony in the Planet Pattern (like Moon/Mars, smaller), with
planet-specific buildings, 1-2 unique resources, one signature hazard or
mechanic, and a planet-specialty global bonus (the MARS ★ +10% pattern).
Planets are chosen, not visited in order; Venus is a specialized heat
challenge (floating habitats, heat-shield tech), not "the next stop."

**Decided anchors:**
- Gate: `mapAssembled` + STAR HARBOR complete. Alien Alloy (from rare
  asteroids) is a construction ingredient for the station and per-planet
  landers — keeps v7 expeditions relevant forever.
- Reuse: world bundles, per-ship travel routes, survival-arc pattern
  (soften per planet — not every world needs one), additive saves.
- Goals ladder continues append-only; each planet contributes 2-3 goals.

**Open design decisions — settle with John BEFORE building:**
1. First drop: all 9 planets, or 3 waves of 3? (Recommendation: waves —
   ship value sooner, keep each wave's planets genuinely distinct.)
2. The per-planet mechanic list (one line each, written, before code).
3. Whether expeditions gain planet-flavored asteroid types in v8.
4. Station + ship visual design and costs.

**Technical risks to resolve first (in this order):**
1. **Memory on the Pi**: every parked world carries two full-world canvases.
   Nine more worlds needs lazy creation (exists — worlds spawn on first
   arrival) plus likely bake-release for unvisited worlds (drop terrain/bg
   canvases of parked worlds, rebuild via `terrainDirty` on swap). Measure
   real memory on the Pi with a synthetic 12-world save before committing
   to the design.
2. **The file split**: index.html will pass ~15k lines in v8. Per the July
   discussion: do the source split (src/ modules → build step → identical
   single-file artifact) as its OWN commit, byte-identical output verified
   by diff, BEFORE any v8 feature work. Never mix restructure with features.

## v9 — INTERGALACTIC (deliberately unspecified)

**Vision only:** an intergalactic station, a journey to another galaxy, and
the payoff of the alien beacons — whoever left them is out there. Everything
else is a blank page on purpose: design it with John (and the boys — this
one should probably be theirs) after v8 ships. Expect: one new galaxy world
with its own rules, an alien-civilization reveal, and the goals ladder's
first contact.

## Backlog (unscheduled, from v7 audit)

- Endgame HUD density (17-18 icons at fs 9) — revisit if TV legibility
  complains; candidate: two-row HUD or per-world resource filtering.
- Stats panel bottom-edge crowding when every line is active.
- Optional QoL: "ride the whole line" express from Moon too, if kids ask.
