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
- Analytics: Firebase
- Auth: Gast-Login zuerst, später OAuth
- Payment: Apple/Google Store-APIs — erst relevant, sobald Monetarisierung geklärt ist (siehe 01-project.md, offen)
- Hosting: [noch nicht festgelegt]

## Architecture
Der Unity-Client rendert, nimmt Input entgegen und darf Bewegung lokal vorhersagen, besitzt aber keine spielentscheidende Logik. Jede Aktion (Laufen, Skill, Gacha, Fortschritt) geht als `request_*` über WebSocket (Kampf/Overworld) oder REST (Meta) an einen monolithischen Node.js/TypeScript-Service. Der Server validiert (Speed, Range, Cooldown, Mana, Rate-Limit), persistiert in PostgreSQL, cached Hot-State in Redis und broadcastet `sync_*` an den Client. Maps sind diskrete IDs mit Ladescreen; Instanzen (Party-Kopien) nutzen später dieselbe Map-Pipeline, ohne sie im MVP zu bauen.

## Decisions (append-only, newest top)
| Date | Decision | Rationale |
|------|----------|-----------|
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

## Folder structure (proposal — not confirmed)
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
/tests
```
