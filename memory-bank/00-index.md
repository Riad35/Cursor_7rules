# gAAAcha — Project Index & Remaining Plan

See also [`06-nostale-gap.md`](06-nostale-gap.md) (Nostale checklist vs build).

**Repos:** `gatcha` (memory-bank / Cursor workspace) · **Code:** sibling `gAAAcha`  
**Stack:** Unity 6.3 client (`client/Unity/gatcha1`) + Node/TS WebSocket server (`server`, port **7777**)  
**Vision:** Fair 2.5D gacha RPG — small overworld, lock-on combat, server pity (see `01-project.md`)

---

## 1. Repo map

| Location | Role |
|----------|------|
| `gatcha/memory-bank/` | Vision, system, active focus, progress, feature slices |
| `gAAAcha/server/` | Authoritative game logic, JSON data, optional Postgres |
| `gAAAcha/server/src/` | Modules + `*.test.ts` (59 tests) |
| `gAAAcha/server/data/` | Maps, classes, skills, monsters, items, banners, shops, quests… |
| `gAAAcha/client/Stubs/` | Canonical C# sources (edit here, sync to Unity) |
| `gAAAcha/client/Unity/gatcha1/` | **Active** Unity project |
| `gAAAcha/client/Unity/Assets/` | Legacy duplicate — ignore for new work |

**Ops:** `cd gAAAcha/server` → `npm run dev` · Unity Play · sync Stubs → `gatcha1/Assets/Scripts/Network/`

---

## 2. Functionality index (what exists)

### A. Session & characters
| Feature | Status | Where |
|---------|--------|--------|
| Guest hello + token | Live | `net.ts`, `NetClient` |
| Register / login (Postgres) | Live if `DATABASE_URL` | `db.ts` |
| Server list | Live (static) | `chars.ts` |
| 8 character slots create/select/delete | Live | `chars.ts`, gate UI |
| Always create Adventurer; class via cards | Live | class cards + `request_use_class_card` |
| Logout → login gate | Live | Esc settings |

### B. World & travel
| Feature | Status | Where |
|---------|--------|--------|
| Discrete maps + portals | Live | `portal.ts`, map JSON |
| Homestone set / teleport | Live | shop/use item |
| Private dungeon instances | Live | `instance.ts` |
| Tower floors / bosses / switches / gates | Live | tower maps + `towerClearedFloor` |
| Soft tile move + server validation | Live | `combat.ts` / `world.ts` |

### C. Combat
| Feature | Status | Where |
|---------|--------|--------|
| Lock-on + Tab cycle | Live | `GrayBoxWorld` |
| Skills / CD / MP / range / AoE / cone / linear | Live | `combat.ts` |
| Projectiles, statuses, threat | Live | `world.ts`, `threat.ts` |
| Weapons + spirit + gear slots | Live | equip APIs |
| Secondary weapon + `N` swap | Live | `request_weapon_swap` |
| Aim indicators (shot, stun, etc.) | Live | client HUD |

### D. Progress & economy
| Feature | Status | Where |
|---------|--------|--------|
| XP / levels / SP | Live | `xp.ts` |
| Skill unlock tree | Live | `skills.ts` |
| Gacha 1/10 + pity UI | Live | `gacha.ts` |
| Inventory **144 (12×12)** + equip/use | Live | gacha + client panel |
| Shops buy/sell | Live | `shop.ts` |
| Quests accept / progress / turn-in | Live | `quest.ts` |
| Auction (list/buy/sell) | Live in-memory | `auction.ts` |
| Gold sync | Live | sync_gold / state |

### E. Social
| Feature | Status | Where |
|---------|--------|--------|
| Chat channels + whisper | Live | `chat.ts` |
| Party invite/join/leave | Live | `party.ts` |
| Default guild “Ashen Legion” | Stub live | `party.ts` |
| Friends add/remove | Live | `friends.ts` |
| Trade invite / offer / confirm | Live | `trade.ts` |

### F. Presentation (client)
| Feature | Status | Where |
|---------|--------|--------|
| Mild 2.5D tilt + yaw (Z/C, RMB) | Live | `GrayBoxWorld` |
| Move/facing vs **camera basis** | Live | `NetworkBootstrap` |
| Craftpix player/slime sheets + tiles | Live | `SpriteCatalog` + StreamingAssets |
| Idle / walk / attack / hurt / death clips | Live (wired) | `SpriteCatalog` + anim lock |
| Billboard art sprites | Live | `GrayBoxWorld` |
| Draggable inventory + char/equip pane | Live | `NetworkBootstrap` |
| Esc settings: resolution + logout | Live | `NetworkBootstrap` |
| Virtual joystick | Live | `VirtualJoystick` |

### G. Persistence
| Feature | Status | Where |
|---------|--------|--------|
| File guest saves | Live | `.runtime/saves` |
| Postgres dual-write | Live when DB up | `db.ts`, `schema.sql` |
| Inventory pad 20→144 | Live | `padInventory` |

---

## 3. Server module cheat-sheet

| Module | Responsibility |
|--------|----------------|
| `net.ts` | WS route all `request_*` / `cast_skill` |
| `world.ts` | Entities, tick, loot, inspect |
| `combat.ts` | Validate move/cast, damage, projectiles |
| `gacha.ts` | Pulls, pity, inventory size |
| `chars.ts` | Slots + server list |
| `portal` / `instance` | Travel + private maps |
| `shop` / `quest` / `xp` / `skills` | Hub + progression |
| `party` / `chat` / `trade` / `friends` / `auction` | Social / market |
| `db` / `persist` | Postgres + file fallback |
| `data.ts` | JSON catalogs |

**Client stubs:** `NetClient` · `InputSender` · `NetworkBootstrap` · `GrayBoxWorld` · `SpriteCatalog` · `VirtualJoystick` · `JsonUtil`

---

## 4. What’s left (prioritized plan)

### P1 — Polish current slice (high value, small scope)
- [ ] Confirm attack/hurt/death feel on all monster types (not only slime sheets)
- [ ] Idle: ensure all directions have non-empty frames / consistent loop
- [ ] Remove or archive legacy `client/Unity/Assets/` to avoid editing the wrong tree
- [ ] Document sync rule in README one-liner (Stubs → gatcha1)

### P2 — Art & readability
- [ ] Run clip usage when sprinting (enum exists; wire or drop)
- [ ] More biomes / NPC skins / class-specific player sprites
- [ ] Autotiles / proper tilemaps (replace full-texture floors)
- [ ] VFX polish beyond primitives for skills

### P3 — Content depth
- [ ] More tower floors / quests / shop stock without new systems
- [ ] Boss telegraphs + phase clarity in UI
- [ ] Class card acquisition loop more visible in HUD

### P4 — Production / MMO-scale (deferred by design)
- [ ] Real multi-process shards (not static `SERVER_LIST`)
- [ ] Persist auction / party / guild beyond process memory
- [ ] Real guild create/invite (drop default-legion stub)
- [ ] Events calendar, analytics (explicitly out of MVP privacy)
- [ ] Mobile controls pass
- [ ] Store / payments (passion-project: skip)

### Plan B (never MVP)
- Auto-battle grid / Arknights-style layer — keep as optional future, do not build now (`01-project.md`)

---

## 5. Quality bar

| Check | State |
|-------|--------|
| Server `npm test` | **59/59** |
| Client automated tests | None |
| Known ops issues | `EADDRINUSE` on 7777; login needs `DATABASE_URL` |

---

## 6. Suggested next slice (pick one)

1. **Art P2** — NPC + second enemy sheets + run clip  
2. **Content P3** — 2–3 quests + tower floor pack from data only  
3. **Hygiene P1** — delete legacy Unity tree + README sync note  

Default recommendation: **1 or 3** before more systems; the gray-box loop is already feature-complete for an MVP demo.
