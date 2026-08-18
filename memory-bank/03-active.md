# 03-active.md — Aktueller Arbeitskontext

## Current Focus

**Nostale gap slice shipped:** Postgres+login wiring, procedural sprite pass, XP/levels, full gear slots, crypt dungeon instances. Next candidates: art polish / trade / party HUD / more biomes.

## Recent Changes

- `DATABASE_URL` + schema on boot; register/login (`L`); file fallback
- Biome tiles + silhouette sprites (`SpriteCatalog`)
- XP on kill, armor/helm/boots/gloves/accessory equip
- `dungeon_crypt` private instances (party-shared); exit portal
- Classes: warrior / mage / archer selectable at create
- Server tests **52/52**; stubs → gatcha1

## Blockers

- keine (set `DATABASE_URL` in `gAAAcha/server/.env` to enable Postgres)

## Decisions Pending

- none critical
