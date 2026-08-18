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
