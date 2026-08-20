# 07-logging.md — MMO log contract

Files are the source of truth. Console is a quiet tagged view. F9 dumps the flight recorder when something looks wrong. Grep the files later; add a regression test when the cause is deterministic.

## Line format

```
2026-08-20T01:18:00.412Z  WARN   GFX      sheet=swordsman_idle  reason=width_not_divisible  tex=256x256  fallback=shape
```

| Field | Rule |
|-------|------|
| Time | ISO-8601 UTC |
| Level | `ERROR` `WARN` `INFO` `DEBUG` `TRACE` |
| Channel | `NET` `COMBAT` `WORLD` `GFX` `UI` `PERSIST` `SOCIAL` `SYS` |
| Body | Readable sentence first; `key=value` context after. Never a full packet at INFO. Do not replace spaces with underscores. |
| Privacy | No chat body, passwords, or full guest tokens |

Default min level: **INFO**. Client **F10** cycles INFO → DEBUG → TRACE. Server: `GAAACHA_LOG_LEVEL`.

## What to log

- **SYS:** boot, bind, join/leave (redacted guest), Postgres vs file fallback
- **NET:** packet **type** at DEBUG; full JSON only at TRACE; every `error` at WARN as a readable sentence (`Could not read packet: … [bad_packet]`), including why (invalid JSON, unknown type)
- **COMBAT:** successful cast (skill, target, dmg, hpAfter, crit); rejects via the error packet (`out_of_range`, `on_cooldown`, …)
- **WORLD:** map change, portal, instance open
- **GFX:** sheet load/slice fail once per path (tex WxH, frame WxH, facing, fallback=shape); empty facing; reconcile snap
- **PERSIST / SOCIAL:** save fail, trade/party errors — not every gold tick

## Do not log

Movement every frame, `sync_state` spam, successful sprite slices, every `recv: {json}`, chat text.

## Files (gitignored, kept on disk)

| Side | Path |
|------|------|
| Server | `gAAAcha/server/.runtime/logs/server.log` (+ `server.1.log` … rotate 5×5MB) |
| Client | `Application.persistentDataPath/gAAAcha/logs/game.log` (same rotation) |
| Dump | `crash-YYYYMMDD-HHMMSS.log` — last ~200 lines on ERROR or client **F9** |

## Bug loop

1. Reproduce (F10=DEBUG if needed).
2. Grep `GFX` / `COMBAT` / `NET`.
3. Fix the cause.
4. Server logic → add/adjust a `*.test.ts`. GFX → slice rule must not WARN on Combat Lab boot.
5. Keep the crash dump; do not commit it.
