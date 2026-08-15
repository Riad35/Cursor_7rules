# Active Context

## Current Focus
Unity-Projekt steht unter `gAAAcha/client/Unity`. Server läuft, `NetworkBootstrap` verbindet beim Play automatisch. Nächster Schritt: in Unity **Play** drücken und Console prüfen — dann erste sichtbare Gray-Box (Sprites, Kamera, Lock-on).

## Recent Changes (last 3 sessions max)
- 2026-08-15: Unity-Projekt per CLI angelegt, Stubs + `NetworkBootstrap` eingehängt, Server neu gestartet, `test:client` grün
- 2026-08-15: P0s festgezogen — Name gAAAcha, PC zuerst, kein Payment, Gast-only Privacy, Ordnerstruktur bestätigt
- 2026-08-15: PostgreSQL-Schema + Pity-Gacha + Unit-Tests (12/12) ohne Unity

## Blockers
- Unity muss vom User geöffnet werden (Editor war beim CLI-Compile gesperrt — normal wenn Hub offen ist)
- Plan-A-Kampf braucht seitliche Sprites, Skill-VFX, kleine Overworld-Tiles
- Postgres ist nur Schema; Live-Persistenz kommt nach dem ersten Unity-Haken

## Decisions Pending
- keine
