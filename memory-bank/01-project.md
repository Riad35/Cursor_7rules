# Project: [OFFEN — noch nicht festgelegt]

## Vision
Ein solo-entwickeltes, faires 2D-Gacha-RPG: kleine 2.5D-Overworld, Charaktere und Gegner in seitlicher Ansicht, Nostale-artiges lock-on Kampfsystem, serverseitig garantiertes Pity, selbst produzierte KI-gestützte Charakterkunst. Engine ist fest Unity 6.3 LTS.

## Core Features
1. **Kleine 2.5D-Overworld**: wenige diskrete Maps, Portale, Ladescreens — kein nahtloses Open World; Charaktere/Gegner seitlich
2. **Nostale-artiges lock-on Kampfsystem**: Skills mit Cooldown, Mana, Range; Server prüft jede Aktion
3. **Server-autoritatives Gacha mit sichtbarem Pity-Counter**: 1 Banner zum Start — Kernversprechen "fair"

## Plan B (mitplanen, nicht bauen)
Auto-Battle-Grid und Collector-Tiefe aus den anderen Referenzen (Arknights-Fairness, Sword-x-Staff-Gacha) bleiben eine optionale spätere Kampfschicht oder ein Fallback — nicht der MVP.

## Target User
- Primary: [OFFEN — Entwurf, nicht bestätigt] Spieler:innen von Gacha-/Collector-RPGs brauchen transparente Drop-Raten und ein spürbares Pity-System, weil viele etablierte Titel intransparent oder zeitintensiv wirken
- Ableitung aus der bisherigen "Fair-ish Gacha"-Ausrichtung, keine bestätigte Zielgruppendefinition

## Constraints
- Engine: Unity 6.3 LTS only — keine zweite Client-Engine (kein Godot)
- Solo-Projekt, kein externes Funding — kostenlose/günstige Tools bevorzugt (Unity Personal, Firebase Free Tier, o. Ä.)
- Offline: Nein — Gacha-Fairness und Combat-Validierung erfordern serverseitige Logik, Online-Verbindung zwingend
- Privacy: [OFFEN] noch nicht besprochen (z. B. DSGVO, Datenspeicherung)
- Plattform: [OFFEN] Mobile / PC / beides — blockiert Unity-Build-Settings und Input-System
- Zeitbudget: 5–15 Std./Woche (Teilzeit)
- Team: Solo, unterstützt durch Cursor Pro
