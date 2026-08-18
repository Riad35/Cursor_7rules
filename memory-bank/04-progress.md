# Progress

## Done
- MOBA indicator-cast
- Party / guild
- Playable Hub Loop (graybox)
- **Gap slice:** Postgres persist/login, sprite silhouettes, XP/levels, full equip, crypt instances
- Server tests 52/52; stubs → gatcha1

## Open
- [P3] Real art / tilesets / VFX (beyond procedural silhouettes)
- [P3] Trade, friends, party HUD, more biomes/dungeons
- [P4] MMO-scale (multi-process, auction, etc.)

## Known Bugs
- Port 7777 `EADDRINUSE` → alten Node killen, `npm run dev`
- Login/register needs live Postgres (`DATABASE_URL`); otherwise `db_offline`
