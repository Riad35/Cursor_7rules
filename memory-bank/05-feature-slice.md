# Feature Slice — Nostale → unser Spiel



Quelle: `Nostale_RPG_Dev_Package.md`. Engine: Unity 6.3 LTS only. Backend: Node.js bleibt Wahrheit.



## Take (MVP — live in Gray-Box)



| Feature | Wie bei uns |

|---------|-------------|

| Server-authoritative | Client sendet nur Inputs |

| WebSocket | Move, Cast, Gacha, Equip, Hello, sync_cooldowns/status/inventory/projectiles |

| Maps | `graybox_01` 36×20 + Blocked-Tiles |

| Hit volume | `hitRadius` per entity (edge-to-edge range) |

| Movement | Hold WASD / left virtual joystick; ticks × moveSpeed; body collision vs enemies/players |

| Skill UI | Placeholder skill bar (all class skills) + CD fills + buff timers |

| Inventory | 20 slots seeded with all items; **B** grid; click equip weapon/spirit |

| Projectiles | Ranged UNIT_TARGET / ranged AA travel with `projectileSpeed`; melee instant |

| Targeting | NO_TARGET / UNIT_TARGET (lock-on Tab / RMB); skillshots Later |

| Combat | Space AA; skills; weapons+spirit; buffs/shields; shove lock |

| Camera | Edge ~72% safe zone; soft lerp |

| Gacha | starter banner, file-save guest |



## Later



| Feature | Vorbereitung |

|---------|--------------|

| Postgres | Schema exists; file persist now |

| MOBA skillshots / ground AoE | See `targeting-system-feature.md` |

| Echte seitliche Sprites | Platzhalter squares |

| Plan-B Auto-Battle-Grid | zurückgestellt |



## Skip



| Feature | Grund |

|---------|-------|

| Godot / Photon / Unity Dedicated Auth | Node SoT |

| Full MMO | longterm |
