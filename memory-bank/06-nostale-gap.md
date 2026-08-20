# Systems backlog vs gAAAcha (gap map)

Original-fantasy build. This is a **systems backlog**, not a Nostale clone. Canonical index: [`00-index.md`](00-index.md). Combat contract: `gAAAcha/docs/combat-rules.md`. Skills HUD: `gAAAcha/docs/ATTACKS.md`.

**Legend:** Done ≈ shipped in gray-box · Partial · Missing · Skip (out of passion MVP / IP)

---

## Tier summary

| Tier | gAAAcha |
|------|---------|
| **T0 Core loop** | Mostly **Done** (create, move, combat, map, inv, equip, level, Adventurer, HP/MP UI) — no Job Level |
| **T1 RPG foundation** | **Partial** — classes via cards (not full SP), skills, quests, instances; no fairy/pet/enhance |
| **T2 Multiplayer/social** | **Partial** — WS multiplayer, party, chat, trade, friends; guild stub; no player shops |
| **T3 Endgame** | **Missing** — raids, full SP, housing, crafting, PvP, events |
| **T4 Polish/monetization** | **Skip** — cash shop, full audio, anti-cheat, mobile |

---

## By area

| # | Area | Status | Notes |
|---|------|--------|-------|
| 1 | Account & character | Partial | Guest + optional register/login; 8 slots; no email recovery, gender/hair, tutorial, Hero/Job levels, titles |
| 2 | Classes | Partial | Adventurer + Fighter/Mage/Marksman/Rogue via **class cards** |
| 3 | Transformation / SP | Missing | Closest: class cards + secondary weapon — not timed transform (combat-rules step 7 still spec-only) |
| 4 | Combat | Partial | Lock-on, AA, skills, floats, death anim, elements; step 3 resolver live. 14-step Unity pipeline parked after MVP (`damage-pipeline-ai-prompt.md`) |
| 5 | Skills | Partial | Hotbar skills, CD/MP, AoE/cone; no SP cost, cast channel UI polish |
| 6 | Equipment | Partial | Three-tier gear (Leather/Iron/Ash) + `levelReq`; no +enhance, shells, runes |
| 7 | Inventory & storage | Partial | 144 bag + equip pane; no warehouse, costume inventory |
| 8 | Pets / partners | Missing | |
| 9 | Mounts | Missing | |
| 10 | Quests | Partial | Main chain to L20 class card + side tower/stew; no full voiced story |
| 11 | Instances | Partial | Private dungeon instances + timer |
| 12 | Raids | Missing | |
| 13 | Housing | Missing | |
| 14 | Guild | Partial | Default “Ashen Legion” stub; create/invite limited |
| 15 | PvP | Missing | |
| 16 | Crafting & economy | Partial | Shops + auction + loot tables + dust/ticket pulls; no craft, player shops |
| 17 | Social | Partial | Chat/party/trade/friends; no mail, block list polish |
| 18 | Map & world | Partial | Discrete maps, portals, tower |
| 19 | Monster & AI | Partial | Aggro, threat, loot, respawn |
| 20 | Cash shop | Skip | Passion-project |
| 21 | Events | Missing | |
| 22 | UI/UX | Partial | HUD, inv, settings, lock outline, on-screen GameLog |
| 23 | Server/tech | Partial | Node WS + optional Postgres; tagged file logs; no Redis shards, anti-cheat |
| 24 | Extra classes | Missing | |
| 25 | Far endgame | Missing | |
| 26 | Sound & music | Missing | |
| 27 | Tutorial | Partial | First-session tip strip (P0-06) |

---

## What to build next (not a clone)

Canonical MVP remaining work: [`08-mvp-todo.md`](08-mvp-todo.md). Next: P1-01 status presentation. Avoid: SP-card clone, cash shop, raids, housing, PvP.

## Client presentation already in

- Ability / hit placeholders use **entity tile + camera-up lift** (not world `+Y` north offset)
- Damaging skill VFX spawn **on the target**
- Ground AoE discs lie on the **XY** plane
- Lock target = **red outline** around the enemy sprite (billboarded)
