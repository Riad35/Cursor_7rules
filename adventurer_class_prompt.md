# Adventurer Base Class — Locked Design (L1–20)

Server-authoritative. Source of truth: `gAAAcha/server/data/{classes,skills,weapons,items}.json` (+ combat.ts).  
Unity ScriptableObjects are **not** authority (client presentation only).

## Decisions (locked)

1. **Kit size:** exactly **8 skills including AA** (not 8+AA).
2. **Band:** level 1–20. Starter kit is AA + Shot; the rest unlocks with SP at L3 / 5 / 8 / 11 / 14 / 17.
3. **Class change gate (explained):** a **class card** only works at **level ≥ 20**.  
   Before 20 you stay Adventurer. That is the rule “everyone uses Adventurer until class change.”
4. **Weapons (NosTale-style):** **Hauptwaffe** (slot 1 / primary) + **Sekundärwaffe** (slot 2 / secondary).  
   Adventurer: Slot1 = melee sword, Slot2 = ranged bow. Skills declare `weaponSlot: 1|2` for damage/range.
5. **Items:** follow MVP from `nostale-item-taxonomie.md` (see below) — not the full NosTale catalog.

---

## Kit (8)

| # | Id | Role | Weapon slot | Notes |
|---|-----|------|-------------|--------|
| 1 | `auto_attack` | Melee AA | 1 | Space / Space |
| 2 | `shot` | Ranged attack | 2 | Skillshot; uses Sekundärwaffe |
| 3 | `shockwave` | AoE burst | 1 | Ground circle |
| 4 | `dash` | Mobility | — | Escape / reposition; short dash |
| 5 | `rally` | Self buff | — | ATK up (Block/Dodge later when avoid rolls exist) |
| 6 | `hook_shot` | Pull **or** near push | 1 | Far = pull; adjacent = AoE shove |
| 7 | `mend` | Self heal + MP | — | Self only for MVP |
| 8 | `decoy` | Damage gate | — | Next hit ×0.2 damage (80% DR), then consume |

Hotkeys (client): Space AA · 1 Shot · remaining skills appear on the bar as they unlock (trainer / SP).

---

## NosTale weapon slots (how it maps)

| NosTale | gAAAcha field | Adventurer default |
|---------|---------------|--------------------|
| Hauptwaffe | `equippedWeaponId` | `sword_iron` |
| Sekundärwaffe | `equippedWeapon2Id` | `bow_hunter` |
| Weapon swap | `swapWeapons` | N / hotkey |

Damage uses the skill’s `weaponSlot` bonus (atk/magicAtk + element/range from that weapon). Secondary also gives a small passive atk/resist while sheathed (already partially in `applyGearStats`).

---

## Item taxonomy MVP (from nostale-item-taxonomie.md)

Implement first (matches taxonomy MVP):

| Taxonomy | In gAAAcha now / next |
|----------|------------------------|
| **A** Waffe Haupt + Sekundär | Dual weapon slots + sword/bow starter |
| **A** Rüstung / Helm / Stiefel / Handschuhe / Accessoire | Already thin armor slots |
| **D** Consumables (heal/buff/teleport) | Rations, stew, homestone |
| **E** Verstärkung / Seltenheit | Later (enhance + shells deferred) |
| **F** Stage-Zugang | Tower / instance portals (thin) |
| **G** Boxen / Gacha | Banner pulls |

Defer for post–L20 / post-MVP: Fairy, pet gear, costumes/wings, full shells, housing, mounts, raid seals.

Generic item fields to keep aligning toward: `id`, rarity, kind, stack, levelReq, classBind, tradeable, sellPrice, questBound.

---

## Out of scope (this pass)

- Full secondary stats dodge/block as avoid rolls  
- Ally-target mend / party rally  
- SP / timed transform cards (taxonomy B)
