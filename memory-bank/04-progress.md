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
- Combat step 2: types + session store (`server/src/combat/`)
- MMO logging: server `log.ts` + client `GameLog` files, ring dump, quiet INFO
- Server tests **102 pass / 1 known flake** (`spirit boosts elemental damage`, variance); King Slime spawn test is green
- **Combat pipeline repair:** approach-cast-damage; no fake death from empty `hpAfter`; skill lists parse arrays not catalog scrapes
- **King Slime** on ridge (8,14): 140 HP / ATK 14 / larger sprite + `king_slime` loot
- **P1-08 Map transition:** fade + map name + biome floor art panel on `sync_state` map change
- **Combat:** 0-HP corpses reap; Shot hits sprite-sized bodies; predicted HP cannot fake-kill
- **P1-07 Boss readability:** telegraph disc + phase HP bar + distinct scale/tint for the four named bosses
- **P1-06 Quest chain:** Mira → pests → Crypt/Marsh → Tower F2 → Apex → L20 class path; HUD tracker + J log
- **P1-05 Consumables:** stew timed ATK/DEF; R quick-use; homestone VFX; pixel held-weapon mark
- **P1-04 Gear ladder:** Leather L1 / Iron L8 / Ash L15 on armor·helm·boots·gloves·acc; `levelReq` on equip; loot + smith/town shops; iron in banner SR
- **P1-03 Dual weapon visible:** HUD W1/W2 chips, N swap feedback, sprite weapon mark; Adventurer seeds bow as Sekundär
- 14-step `DamagePipeline` parked **after MVP** (`damage-pipeline-ai-prompt.md`)
- **Enemy loop:** slime AA damage + HP ratio; aggro/chase/leash; wall slide; orc/plant melee; world HP bar predict on cast
- Project index: `memory-bank/00-index.md` · logging contract: `memory-bank/07-logging.md`
- Adventurer L1–20 kit locked (8 skills + dual weapons; class cards ≥20)
- **MVP gap list:** `memory-bank/08-mvp-todo.md` (canonical remaining work)
- **P0-01 Gacha identity:** banner pulls cards/spirits/gear/portraits; starter bag shrunk; G pity panel
- **P0-02** Damage resolver (`combat/damage.ts`) + element floats
- **P0-03** Homestone respawn UI (no reconnect)
- **P0-04** AA+Shot at L1; remaining skills gated + trainer curve
- **P0-05** Class-change panel; banner primary / 500g shop backup
- **P0-06** First-session tutorial tips
- **P0-07 Loop economy:** `loot.json` per monster type; XP curve 3192 to L20 (tower-weighted); 1-pull 10 dust / 10-pull 90 or tickets; shop gear + ticket gold sinks
- **P1-01** Buff-row icons (stun / DoT / Rally / Decoy); 8-skill hotbar; Shot projectile sweep; Ridge Orc + Ridge Plant

## Open

Canonical checkboxes: [`08-mvp-todo.md`](08-mvp-todo.md). Summary:

- **P0** ~~Gacha identity~~ · ~~combat step 3~~ · ~~death/respawn~~ · ~~L1–20 skill unlocks~~ · ~~class-change moment~~ · ~~tutorial~~ · ~~loot/XP economy~~
- **P1** ~~status icons~~ · ~~skill HUD~~ · ~~dual-weapon visible~~ · ~~gear ladder~~ · ~~consumable buffs~~ · ~~main quest chain~~ · ~~boss readability~~ · ~~loading screens~~
- **P2** Assets: class sprites, NPCs, ~8 enemy bodies, tiles, icons, VFX, SFX
- **P3** Tower/shop/flavor data; one rule for tomes vs SP
- **P4** Legacy Unity tree, README sync, idle/run clips, guest-login clarity

MMO-scale (shards, persist auction/guild, real guilds) stays **out of MVP** — see 08 “Out of MVP”.

## Known Bugs
- Port 7777 `EADDRINUSE` → kill old node (logged SYS ERROR)
- Login/register needs `DATABASE_URL` (file slots still work for guests)
- Esc resolution in Unity Editor needs matching Game view size (standalone OK)
