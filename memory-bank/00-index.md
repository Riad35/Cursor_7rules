# gAAAcha — Project Index & Remaining Plan

See also [`08-mvp-todo.md`](08-mvp-todo.md) (**canonical remaining work**) · [`06-nostale-gap.md`](06-nostale-gap.md) (systems backlog) · [`07-logging.md`](07-logging.md) (MMO log contract).

**Repos:** `gatcha` (memory-bank / Cursor workspace) · **Code:** sibling `gAAAcha`  
**Stack:** Unity 6.3 client (`client/Unity/gatcha1`) + Node/TS WebSocket server (`server`, port **7777**)  
**Vision:** Fair 2.5D gacha RPG — small overworld, lock-on combat, server pity (see `01-project.md`)

---

## 1. Repo map

| Location | Role |
|----------|------|
| `gatcha/memory-bank/` | Vision, system, active focus, progress, feature slices |
| `gAAAcha/server/` | Authoritative game logic, JSON data, optional Postgres |
| `gAAAcha/server/src/` | Modules + `*.test.ts` |
| `gAAAcha/server/data/` | Maps, classes, skills, monsters, items, banners, shops, quests… |
| `gAAAcha/client/Stubs/` | Canonical C# sources (edit here, sync to Unity) |
| `gAAAcha/client/Unity/gatcha1/` | **Active** Unity project |
| `gAAAcha/client/Unity/Assets/` | Legacy duplicate — ignore for new work |

**Ops:** `cd gAAAcha/server` → `npm run dev` · Unity Play · sync Stubs → `gatcha1/Assets/_Project/Scripts/` (same relative folder: Core/Network/World/UI)

**Logs:** server `.runtime/logs/server.log` · client `persistentDataPath/gAAAcha/logs/game.log` · F9 dump · F10 verbosity

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
| Combat types + session store (step 2) | Live | `combat/types.ts`, `combat/sessionStore.ts` |
| Tagged MMO logs (files + ring dump) | Live | `log.ts`, `GameLog.cs` |
| Projectiles, statuses, threat | Live | `world.ts`, `threat.ts` |
| Weapons + spirit + gear slots | Live | equip APIs |
| Secondary weapon + `N` swap | Live | `request_weapon_swap` |
| Aim indicators (shot, stun, etc.) | Live | client HUD |

### D. Progress & economy
| Feature | Status | Where |
|---------|--------|--------|
| XP / levels / SP | Live | `xp.ts` — 28+14×level, 3192 to L20 |
| Skill unlock tree | Live | `skills.ts` |
| Gacha 1/10 + pity UI | Live | `gacha.ts` — 10/90 dust or tickets |
| Inventory **144 (12×12)** + equip/use | Live | gacha + client panel |
| Shops buy/sell | Live | `shop.ts` + tickets / gear sinks |
| Kill loot tables | Live | `loot.json` / `loot.ts` |
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
| `shop` / `quest` / `xp` / `skills` / `loot` | Hub + progression + drop tables |
| `party` / `chat` / `trade` / `friends` / `auction` | Social / market |
| `db` / `persist` | Postgres + file fallback |
| `data.ts` | JSON catalogs |
| `log.ts` | Tagged file + console logger |

**Client stubs:** `NetClient` · `InputSender` · `NetworkBootstrap` · `GrayBoxWorld` · `SpriteCatalog` · `VirtualJoystick` · `JsonUtil` · `GameLog` · `PredictionReconciler`

---

## 4. What’s left

**Canonical list:** [`08-mvp-todo.md`](08-mvp-todo.md) (P0 loop/combat/gacha → P2 assets → P4 hygiene). Full MMO systems that are *not* MVP stay in [`06-nostale-gap.md`](06-nostale-gap.md). Plan B auto-battle grid: never MVP (`01-project.md`).

---

## 5. Quality bar

| Check | State |
|-------|--------|
| Server `npm test` | **93/93** |
| Client automated tests | None |
| Known ops issues | `EADDRINUSE` on 7777; login needs `DATABASE_URL` |

---

## 6. Suggested next slice

Default: **P1-03 Dual weapon visible** (sword vs bow, N-swap feedback). P1-02 skill HUD is in. See [`08-mvp-todo.md`](08-mvp-todo.md).
