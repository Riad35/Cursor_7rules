# Feature Spec: MOBA Targeting System (Wild Rift–style)

## 1. Overview

A touch-first targeting system supporting movement, auto-attacks, and multiple ability-targeting modes (no-target, unit-target, skillshot, ground-target AoE), with both smart-cast and indicator-cast input flows. Designed for mobile (touch) but should degrade cleanly to mouse/keyboard.

**Goals**
- Sub-100ms responsiveness from touch input to visual feedback (indicator shown/target locked)
- Deterministic, server-authoritative targeting resolution (client shows prediction, server validates)
- Extensible per-ability targeting config so designers can add new abilities without new code

**Non-goals**
- Full netcode/rollback design (referenced but not specified here)
- Camera system design (assumed fixed-angle isometric camera already exists)

---

## 2. Input Layer

### 2.1 Movement joystick
- Virtual joystick spawns at touch-down origin (not fixed position) within a designated screen zone
- Outputs a normalized 2D vector `(dx, dy)` clamped to joystick radius
- Vector converted to world-space movement direction via camera-relative transform (accounts for isometric rotation)
- Continuous polling each frame while touch is held; movement stops on touch-up

### 2.2 Ability input buttons
- Each ability slot (Q/W/E/R equivalent + summoner/flash + basic attack) is a touch region
- On press, input handling branches based on the ability's **targeting type** (see Section 3)
- Supports two interaction patterns:
  - **Tap** — single touch-down + touch-up in place (used for no-target, unit-target, or smart-cast skillshots)
  - **Press-and-drag-and-release** — touch-down starts aiming, drag updates aim direction/position, touch-up confirms (used for indicator-cast skillshots and ground-target AoE)
- Minimum drag distance threshold before "aiming" state activates, to distinguish accidental taps from intentional drags

### 2.3 Input state machine
```
IDLE → (button press) → determine targeting type
  ├─ NO_TARGET        → fire immediately → IDLE
  ├─ UNIT_TARGET       → await tap-on-target → validate → fire → IDLE
  ├─ SKILLSHOT_SMART   → fire toward joystick/facing direction → IDLE
  ├─ SKILLSHOT_INDICATOR → AIMING → (drag updates rotation) → release → fire → IDLE
  └─ GROUND_AOE        → AIMING → (drag updates reticle position) → release → fire → IDLE
```
- Global cancel condition: if ability goes on cooldown, silenced, or stunned mid-aim, force-cancel back to IDLE and hide indicator

---

## 3. Targeting Modes

Each ability has a static config object defining its targeting behavior:

```json
{
  "abilityId": "champion_q",
  "targetingType": "SKILLSHOT_LINEAR",
  "range": 800,
  "width": 90,
  "castTime": 0.25,
  "projectileSpeed": 1600,
  "smartCastDefault": false,
  "requiresLineOfSight": true,
  "affectsTerrain": false
}
```

### 3.1 No-target
- Fires on press with no additional input
- Used for self-buffs, passives, stat-boost actives
- Validation: ability off cooldown, resource cost available, not silenced

### 3.2 Unit-target (point-and-click)
- Requires selecting a valid enemy (or ally, for heals) unit
- Resolution: raycast/screen-to-world pick against unit hitboxes at touch point, or nearest valid unit within a small tap-tolerance radius if tap misses exact hitbox
- If a unit is already "locked" (see Section 4), unit-target abilities default to the locked target unless the player taps a different valid unit
- Validation: target within range, target type matches ability's valid-target filter (enemy/ally/self), line-of-sight check if required

### 3.3 Skillshot — linear
- Press-and-hold shows a rectangular indicator (length = range, width = ability width) anchored at champion position
- Indicator rotates continuously to follow drag vector angle
- On release: locks direction, spawns projectile (or instant hitscan) along that vector
- Clamp indicator length to max range even if drag exceeds it

### 3.4 Skillshot — circular/point-blank
- Indicator is a circle of fixed radius, follows the touch/drag position but clamped to max cast range from champion
- Fires at the confirmed point; area-of-effect resolves at impact location, not along a travel path

### 3.5 Skillshot — cone
- Indicator is a cone/fan shape, angle fixed by ability config, rotates with drag direction like linear skillshots
- Used for wide, short-range abilities

### 3.6 Ground-target AoE
- Reticle (circle, typically) follows touch point directly (not champion-anchored rotation), clamped to max range from champion position
- On release, ability resolves at reticle's final position after any cast-time delay
- Some AoEs are "delayed impact" (e.g., a telegraphed strike) — reticle stays visible during a wind-up period before the effect triggers

### 3.7 Smart-cast vs. indicator-cast (settings toggle, per-ability)
- **Smart-cast**: tap fires immediately using current joystick-held direction (or last movement direction if stationary) as the aim vector; no visible held-indicator step
- **Indicator-cast**: requires the full press-drag-release flow described above
- This is a player preference stored per-ability (some players smart-cast skillshots but indicator-cast ults)

---

## 4. Auto-Attack Targeting

### 4.1 Candidate gathering
- Every frame (or on a throttled tick, e.g. 10Hz, for performance), query all enemy units within `attackRange` using spatial partitioning (grid or quadtree) rather than brute-force distance checks against every unit on the map

### 4.2 Target priority resolution
```
function getAutoAttackTarget(champion):
    candidates = spatialQuery(champion.position, champion.attackRange, filter=ENEMY)
    if candidates.empty: return null

    if champion.manualTarget exists and isValid(champion.manualTarget, candidates):
        return champion.manualTarget   # explicit player tap overrides default priority

    sort candidates by:
        1. isAttackingMe (recent aggro, decays after N seconds)
        2. unitTypePriority (see 4.3)
        3. lowestCurrentHP
        4. closestDistance
    return candidates[0]
```

### 4.3 Unit type priority rules
- Towers/turrets: never auto-target enemy champions over minions, UNLESS that champion has damaged an allied champion within the last ~X seconds (aggro trigger), or the champion is within a minimum "tower dive" proximity threshold
- Champions: default priority is champions > monsters/jungle camps > minions when in mixed range, unless minions are actively lower HP and no aggro is active (varies by exact game balance rules — flag as a tunable, not hardcoded)
- Locked target persists across frames until: target dies, target leaves attack range, target becomes untargetable (stealth/invulnerable), or player explicitly retargets

### 4.4 Manual retarget
- Tapping a specific enemy sets `manualTarget` and overrides auto-priority for subsequent auto-attacks until that target is invalid or player taps elsewhere
- Tapping empty ground clears `manualTarget` and reverts to auto-priority

---

## 5. Core Systems / Building Blocks

### 5.1 Screen-to-world raycasting
- Convert touch screen coordinates → world-space ray via camera projection matrix
- Intersect against ground plane (for movement/ground-target) or unit hitbox colliders (for unit-target)
- Tap-tolerance: expand effective hitbox radius slightly beyond visual model bounds to account for imprecise touch input

### 5.2 Spatial partitioning
- Grid-based (fixed cell size, e.g. matching average attack range) or quadtree
- Rebuilt or updated incrementally each tick as units move
- Used by: auto-attack candidate queries, skillshot collision checks, AoE impact resolution

### 5.3 Aim indicator rendering
- Separate visual layer per active aiming state (line, cone, circle mesh/sprite)
- Rotates/repositions per-frame based on live drag input
- Clipped/scaled to the ability's max range
- Color-coded or dashed when target is out of range or blocked by terrain (line-of-sight fail state)

### 5.4 Skillshot travel & collision
- For projectile-based skillshots: spawn projectile at cast time, move along locked vector at `projectileSpeed`, check collision against enemy hitboxes each tick until max range reached or hit registered
- For hitscan/instant skillshots: resolve collision immediately along the vector at cast confirmation

### 5.5 Prediction (optional, stretch feature)
- For skillshots vs. moving targets, client-side aim assist can extrapolate target position using `targetPosition + targetVelocity * projectileTravelTime`
- Purely a client-side convenience for auto-aim assist features; must not be trusted server-side

### 5.6 Line-of-sight checks
- Raycast against terrain/fog-of-war blockers between caster and target/impact point
- Required for abilities flagged `requiresLineOfSight: true`
- Should reuse the same occlusion data used for vision/fog-of-war system if one exists

---

## 6. Server Authority & Validation

- Client sends: ability ID, cast timestamp, target data (unit ID for unit-target, or vector/point for skillshot/AoE)
- Server re-validates on receipt:
  - Cooldown state
  - Resource cost (mana/energy)
  - Crowd-control state (silenced, stunned, feared preventing cast)
  - Range check using server-authoritative positions (anti-cheat: reject if client-reported target is outside true range + tolerance)
  - Line-of-sight re-check server-side
- On validation failure: reject silently or send correction; client should already have shown optimistic local feedback (input latency hiding), then reconcile if server rejects

---

## 7. Edge Cases & Special Rules

- **Target death mid-cast**: for unit-target abilities with cast time, if target dies before cast completes, ability either fizzles (mana refunded) or redirects, per ability config
- **Stealth/invisibility**: stealthed units excluded from auto-attack candidate pool and from unit-target tap resolution unless caster has true sight
- **Untargetable states** (e.g., dashes, spell shields): excluded from candidate pool during that window
- **Self-targeting abilities cast while moving**: should not interrupt movement input
- **Simultaneous multi-touch**: movement joystick and ability aiming must be handled on independent touch IDs so moving while aiming a skillshot works
- **Out-of-range confirm**: if player releases a skillshot/AoE drag beyond max range, clamp to max range rather than canceling the cast
- **Ability cast during channel/root**: some abilities lock the champion in place during cast — movement input should be ignored/queued during this window, not silently dropped

---

## 8. Data Structures (reference)

```ts
interface AbilityTargetingConfig {
  abilityId: string;
  targetingType: "NO_TARGET" | "UNIT_TARGET" | "SKILLSHOT_LINEAR" 
               | "SKILLSHOT_CIRCLE" | "SKILLSHOT_CONE" | "GROUND_AOE";
  range: number;
  width?: number;       // for linear/cone skillshots
  radius?: number;      // for circle skillshots/AoE
  coneAngle?: number;   // for cone skillshots
  castTime: number;
  projectileSpeed?: number;   // omit for instant/hitscan
  smartCastDefault: boolean;
  requiresLineOfSight: boolean;
  validTargetFilter?: "ENEMY" | "ALLY" | "SELF" | "ANY";
}

interface AutoAttackState {
  manualTarget: EntityId | null;
  lastAttackedByTimestamp: Map<EntityId, number>; // aggro tracking
  attackRange: number;
  attackCooldown: number;
}
```

---

## 9. Open Questions / Tunables

- Exact aggro-decay timing for tower/minion priority (needs balance pass)
- Tap-tolerance radius value (needs playtesting on various device screen sizes/DPI)
- Whether prediction/aim-assist is in scope for v1 or deferred
- Auto-attack query tick rate (full per-frame vs. throttled) — performance vs. responsiveness tradeoff
