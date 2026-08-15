# Nostale-Inspired 2D Co-op RPG — Complete Dev Package
## For Cursor AI Context + Solo Dev Roadmap

---

# PART 1: ASSETS YOU NEED

## A. Visual Assets (2D / 2.5D)

### 1. Character Sprites
- **4-directional or 8-directional walk cycles** (up, down, left, right + diagonals)
- **Combat animations**: attack (melee/ranged), cast spell, hit reaction, death
- **Idle animations**: standing breathing loop
- **Emotes**: wave, sit, dance, etc.
- **Equipment layers**: separate sprite sheets for weapons, armor, helmets, wings, pets
- **Resolution**: 32x32, 48x48, or 64x64 per frame (NosTale uses ~48x48-ish anime sprites)
- **Format**: PNG sprite sheets with transparent backgrounds

### 2. Monster / NPC Sprites
- Same animation categories as characters
- Bosses need larger sprite sheets (128x128 or bigger)
- NPCs need idle + talk animations

### 3. Tilesets (Environment)
- **Ground tiles**: grass, dirt, stone, water, snow, etc.
- **Autotiles**: transitions between terrain types
- **Cliff/wall tiles**: for elevation and collision
- **Decorative objects**: trees, rocks, houses, fences
- **Animated tiles**: water, torches, portals
- **Format**: 16x16, 32x32, or 64x64 tiles in grid-aligned sprite sheets

### 4. UI Elements
- **HUD**: health/mana bars, minimap frame, hotbar, chat window
- **Menus**: inventory grid, character stats, skill tree, party list, settings
- **Buttons**: normal, hover, pressed, disabled states
- **Fonts**: pixel-art or clean anime-style TTF/OTF (with license)
- **Cursors**: default, attack, talk, loot, pointer
- **Icons**: items, skills, buffs/debuffs, status effects

### 5. Effects / VFX
- **Spell effects**: projectile sprites, explosion frames, buff auras
- **Hit effects**: slash marks, blood/damage numbers, block sparks
- **Environmental**: weather (rain, snow), fog, lighting
- **UI effects**: level-up flash, loot drop beam, portal swirl

### 6. Maps / Level Design Data
- **Tiled map files** (.tmx) or custom format
- **Collision layers**: walkable vs blocked grid
- **Spawn points**: monster spawn zones, player start, NPC positions
- **Portal/transition zones**: map-to-map teleporters
- **Instance/raid maps**: separate map files for dungeons

## B. Audio Assets

### 1. Music
- **Ambient tracks**: per biome (forest, desert, dungeon, town)
- **Combat music**: boss battle, regular fight
- **Menu/title screen music**
- **Format**: OGG or MP3, loop seamlessly

### 2. Sound Effects (SFX)
- **UI**: click, hover, error, success, level-up
- **Combat**: sword swing, arrow shot, fireball cast, hit, block, death
- **Movement**: footsteps per terrain, jump, teleport
- **Environment**: portal activate, chest open, item pickup
- **Format**: WAV or OGG, short clips

## C. Data Assets (Code-Driven)

### 1. Static Game Data (JSON/ScriptableObjects)
```
/items.json        — ID, name, type, stats, icon, drop rate
/monsters.json     — ID, name, HP, damage, AI type, loot table, sprite
/skills.json       — ID, name, cooldown, damage, mana cost, VFX reference
/maps.json         — ID, name, tileset, spawn tables, portal destinations
/quests.json       — ID, name, objectives, rewards, dialogue
/classes.json      — base stats, skill tree, starting equipment
```

### 2. Localization Files
- All text strings externalized for translation

## D. Code Architecture Assets

### 1. Game Engine
- **Godot 4.x** (recommended for 2D, built-in multiplayer, free, exports to web/mobile)
- OR **Unity** (better asset store, heavier, Netcode for GameObjects)

### 2. Networking Stack
- **Godot**: Built-in MultiplayerAPI (ENet or WebSocket)
- **Unity**: Netcode for GameObjects OR Mirror OR FishNet
- **Alternative**: Photon PUN (managed service, easier, costs $ at scale)

### 3. Database (for persistence)
- **SQLite** (local/dev) → **PostgreSQL** (production)
- OR **Firebase** / **Supabase** (managed, auth + DB)

### 4. Backend Server Language
- **Option A**: Godot headless server (same code as client!)
- **Option B**: Node.js + Socket.io (if web-first)
- **Option C**: C# dedicated server (if Unity)

---

# PART 2: DETAILED ROADMAP

## PHASE 0: Foundation (Weeks 1–2)
**Goal**: One character walking on a map, no multiplayer yet.

- [ ] Set up Godot 4.x project (2D renderer)
- [ ] Import a placeholder tileset (OpenGameArt has free ones)
- [ ] Create a simple 50x50 test map in Godot TileMap
- [ ] Add a player character with 4-directional walk animation
- [ ] Implement basic movement (WASD / joystick) with collision
- [ ] Add camera follow
- [ ] Placeholder UI: HP bar, MP bar

**Deliverable**: You can walk around a map and bump into walls.

---

## PHASE 1: Core Combat (Weeks 3–5)
**Goal**: Hit things, things hit back, die, respawn.

- [ ] Add a monster with idle + walk + attack + death animations
- [ ] Basic AI: wander, aggro when player is near, chase, attack
- [ ] Player attack: click-to-attack or auto-attack in range
- [ ] Damage calculation: ATK - DEF = damage
- [ ] HP/MP regeneration over time
- [ ] Death: fade out, respawn at start point after 3 seconds
- [ ] Loot table system: monster dies → roll for drops → spawn item on ground
- [ ] Pick up items → add to inventory array

**Deliverable**: You can fight monsters, get loot, die, respawn.

---

## PHASE 2: Inventory & Equipment (Weeks 6–7)
**Goal**: Full RPG item loop.

- [ ] Inventory grid UI (drag-and-drop or click-to-equip)
- [ ] Equipment slots: weapon, armor, helmet, boots, gloves, accessory
- [ ] Item stats modify player stats (equip sword → ATK +10)
- [ ] Consumables: potions that restore HP/MP
- [ ] Item rarity system (common → rare → epic → legendary)
- [ ] Tooltips showing item stats on hover

**Deliverable**: Full loot-and-equip loop feels satisfying.

---

## PHASE 3: Skills & Classes (Weeks 8–9)
**Goal**: Abilities beyond auto-attack.

- [ ] Skill bar UI (1–0 hotkeys)
- [ ] Skill system: cooldown, mana cost, cast time, range
- [ ] 3 basic skills: melee slash, ranged shot, heal
- [ ] Skill VFX: instantiate animated sprite on cast
- [ ] Class selection at start (Warrior / Mage / Archer)
- [ ] Base stats differ per class
- [ ] Skill tree UI (future-proofing)

**Deliverable**: Combat has depth, not just clicking.

---

## PHASE 4: LOCAL MULTIPLAYER TEST (Weeks 10–12)
**Goal**: Two clients connect, see each other, fight together.

- [ ] Set up Godot MultiplayerAPI with ENet (UDP-based)
- [ ] Dedicated server scene (headless Godot build)
- [ ] Player spawn: server assigns spawn point, tells client
- [ ] State sync: server sends player positions 20x/sec to all clients in map
- [ ] Client interpolation: smooth movement between received positions
- [ ] Combat sync: player clicks attack → sends to server → server validates → broadcasts to all clients → play animation
- [ ] Monster sync: server owns all monsters, clients display them
- [ ] Loot ownership: first damage gets loot priority for 10 seconds
- [ ] Chat system: simple global chat

**Deliverable**: Two game windows open, you see yourself and a friend (or second client).

---

## PHASE 5: Instance / Dungeon System (Weeks 13–15)
**Goal**: Private dungeon instances for 1–4 players.

- [ ] "Instance Manager" server node that creates temporary map copies
- [ ] Party system: invite → accept → share instance
- [ ] When party enters portal → server spawns new instance map → teleports party
- [ ] Instance has its own monster spawns, boss, loot chests
- [ ] Instance cleanup: when empty for 5 minutes, delete from memory
- [ ] Boss mechanics: phase changes at HP thresholds, AoE attacks, minion spawns
- [ ] Instance timer: 30-minute limit

**Deliverable**: You and friends can enter a dungeon, fight a boss, get loot, leave.

---

## PHASE 6: Persistence & Progression (Weeks 16–18)
**Goal**: Progress saves between sessions.

- [ ] SQLite database schema: players, characters, inventory, skills, quest progress
- [ ] Login system (username/password, local only for now)
- [ ] On logout: serialize character → save to DB
- [ ] On login: load from DB → spawn character
- [ ] Quest system: talk to NPC → kill 10 boars → return → reward
- [ ] Experience/leveling: XP bar, level-up stat gains
- [ ] Death penalty: lose 5% XP or durability (configurable)

**Deliverable**: Close game, reopen, your character is where you left off.

---

## PHASE 7: Polish & First Public Test (Weeks 19–24)
**Goal**: Feels like a real game.

- [ ] Replace all placeholders with original or licensed assets
- [ ] Add 3+ biomes with unique monsters
- [ ] Add 5+ dungeon instances with different themes
- [ ] Party UI: HP bars, buffs, class icons
- [ ] Trading system: secure trade window
- [ ] Friends list
- [ ] Settings: volume, graphics, keybinds
- [ ] Web export (Godot HTML5) or mobile export
- [ ] Deploy test server on cheap VPS (Hetzner ~$5/month)

**Deliverable**: Friends can register, play, and give feedback.

---

## PHASE 8: MMO-Scale (Months 6–12+)
**Goal**: More players, more maps, live ops.

- [ ] Multiple map servers (one process per map or zone)
- [ ] Login server + World server architecture
- [ ] Load balancing: spawn new map instances when crowded
- [ ] Guild system
- [ ] Auction house / player economy
- [ ] Events: double XP weekends, holiday dungeons
- [ ] Anti-cheat: server validation, rate limiting, basic checksums
- [ ] Analytics: track player retention, popular dungeons

**Deliverable**: A living game that grows.

---

# PART 3: IMPORTANT NOTES — HOW NOSTALE SOLVED NETCODE
## Feed this entire section into Cursor as context

## NOSTALE NETWORKING ARCHITECTURE — REFERENCE FOR OUR GAME

### Overview
NosTale is a 2D isometric anime MMORPG released in 2006 by Entwell (Korea) and published by GameForge. It uses a TCP-only, packet-based client-server architecture with map-based sharding. Understanding how they solved (or sidestepped) hard netcode problems informs our design decisions.

---

### 1. TRANSPORT PROTOCOL: TCP-ONLY

**What NosTale Did:**
- Uses TCP for ALL game communication. No custom UDP protocol.
- TCP guarantees packet order and delivery. The server and client exchange discrete "packets" (custom binary protocol, not HTTP).
- Each packet has a header (packet type/ID) and a payload (comma-separated or binary data).

**Why This Worked for Them:**
- 2D isometric combat is slower-paced than FPS. A 100ms delay in position update is less noticeable than in a twitch shooter.
- TCP head-of-line blocking (where one lost packet stalls everything behind it) is acceptable because movement is not physics-based platforming.
- Much simpler to implement and debug than a custom UDP reliability layer.

**What We Should Do:**
- Use Godot's built-in MultiplayerAPI with ENet (which is UDP-based but with reliability channels) OR WebSocket (TCP).
- For our co-op RPG with 1–8 players per instance, TCP or reliable UDP is perfectly fine.
- Do NOT build a custom UDP stack unless we have 50+ concurrent players in one view.

---

### 2. PACKET / EVENT SYSTEM

**What NosTale Did:**
- Client and server communicate via structured packets.
- Example packet flow (from reverse-engineered private servers):
  - Client sends: `walk [x] [y]` → Server validates → Server broadcasts to all nearby players: `mv [playerID] [x] [y]`
  - Client sends: `u_s [skillID] [targetID]` → Server checks cooldown/mana → calculates damage → broadcasts: `su [casterID] [targetID] [damage] [skillID]`
- Private server emulators (OpenNos, SaltyEmu) abstracted these into an Event Pipeline:
  - Packet arrives → Deserialized into Event → EventChecker validates → EventHandler executes → Response packets generated

**What We Should Do:**
- Define all game actions as RPCs (Remote Procedure Calls) in Godot:
  - `rpc_id(1, "request_move", target_x, target_y)` — client asks server
  - Server validates, then: `rpc("sync_move", player_id, x, y)` — server tells all clients
- Use Godot's `@rpc` annotations to control authority:
  - Client can only call "request_" functions on server
  - Server owns "sync_" and "spawn_" functions

---

### 3. STATE SYNCHRONIZATION STRATEGY

**What NosTale Did:**
- Server is the **single source of truth** for everything: player positions, monster HP, item drops, quest states.
- Client sends INPUTS ("I want to move to X,Y", "I want to use skill Z on target T").
- Server validates the input, updates the world state, then tells clients what changed.
- Position updates are sent periodically (likely 10–20 times per second) to all players on the same map.
- Because it's 2D grid/tile-based movement, the server can validate walks by checking the path against collision data.

**What We Should Do:**
- Implement the same **Server-Authoritative** model.
- Client NEVER sends "my position is X,Y" — only "I want to walk to X,Y".
- Server calculates the path, updates the server-side position, and broadcasts the new position.
- For smoother feel, client can PREDICT the movement locally (start walking immediately) but be ready to snap to server's authoritative position if they diverge.

---

### 4. MAP SHARDING & INTEREST MANAGEMENT

**What NosTale Did:**
- The world is divided into discrete maps (e.g., "NosVille", "Desert", "Cave Floor 1").
- Each map runs as a separate server process or thread. Players on different maps are on different servers.
- Within a map, the server only sends updates about entities "near" the player. This is crude interest management based on map zones.
- Portals between maps trigger a server transition: save state, disconnect from Map A server, connect to Map B server, load state.

**What We Should Do:**
- Start with ONE server handling ONE map. Don't over-engineer.
- When ready for multiple maps, use Godot's multi-server approach or run separate headless Godot instances per map, with a "World Server" routing connections.
- For interest management in Godot:
  - Use `MultiplayerAPI.object_configuration_add` with visibility filters
  - Only sync nodes within a certain distance of each player

---

### 5. INSTANCE / DUNGEON SYSTEM

**What NosTale Did:**
- Raids and instances are separate map copies created on-demand.
- When a party enters a raid portal, the game creates a new instance of that map, copies the party into it, and runs a separate simulation.
- Instances have timers, unique monster spawns, and boss mechanics.
- When the instance is complete (boss dead or timer expires), players are teleported out and the instance is destroyed.

**What We Should Do:**
- Build an "InstanceManager" as a singleton in our server.
- When a party enters a dungeon portal:
  1. InstanceManager.create_instance(dungeon_template_id, party_member_ids)
  2. Load dungeon scene (TileMap + spawners)
  3. Teleport party members to instance spawn points
  4. Run instance logic (spawn waves, boss phases)
  5. On completion/timeout: award loot, teleport to exit, queue instance for deletion
- Use Godot's `add_child()` to dynamically load instance scenes. Each instance is a separate branch in the scene tree.

---

### 6. COMBAT & SKILL SYNCHRONIZATION

**What NosTale Did:**
- Combat is largely "lock-on" and ability-based, not free-aim twitch.
- Player selects target → presses skill → client sends request → server checks range/mana/cooldown → calculates damage using stats → applies to target → broadcasts result.
- Damage numbers and hit effects are client-side VFX triggered by the server's "damage dealt" packet.
- Boss mechanics are scripted server-side: at 75% HP, spawn adds; at 50%, cast AoE; at 25%, enrage.

**What We Should Do:**
- Same lock-on or proximity-based combat. Avoid free-aim projectile physics (hard to sync).
- Skill flow:
  1. Client: `rpc_id(1, "cast_skill", skill_id, target_id)`
  2. Server: Check cooldown (server tracks timers), check mana, check range
  3. Server: Calculate damage = (ATK + skill_damage) - target_DEF
  4. Server: Apply damage, check death, roll loot
  5. Server: `rpc("on_skill_cast", caster_id, target_id, damage, skill_id)` to all observers
  6. Clients: Play animation, show damage number, spawn VFX
- Boss AI runs entirely on server. Clients just display what the server tells them.

---

### 7. INVENTORY & ITEM SECURITY

**What NosTale Did:**
- Inventory is server-authoritative. The client shows a UI representation, but the server holds the actual item arrays.
- Item moves (equip, unequip, drop, trade) are all server-validated.
- Private server emulators found exploits where client-authoritative checks allowed item duplication—this proves why server authority is critical.

**What We Should Do:**
- Server stores inventory as arrays of item IDs.
- Client sends requests: "move item from slot 5 to slot 2" or "equip item in slot 3".
- Server validates: Is the item in slot 5? Is slot 2 empty? Is the item equippable by this class?
- Server updates its state, then tells the client: `rpc("inventory_updated", new_inventory_state)`.
- NEVER trust client inventory state for gameplay logic.

---

### 8. DATABASE & PERSISTENCE

**What NosTale Did:**
- Static data (items, monsters, skills, maps) is stored in `.dat` files loaded at server startup. This never changes at runtime.
- Dynamic data (player characters, inventory, quest progress) is stored in a database (likely MySQL or similar).
- The server keeps active player data in RAM for speed. On logout or periodic auto-save, it writes to the database.

**What We Should Do:**
- Use JSON files for static data (items, monsters, skills). Load into dictionaries at startup.
- Use SQLite for development (single file, zero setup).
- Schema:
  ```sql
  CREATE TABLE players (id INTEGER PRIMARY KEY, username TEXT, password_hash TEXT, created_at TIMESTAMP);
  CREATE TABLE characters (id INTEGER PRIMARY KEY, player_id INTEGER, name TEXT, class TEXT, level INTEGER, xp INTEGER, map_id TEXT, x REAL, y REAL);
  CREATE TABLE inventory (character_id INTEGER, slot INTEGER, item_id INTEGER, quantity INTEGER, PRIMARY KEY (character_id, slot));
  CREATE TABLE character_skills (character_id INTEGER, skill_id INTEGER, level INTEGER);
  ```
- Save on: level up, item change, logout, and every 5 minutes as auto-save.

---

### 9. ANTI-CHEAT BASICS

**What NosTale Did:**
- Server validates all movement against collision maps.
- Server validates all combat against stats, ranges, and cooldowns.
- Packet encryption (lightweight XOR or similar) to prevent trivial packet sniffing.
- Rate limiting on actions to prevent spam.
- Despite this, private servers and bots exist—perfect anti-cheat is impossible, but server authority stops 90% of exploits.

**What We Should Do:**
- Server validates:
  - Walk speed (max tiles/sec based on stats)
  - Attack range (melee = 1 tile, bow = 5 tiles, etc.)
  - Skill cooldowns (server tracks timestamps)
  - Mana costs (server deducts, not client)
- Rate limit: max 10 actions/sec per player.
- Don't encrypt packets initially—add XOR obfuscation later if needed.

---

### 10. WHY NOSTALE'S APPROACH MAKES IT FEASIBLE FOR US

NosTale succeeded because it **sidestepped** the hardest netcode problems through design choices:

| Hard Problem | How NosTale Avoided It |
|-------------|----------------------|
| Twitch reflex combat | Used lock-on, ability-based combat with cast times |
| Physics simulation | 2D tile-based movement, no rigidbody physics |
| Large-scale battles | Maps cap at ~50–100 players; instances are 5–15 players |
| Complex prediction | TCP + simple reconciliation; no client-side prediction needed |
| Open world seamless loading | Discrete maps with loading screens between them |
| Real-time economy | Trade windows + NPC shops, not a fully simulated market |

**Our game should copy these choices exactly.**

---

# PART 4: CURSOR PROMPT TEMPLATE

When starting a new feature in Cursor, paste this context + your request:

```
CONTEXT:
We are building a 2D isometric co-op RPG inspired by NosTale using Godot 4.x.
The game uses SERVER-AUTHORITATIVE networking.
Client sends INPUTS only. Server owns all game state.
We use Godot's MultiplayerAPI with ENet.
Maps are discrete (not seamless open world).
Combat is lock-on ability-based (not twitch/free-aim).
Movement is 2D tile-based or grid-snapped.
Instances/dungeons are separate scenes spawned on-demand.

CURRENT TASK:
[Describe what you want to build, e.g.:
"Implement the server-side skill system. When a client sends 'cast_skill' RPC,
the server should check cooldowns, mana, range, calculate damage, apply it,
and broadcast the result to all players in the same instance."]

CONSTRAINTS:
- Use GDScript
- Server must validate everything
- Client should predict animation start but accept server correction
- Include error handling (not enough mana, target out of range, skill on cooldown)
```

---

# PART 5: FIRST LOCAL TEST CHECKLIST

## Your "Solo Player" Test (No friends needed yet)

1. **Start the server**: Run Godot headless server build
   ```bash
   godot --headless --server
   ```

2. **Start the client**: Run normal Godot client
   - Enter any username/password (local auth for now)
   - Client connects to `127.0.0.1:7777`

3. **Verify**: Check these one by one
   - [ ] Client spawns player at correct position
   - [ ] Server logs show "Player connected"
   - [ ] Walk around — server receives move requests, validates, broadcasts
   - [ ] Open second client window — can you see yourself twice? (Test with two clients)
   - [ ] Spawn a monster — does it appear in both clients?
   - [ ] Attack monster — does HP decrease on both clients? Does death sync?
   - [ ] Loot drops — can you pick it up? Does inventory sync?
   - [ ] Enter a portal — does it load a new map/instance?

4. **Stress test**: Spawn 20 monsters. Do all clients stay in sync?

---

# QUICK REFERENCE: GODOT MULTIPLAYER RPC SETUP

```gdscript
# In your player script
extends CharacterBody2D

@rpc("any_peer", "call_remote")
func request_move(target_pos: Vector2):
    # Only the server should process this
    if not multiplayer.is_server():
        return
    # Validate...
    var can_move = validate_movement(target_pos)
    if can_move:
        sync_move.rpc(multiplayer.get_remote_sender_id(), target_pos)

@rpc("authority", "call_local")
func sync_move(player_id: int, pos: Vector2):
    # All clients (and server) execute this
    var player = get_player_node(player_id)
    player.move_to(pos)
```

---

# FINAL NOTES

- **Start small.** A single map, one class, three skills, one monster type.
- **Make it fun before you make it multiplayer.** Single-player combat feel is 80% of the work.
- **Multiplayer is additive.** Get single-player solid first, then add networking.
- **Use placeholders.** A gray box that moves correctly is worth more than a beautiful sprite that desyncs.
- **Read the private server source.** OpenNos and SaltyEmu on GitHub are goldmines for understanding how NosTale's protocol works. You don't need to copy their code, but their packet handlers show you the exact flow.
- **Join the Godot Discord.** The multiplayer channel is active and helpful.

Good luck. Build the gray box first. Make it move. Make it hit. Then make it online.
