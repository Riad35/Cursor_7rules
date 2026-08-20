# System & Tech

## Stack
- Framework: Unity 6.3 LTS (6000.3.x) — einzige Client-Engine
- Language: C# (Client, mit Unity 6.3 gebündelt); TypeScript 5.x (Backend)
- Backend: Node.js 24 LTS (Source of Truth — kein Unity Dedicated Server / Netcode for GameObjects)
- Game channel: WebSocket (TCP) Unity ↔ Node
- Meta/API: REST für Login, Gacha-Historie, Katalog
- Database: PostgreSQL 17.x (dynamisch: Charaktere, Inventar-Slots, Pity, Quest)
- Static data: JSON (items, monsters, skills, maps) — zur Server-Startzeit geladen
- Cache/Session/Pity: Redis 7.x
- CDN: Cloudflare
- Analytics: aus im MVP (Privacy)
- Auth: Gast-Login zuerst, später OAuth
- Payment: keines im MVP (Passion-Project, kein Store)
- Hosting: lokal, bis ein Deploy ansteht

## Architecture
Der Unity-Client rendert, nimmt Input entgegen und darf Bewegung lokal vorhersagen, besitzt aber keine spielentscheidende Logik. Jede Aktion (Laufen, Skill, Gacha, Fortschritt) geht als `request_*` über WebSocket (Kampf/Overworld) oder REST (Meta) an einen monolithischen Node.js/TypeScript-Service. Der Server validiert (Speed, Range, Cooldown, Mana, Rate-Limit), persistiert in PostgreSQL, cached Hot-State in Redis und broadcastet `sync_*` an den Client. Maps sind diskrete IDs mit Ladescreen; Instanzen (Party-Kopien) nutzen später dieselbe Map-Pipeline, ohne sie im MVP zu bauen.

## Decisions (append-only, newest top)
| Date | Decision | Rationale |
|------|----------|-----------|
| 2026-08-20 | Shot/skillshot catch radius matches client sprite scale (~2.2×, bosses larger). Predicted HP never reaches 0. Server reaps any monster left at 0 HP. | Overlay 0 HP blocked further shots; bolts flew through the visible body; DoT leftovers never despawned |
| 2026-08-20 | P1-08: map change is a 1.2s OnGUI overlay (fade, map name, biome floor tile). No extra scene. | Discrete maps already; vision asked for a load screen, not seamless world |
| 2026-08-20 | P1-07: bosses wind up 700–900ms with a red ground disc, then hit. Phase 2/3 at 66%/33% HP for all four named bosses (not only `monsterType: boss`). HUD is a segmented top bar. Unique look is scale + tint + body sheet (orc/plant/slime), not new art. | Trash and bosses were the same blob; tower wardens never phased |
| 2026-08-20 | P1-06: main quest chain is gated with `requiresQuestId` (Mira/Kael → pests → crypt+marsh → F2 → Apex → L20 card talk). F3/F4 and stew stay side. Tracker is HUD + J log. | Quests were all available at once; need a readable path to class change |
| 2026-08-20 | P1-05: stew is 60s ATK+DEF `attr_up` (replaces itself, does not stack). R uses stew then ration then bread. Homestone sends `sync_fx`. Held weapon is a pixel silhouette, not a shape mark. | Food was heal-only; yellow diamond was the 64px placeholder scaled by the 2.2 player sprite |
| 2026-08-20 | P1-04: gear ladder is Leather L1 / Iron L8 / Ash L15 on the five armor slots. Equip is server-gated (`levelReq`). Starter bag stays sword+bow+ration+dust. | Need a climb through L20 without dumping plate into the starter bag |
| 2026-08-20 | P1-03: dual-wield is HUD + sprite mark (no bow body sheet). N swaps slots; Shot greys without Sekundär. Guests seed `bow_hunter` in slot 2. | NosTale Haupt/Sekundär must be readable; P2 owns the bow attack sheet |
| 2026-08-20 | 14-step Unity `DamagePipeline` (`damage-pipeline-ai-prompt.md`) is **after MVP**. Live formula stays `combat-rules.md` + `damage.ts`. | MMO needs one server owner; prompt order (crit before element, K=100) would retune live numbers |
| 2026-08-20 | Combat chase uses last-good *or* predicted range; do not predict CD until `sync_cooldowns`; do not move-lock on every self `sync_move`. Parse `skillIds` / `classSkillIds` as JSON arrays only. | Catalog scrape + 250ms move-lock + predicted CD after OOR broke attack → walk-in → skill and made hits look like 0 HP |
| 2026-08-20 | Ridge spawn `monster_slime_2` replaced by King Slime (`monster_king_slime_1`, type `king_slime`) | User asked for one bigger/stronger slime on the field |
| 2026-08-20 | P1-02: skill HUD is presentation-only (local cast bar + predicted CD). MP/weapon-slot numbers come from `sync_skills.catalog`. | Need readable Adventurer kit without a second UI framework |
| 2026-08-20 | Field slime aggro 4 / leash 8 (match lab). Monster move slides around blocked tiles instead of stopping or oscillating on walls. | 1.8 aggro was melee-only so pull felt broken; ridge rock at (10,10) trapped straight-line chase/leash |
| 2026-08-20 | Adventurer hotbar is the 8 class skills (Space + 1–7). Number keys no longer cast other-class skills. Directional skillshots use swept collision. Orc/Plant Craftpix packs are live bodies on Ridge. | Key 1 was Slash (locked → no damage); Shot ticks could skip the hitbox; user added two enemy packs |
| 2026-08-20 | P0-07: Star Dust is pull currency (10 / 90 for 10-pull); Banner Tickets are a gold-shop alternate (1 ticket = 1 pull). Per-monster loot tables; XP `28 + 14×level` (3192 to L20); tower XP >> field. | Close the farm → pull → shop loop; L20 reachable in a tower session chain |
| 2026-08-20 | P0-01–06 shipped: banner = cards/spirits/gear/portraits; damage resolver step 3; death→homestone respawn; Adventurer unlock curve; class-change panel; tutorial tips. Banner is primary class-card path; Card Broker (500g) is backup. | Close the product loop before P1/P2 art |
| 2026-08-20 | MVP remaining work lives in `08-mvp-todo.md`; banner pulls class cards / spirits / gear; Aurel+Nyla are skins/portraits not extra classes; starter bag = sword+bow+ration+dust | Gray-box systems are ahead of the product loop; dead `char_*` items and full starter inventory made gacha/shops meaningless |
| 2026-08-20 | MMO tagged file logs (INFO default; F9 dump / F10 verbosity; no full packets/chat) | Diagnose recurring combat/GFX bugs from files; privacy |
| 2026-08-19 | Mild tilt ortho + yaw (Z/C, RMB); front idle; angled on move | User chose option 2; Q/E kept for skills |
| 2026-08-19 | Inventory size 144 (12×12); pad legacy 20-slot saves | User request; DB check migrated |
| 2026-08-19 | Login→server→8 slots gate; create always Adventurer; class cards for specialists | User build order; cards carry resist + secondary weapon seeds |
| 2026-08-19 | Tower shared maps `tower_f*` + `tower_boss_*` instances; gate via `towerClearedFloor` + switches | Avoid dungeon→dungeon portal hide; progression server-validated |
| 2026-08-19 | Class id migrate wanderer→adventurer, warrior→fighter, archer→marksman | Old saves still boot |
| 2026-08-19 | Trade/friends/party HUD/guild/settings before commissioned art | User: assets later, everything else first |
| 2026-08-19 | Any `dungeon_*` portal opens party-shared instance | Soft-code beyond crypt-only |
| 2026-08-18 | Postgres optional via DATABASE_URL; dual-write file+DB; register/login over WS | File fallback keeps CI/dev without DB; schema applied on boot |
| 2026-08-18 | Crypt portals create private instances (`mapId#inst`); party shares one | Package Phase 5 without multi-process yet |
| 2026-08-18 | Procedural SpriteCatalog silhouettes + biome tiles | Replace graybox before commissioned art |
| 2026-08-18 | Playable hub: town+field portals, shops, thin quests, Homestone, neutrals (graybox) | First end-to-end MMO loop before art/Postgres |
| 2026-08-18 | Party max 4 + default guild Ashen Legion (in-memory) | Social slice before Postgres; guild stub is enough for chat |
| 2026-08-18 | Postgres deferred until user has Docker/local install | No blocker for gray-box; file/guest persist stays |
| 2026-08-15 | Roadmap A→B→C: Feel then Combat then World | Solo timebox; Grid-Snap soft + edge camera first |
| 2026-08-15 | Grid-Snap soft (client lerp) not continuous walk | Keeps server tile validation; better feel |
| 2026-08-15 | Edge camera (safe zone ~72%) not hard follow | Less motion sickness; Nostale-like framing |
| 2026-08-15 | Name `gAAAcha`, Setting original 2.5D-Fantasy | Repo-Name ist der Projektname; keine lizenzierte Nostale-Welt |
| 2026-08-15 | PC zuerst, Mobile später | Gray-Box und Stubs sind Tastatur; Unity-Build bleibt Standalone |
| 2026-08-15 | Kein Store/Payment im MVP | Passion-Project; Payment-Architektur nicht bauen |
| 2026-08-15 | Privacy: nur Gast-Token + Spielstand | Keine Analytics/OAuth bis ein Release-Grund existiert |
| 2026-08-15 | Ordnerstruktur aus 02-system bestätigt | Ziel-Layout; Gray-Box darf flach in `server/src` bleiben bis zum ersten Split |
| 2026-08-15 | Pity: Soft ab 50, Hard 80, Basis-SSR 2%, 10er mind. 1 SR | Fair und testbar; Counter ist sichtbar in `sync_gacha` |
| 2026-08-15 | Gacha jetzt über WebSocket `request_gacha` | Passt zur Gray-Box; REST-Historie kommt mit Auth/Postgres |
| 2026-08-15 | Schema in `server/db/schema.sql`, Runtime in-memory | Vertrag steht; kein Postgres-Zwang solange Unity fehlt |
| 2026-08-15 | Spielrepo `gAAAcha` getrennt von Rules-Repo `Cursor_7rules` | Memory-Bank/.cursor bleiben Template; Spielcode nicht in dasselbe origin mischen |
| 2026-08-15 | Unity 6.3 LTS only — Godot aus Nostale-Paket verworfen | Einzige Client-Engine; Godot/ENet/GDScript und Photon als Netz-Ersatz vom Tisch |
| 2026-08-15 | Nostale-Kampf (lock-on) als Plan A; Auto-Battle-Grid als Plan B | Gewünschte Kampfidentität; Grid und Collector-Tiefe später optional |
| 2026-08-15 | WebSocket (TCP) statt Custom-UDP | Passt zu Nostales TCP-only und langsamem 2D-Kampf; einfacher zu debuggen |
| 2026-08-15 | Diskrete Maps + Ladescreen, kein Seamless-Open-World | Vermeidet härtestes Netcode-Problem; Map-IDs sind MMO-ready |
| 2026-08-15 | MMO-ready Monolith (kein Multi-Server jetzt) | Schema/Events/Map-IDs vorbereiten; Party/Instanz/Chat/Trade später |
| 2026-08-15 | Node-Monolith bleibt Source of Truth | Kein Unity Dedicated Server; Betriebsaufwand für Solo gering |
| 2026-08-15 | PostgreSQL statt NoSQL (z. B. MongoDB) | Gacha-Historie und Käufe brauchen transaktionale Integrität und Audit-Fähigkeit |

## Patterns
- `request_*` / `sync_*`: Client sendet nur Inputs; Server broadcastet autoritativen State
- Client prediction + server snap: lokal loslaufen, bei Divergenz auf Serverposition ziehen
- Server-authoritative: Gacha-RNG, Pity, Combat, Inventar nur im Backend
- Static JSON vs DB: Katalog/Skills/Maps als Dateien; Spielerzustand in PostgreSQL
- Modular monolith: `/src/net`, `/combat`, `/maps`, `/gacha` — InstanceManager später im gleichen `/maps`

## Folder structure (confirmed)
**Unity-Projekt (Client):**
```
/Assets
  /Scripts
    /Overworld
    /Combat
    /Gacha
    /Characters
    /Network
    /UI
  /Art
    /Characters
    /UI
    /Environment
  /Prefabs
  /Scenes
  /Resources
```

**Backend (separates Repo empfohlen):**
```
/src
  /api
  /net
  /combat
  /maps
  /gacha
  /characters
  /db
  /cache
  /auth
/data
  /items.json
  /monsters.json
  /skills.json
  /maps.json
  /banners.json
  /classes.json
/db
  /schema.sql
/tests
```
