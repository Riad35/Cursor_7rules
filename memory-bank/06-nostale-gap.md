# Nostale checklist vs gAAAcha (gap map)

Source: [`Nostale_Complete_Feature_Checklist.md`](../Nostale_Complete_Feature_Checklist.md)  
Code: sibling `gAAAcha` · Index: [`00-index.md`](00-index.md)

**Legend:** Done ≈ shipped in gray-box · Partial · Missing · Skip (out of passion MVP / IP)

---

## Tier summary

| Nostale tier | gAAAcha |
|--------------|---------|
| **T0 Core loop** | Mostly **Done** (create, move, combat, map, inv, equip, level, Adventurer, HP/MP UI) — no Job Level |
| **T1 RPG foundation** | **Partial** — classes via cards (not full SP), skills, quests, instances; no fairy/pet/enhance |
| **T2 Multiplayer/social** | **Partial** — WS multiplayer, party, chat, trade, friends; guild stub; no player shops |
| **T3 Endgame** | **Missing** — raids, full SP, housing, crafting, PvP, events |
| **T4 Polish/monetization** | **Skip** — NosMall, full audio, anti-cheat, mobile |

---

## By checklist section

| # | Area | Status | Notes |
|---|------|--------|-------|
| 1 | Account & character | Partial | Guest + optional register/login; 8 slots; no email recovery, gender/hair, tutorial, Hero/Job levels, titles |
| 2 | Classes | Partial | Adventurer + Fighter/Mage/Marksman/Rogue via **class cards** (not Nostale class-change quest / Martial Artist) |
| 3 | SP cards | Missing | Closest: class cards + secondary weapon — **not** SP transform/G-window/SP points |
| 4 | Combat | Partial | Lock-on, AA, skills, floats, death anim, elements stub; no full elemental matrix, durability death penalty |
| 5 | Skills | Partial | Hotbar skills, CD/MP, AoE/cone; no SP cost, cast channel UI polish, drag skill bar |
| 6 | Equipment | Partial | Weapon/spirit/armor slots; no +enhance, shells, runes, rarity crafting depth |
| 7 | Inventory & storage | Partial | 144 bag + equip pane; no warehouse, costume inventory |
| 8 | Pets / partners | Missing | |
| 9 | Mounts | Missing | |
| 10 | Quests | Partial | Accept/progress/turn-in thin quests; no full main story acts |
| 11 | Time-Space / instances | Partial | Private dungeon instances + timer; not full TS score UI |
| 12 | Raids | Missing | |
| 13 | Housing / Mini-Land | Missing | |
| 14 | Family / guild | Partial | Default “Ashen Legion” stub; create/invite limited |
| 15 | PvP | Missing | |
| 16 | Crafting & economy | Partial | Shops + auction in-memory; no craft, player shops, exchange |
| 17 | Social | Partial | Chat/party/trade/friends; no mail, block list polish |
| 18 | Map & world | Partial | Discrete maps, portals, tower; not full NosVille→Act world |
| 19 | Monster & AI | Partial | Aggro, threat, loot, respawn; not full AI packages |
| 20 | NosMall | Skip | Passion-project — no cash shop |
| 21 | Events | Missing | |
| 22 | UI/UX | Partial | HUD, inv, settings, lock outline; not full Nostale chrome |
| 23 | Server/tech | Partial | Node WS + optional Postgres; no Redis shards, anti-cheat |
| 24 | Martial Artist | Missing | |
| 25 | Act 10+ endgame | Missing | |
| 26 | Sound & music | Missing | |
| 27 | Tutorial | Missing | |

---

## What to build next (aligned to checklist, not clone)

Stay **original fantasy** (no Nostale IP). Use the checklist as a **systems backlog**, not a copy list.

1. **Combat feel (done this pass):** target red outline; damage FX on target; map-correct FX frames  
2. **T0/T1 gaps worth filling:** tutorial tip, Job-or-skill progression clarity, equipment enhance (+1…+5) light  
3. **Avoid for now:** SP card clone, NosMall, raids, housing, PvP  

---

## Done this session (client)

- Ability / hit placeholders use **entity tile + camera-up lift** (not world `+Y` north offset)  
- Damaging skill VFX spawn **on the target**  
- Ground AoE discs lie on the **XY** plane  
- Lock target = **red outline** around the enemy sprite (billboarded)
