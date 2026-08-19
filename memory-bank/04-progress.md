# Progress

## Done
- Hub + gap + social/content slices
- Polish: trade items, skill tree, auction, instance timer, boss phases
- Tower + Login + Classes
- Art pass 1: Craftpix swordsman/slime + terrain
- 2.5D camera + facing sprites + 12×12 inventory + Esc settings/logout
- Camera-basis move/facing fix
- Combat anims (death/hurt/attack/walk+run attack) + lock outline on sprite (+2px)
- Test arena → Combat Lab (40×28 zoned) + cast desync fix (soft collision / lastGood range)
- Esc UI scale via GUI.matrix + resolution prefs apply
- Server tests **66/66**
- Project index: `memory-bank/00-index.md`

## Open
- [P1] Hygiene: archive legacy Unity tree; README sync note
- [P2] Art: NPCs, class sprites, tiles/VFX polish
- [P3] Content: more tower/quests/shops from data
- [P4] MMO-scale (multi-process, persist social/auction, real guilds)

## Known Bugs
- Port 7777 `EADDRINUSE` → kill old node
- Login/register needs `DATABASE_URL` (file slots still work for guests)
- Esc resolution in Unity Editor needs matching Game view size (standalone OK)
