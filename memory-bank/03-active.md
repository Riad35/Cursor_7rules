# 03-active.md — Aktueller Arbeitskontext

## Current Focus

**Combat lab playtest:** cast desync fixed; expand `test_arena` verified in-client.

## Recent Changes

- Cast desync: soft local collision, range from `_lastGood*`, real hit radii, living lock refresh, `out_of_range` retries same skill; server rejects cross-map UNIT targets
- Stubs synced → Unity `_Project/Scripts`
- `test_arena` → 40×28 Combat Lab (melee/ranged/dummy/force/chase/hazard/cannon/variety); `slime_yard` empty stub
- Zone smoke checklist: `docs/ATTACKS.md` → Combat Lab

## Blockers

- none (Unity playtest for floats/hurt/counterattack still manual)

## Decisions Pending

- Approve → combat step 3 (pure damage resolver)
