# 08-mvp-todo.md — MVP-Lückenliste (Ziel vs. Ist)

Canonical remaining work for a **playable PC launch**. Systems backlog (full MMO): [`06-nostale-gap.md`](06-nostale-gap.md). Vision: [`01-project.md`](01-project.md). Adventurer lock: `adventurer_class_prompt.md`.

**Code:** sibling repo `gAAAcha`. **Scope:** Adventurer L1–20 + class cards + fair pity. No raids, PvP, housing, cash shop.

---

## Target loop

```
Create Adventurer → tutorial + first skills → field combat / loot / XP
→ town (shop, quest, homestone) → banner pull (pity) → equip / consumables
→ tower + instances → level 20 → class card → Fighter / Mage / Marksman / Rogue
```

Gray-box today is ~**70% systems / 20% content / 10% presentation**. The product inverts that.

---

## Ist vs. Ziel

| Schicht | Ist | Ziel-MVP |
|---------|-----|----------|
| Combat | Lock-on, 8 Adventurer skills, primitive VFX; ad-hoc damage in `gAAAcha/server/src/combat.ts` | Contract in `gAAAcha/docs/combat-rules.md` (hit → element → crit → mitigation → variance → floor) |
| Gacha | 1 banner, pity HUD string; pool = `char_aurel` / `char_nyla` / dust | Pulls that change the build; pity screen; reveal |
| Classes | Always Adventurer at create; cards gated at Lv20; **all 8 skills at create** | L1–20 unlock curve; visible class-change moment |
| World | 16 maps (town, ridge, marsh, crypt, ruins, tower 1–5, rest towns, combat lab) | Same maps, readable (tiles, NPCs, portals, **loading screen done P1-08**) |
| Art | 1 swordsman sheet + 1 slime sheet + 5 floor textures | Player/class bodies, ~8 enemies, NPCs, UI icons |
| UI | All `OnGUI` in `NetworkBootstrap.cs` | HUD / hotbar / inventory / gacha as real panels |
| Audio | 0 files | SFX minimum |
| Death | `"reconnect to respawn"` | Death screen + homestone respawn |
| Loot | Per-type tables in `loot.json`; dust is common, not guaranteed | Drop tables + gear ladder L1–20 (**P1-04 done**: Leather/Iron/Ash) |

**Identity break:** Vision is collector gacha (Aurel/Nyla). Later lock is class cards. Today `char_aurel` / `char_nyla` are dead inventory items (`kind: character`, no `use`). `seedStarterInventory` grants all weapons/spirits/chars — gacha and shops are meaningless.

**Locked for MVP (P0-01):** Banner pulls **class cards, spirits, gear**. Aurel/Nyla are **named skins / portraits**, not extra classes. Starter bag = sword + bow + ration + dust.

---

## P0 — Without this it is not the game

- [x] **P0-01 Gacha identity**
  - Re-pool `gAAAcha/server/data/banners.json`: cards / spirits / weapons / armor
  - Aurel + Nyla → skin or portrait, not a second class
  - Shrink `seedStarterInventory` in `gAAAcha/server/src/gacha.ts` to sword + bow + ration + dust
  - Pity UI as its own panel (counter, soft/hard, rates), not a HUD string
- [x] **P0-02 Combat step 3**
  - Pure damage fn per `gAAAcha/docs/combat-rules.md` §1–4 + unit tests
  - Wire `gAAAcha/server/src/combat.ts` to it
  - Element HUD (tinted floats / resist hint)
- [x] **P0-03 Death and respawn**
  - Keep death clip; add UI “Respawn at Homestone”
  - `you_are_dead` in `gAAAcha/server/src/net.ts` must not force reconnect
- [x] **P0-04 Adventurer L1–20 unlocks**
  - Stop granting full kit in `starterSkillsFor` (`gAAAcha/server/src/skills.ts`)
  - Suggested: L1 AA+Shot, then Shockwave / Dash / Rally / Hook / Mend / Decoy
  - Skill points + trainer must show the curve
- [x] **P0-05 Class-change moment**
  - Use card → panel, new skill set, new sprite, resist bonus
  - HUD: “Class card from level 20”
  - Cards in banner *or* expensive shop — pick one primary path
- [x] **P0-06 First 10 minutes**
  - Mini-tutorial: move, Tab lock, AA, inventory, homestone, first quest
- [x] **P0-07 Loop economy**
  - Per-monster loot tables (not always dust)
  - XP curve so L20 is reachable in a session chain (tower = main XP)
  - Gold sinks: shop gear + banner tickets / dust as pull currency

---

## P1 — Combat feel and RPG foundation

- [x] **P1-01 Status presentation** — stun / DoT / Rally / Decoy icons on buff row (no Redis session rewrite)
- [x] **P1-02 Skill presentation** — cast bar, CD sweep, names, MP cost, weapon-slot hint; hotkeys match `gAAAcha/docs/ATTACKS.md`; fix stale `gAAAcha/client/README.md`
- [x] **P1-03 Dual weapon visible** — sword vs bow icon/sprite; `N` swap feedback; grey or auto-swap skills that need the other slot
- [x] **P1-04 Gear ladder L1–20** — ≥3 tiers per slot (Leather / Iron / Ash), `levelReq`, loot + shop (not starter bag)
- [x] **P1-05 Consumables** — `buff_food` must apply timed ATK/DEF buff (`gAAAcha/server/src/shop.ts`), not heal-only; quick-use hotkey; homestone VFX
- [x] **P1-06 Quest chain to class change** — Town → Ridge pests → Crypt/Marsh → Tower F2 → F5 → Lv20 card; tracker in quest log (`gAAAcha/server/data/quests.json` is thin / talk-only)
- [x] **P1-07 Boss readability** — Crypt Warden, Ruins Colossus, Floor-2 Warden, Apex: telegraph disc, phase HP bar, unique sprite
- [x] **P1-08 Map transition** — loading screen (fade + map name + 1 art panel). Maps exist; vision required a load screen

---

## P2 — Assets (largest hole)

Wired today in `gAAAcha/client/Unity/gatcha1/Assets/_Project/Scripts/World/SpriteCatalog.cs`: player sheet + slime/orc/plant enemy bodies (other types reuse those three). NPCs = hex, portals = purple shapes, floors = one full texture per biome. StreamingAssets: `Sprites/player_*`, `Sprites/slime_*`, `Sprites/orc_*`, `Sprites/plant_*`, `Tiles/floor_*`, `Tiles/wall_mound`.

Unused Aseprite under `_Project/Art/Sprites/player/` (Swordsman lvl2/3, extra slimes).

### Characters (Idle / Walk / Run / Attack / Hurt / Death × 4 dirs)

- [ ] Adventurer (sword; bow attack variant or overlay)
- [ ] Fighter, Mage, Marksman, Rogue (post class-change)
- [ ] Portraits: Adventurer + 4 classes + Aurel + Nyla (gacha / char select)

### NPCs ( ≥6 bodies, recycle)

- [ ] Smith, vendor, cook, trainer, quest giver, guard, card broker, auctioneer, tower NPC
- [ ] Homestone object sprite (not hex)

### Enemies (MVP set — not 25 uniques)

- [ ] Slime (exists) + 2 recolors
- [ ] Imp / Wisp (ranged)
- [ ] Brute / Golem
- [ ] Pest / Rat
- [ ] Skeleton
- [ ] Knight / Guard
- [ ] Boss A (humanoid Warden)
- [ ] Boss B (Apex / Colossus, larger)

Everything else: recolor + scale. Combat Lab may stay slime.

### World / tiles

- [ ] Town: floor, paths, building props (stall, fountain, wall)
- [ ] Field Ridge: grass, rock, gate
- [ ] Marsh: mud, reeds
- [ ] Crypt / Ruins / Tower: 2 dungeon palettes
- [ ] Portal sprite (idle + active)
- [ ] Loading-screen art × 4 biomes

### UI art

- [ ] Skill icons: 8 Adventurer + ~15 class skills
- [ ] Item icons: every entry in `items.json` + `weapons.json` + `spirits.json` (~25)
- [ ] Class-card art × 4
- [ ] Banner key art + pity frame
- [ ] HUD: HP/MP, hotbar slots, target frame, XP bar, gold (**no minimap**)
- [ ] Inventory / equip chrome, rarity colors
- [ ] Login / 8-slot select / create chrome
- [ ] Font (UI + damage numbers)
- [ ] Cursor

### VFX (beyond primitives)

- [ ] AA swing, shot projectile, shockwave ring, dash trail, hook line, mend glow, decoy shield
- [ ] Hit spark, crit, heal float, death poof
- [ ] Level-up, teleport, gacha reveal, portal ripple
- [ ] Boss telegraph disc (geometry exists — art pass)

### Audio (MVP = SFX; music is P3)

- [ ] UI click, loot, level-up, portal, pull
- [ ] Combat: swing, bow, hit, hurt, death, skill × 8
- [ ] Optional town / field / tower loops — **P3**, not blocking

---

## P3 — Content depth (data, little code)

- [ ] Tower floors: layouts / props, not only blocked tiles + 3 mobs
- [ ] Shop stock differs in town 2 / 3 (today almost only rations)
- [ ] One banner is enough; pool must be meaningful (see P0-01)
- [ ] Elements feel in play (Water→Fire→Wind→Earth + Light/Shadow) — data has `element`; needs P0-02
- [ ] Skill tomes vs skill points: one rule (trainer **or** tome)
- [ ] Flavor / item descriptions (fields almost absent)
- [ ] Client README + in-game hotkey help = Adventurer kit

---

## P4 — Launch hygiene

- [ ] Archive legacy `gAAAcha/client/Unity/Assets/`; document Stubs → `gatcha1` sync in README
- [ ] Wire run clip or drop the enum
- [ ] Idle frames all 4 directions
- [ ] Guest default; login without `DATABASE_URL` is expected
- [ ] Port 7777 `EADDRINUSE` note; keep `npm test` green
- [ ] Smoke: Create → quest → kill → pull → L20 card

---

## Out of MVP (do not build from this list)

Fairy / pets, mounts, enhance / shells, warehouse, mail, player shops, real guilds, persistent auction, raids, housing, PvP, events, Redis shards, anti-cheat, store, mobile, Plan-B auto-battle grid, combat-rules step 7 (transform gauge).

**After MVP (reference, not a rewrite):** 14-step `DamagePipeline` in [`damage-pipeline-ai-prompt.md`](../damage-pipeline-ai-prompt.md). Grow `gAAAcha/server/src/combat/damage.ts` (named steps, penetration, DoT through the same fn). Do not make Unity the damage authority. Locked order stays `combat-rules.md` (element before crit, K=50).

---

## Build order (5–15 h/week)

1. P0-01 Gacha identity + starter bag
2. P0-02 Damage resolver
3. P0-03 Death/respawn + P0-06 tutorial tips
4. P0-04 Skill unlocks + P0-05 class-change moment
5. P2 player / NPC / 3 enemy sheets + skill/item icons
6. P1 gear ladder + quest chain + loading screen
7. P2 remaining art / VFX / SFX
8. P3 tower content + P4 hygiene
