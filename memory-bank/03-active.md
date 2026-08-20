# 03-active.md — Aktueller Arbeitskontext

## Current Focus

**P1-08 Map transition** shipped (fade + map name + floor-tile art panel). Next: **P2 assets** (player/NPC/enemy sheets, skill/item icons). Canonical list: [`08-mvp-todo.md`](08-mvp-todo.md).

## Recent Changes

- **Logs:** Unity `Unrecognized_message` was movement JSON with German decimal commas. Move packets now use invariant `3.5`. Errors print a full sentence, not `msg=Unrecognized_message`.
- **Combat fix:** 0-HP enemies now death-clip + despawn (server reaps leftovers; client never fake-kills from predicted HP). Shot collision matches sprite scale; bolt stops on the body.
- **P1-08:** Portal/homestone `sync_state` shows a 1.2s load overlay (map name + biome floor art)
- **P1-07:** Crypt Warden / Ruins Colossus / F2 Warden / Apex: windup telegraph disc, 66%/33% phase bar, unique scale+tint+body
- **P1-06:** Main quest chain to L20 class card

## Blockers

- Restart Node 7777 + Unity Play after C# copy
- No dedicated bow body sheet yet (P2)

## Decisions Pending

- None
