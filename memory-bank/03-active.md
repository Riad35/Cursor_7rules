# 03-active.md — Aktueller Arbeitskontext

## Current Focus

**Playtest fixes:** white hit-rings + slime_yard density; commit/push.

## Recent Changes

- Hit rings were opaque white quads (looked like white squares) — transparent ring, hidden for art sprites
- slime_yard trimmed to 12 slimes + dummy; proximity aggro no longer seeds every packed unit
- Pending respawn dedupe + clear monster AI on death
- Combat step 2 scaffolding still present (memory session store); Redis not required

## Blockers

- none

## Decisions Pending

- Approve → combat step 3 (pure damage resolver)
