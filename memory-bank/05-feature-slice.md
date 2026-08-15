# Feature Slice — Nostale → unser Spiel



Quelle: `Nostale_RPG_Dev_Package.md`. Engine: Unity 6.3 LTS only. Backend: Node.js bleibt Wahrheit.



## Take (MVP — live in Gray-Box)



| Feature | Wie bei uns |

|---------|-------------|

| Server-authoritative | Client sendet nur Inputs |

| WebSocket | Move, Cast, Gacha, Equip, Inspect, Chat, Hello, sync_* |

| Maps | `graybox_01` 36×20 + Blocked-Tiles |

| Targeting | Tab cycle; RMB menu; Space AA; offensive skills need lock |

| AoE | Centers on **selected target** (`aoeRadius`); shockwave UNIT_TARGET |

| Buffs | Self / NO_TARGET; sphere+ring cast VFX |

| Cast indicators | Melee beam, AoE ring on target, buff burst (no aim mode) |

| Projectiles | Ranged AA + bolts: spawn → trail → impact flash |

| Target frame | Portrait, HP/MP, buffs, aggro 0–100%; red/yellow border |

| Inventory | 20 slots; **B** grid |

| Chat | World / Server / Guild / Map; whisper; party/guild stubbed |

| Combat | Weapons+spirit; buffs/shields; shove lock; body collision |



## Later (recommended next)



| Priority | Feature | Why |

|----------|---------|-----|

| **1** | MOBA **indicator-cast** (press-drag aim) | Visual layer exists; next step is real skillshot aim vectors |

| 2 | Real party / guild backends | UI stubs ready |

| 3 | Sprite pass | Replace cubes |

| 4 | Postgres live | File-save works |



## Skip



| Feature | Grund |

|---------|-------|

| Godot / Photon / Unity Dedicated Auth | Node SoT |

| Full MMO | longterm |
