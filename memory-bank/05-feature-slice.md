# Feature Slice — Nostale → unser Spiel



Quelle: `Nostale_RPG_Dev_Package.md`. Engine: Unity 6.3 LTS only. Backend: Node.js bleibt Wahrheit.



## Take (MVP — live in Gray-Box)



| Feature | Wie bei uns |

|---------|-------------|

| Server-authoritative | Client sendet nur Inputs |

| Targeting | UNIT / NO_TARGET + **indicator-cast** (linear / cone / ground) |

| Indicator skills | `shot`/`stun_bolt` linear; `blind_dust` cone; `shockwave` ground circle |

| Aim UX | Hold skill → aim indicator → release/LMB confirm; Esc cancel |

| AoE | Ground shockwave at aim point; cast VFX rings |

| Projectiles | Homing AA + **directional** skillshots |

| Target frame / chat / threat / inventory | As before |



## Roadmap (locked order)



1. [x] MOBA indicator-cast

2. [ ] **Next: Postgres** persistence

3. [ ] Real party / guild

4. [ ] Sprite pass + misc



## Skip



| Feature | Grund |

|---------|-------|

| Smart-cast toggle / LoS | Later |

| Godot / Photon | Node SoT |
