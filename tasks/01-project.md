# 01-project.md — Projekt-Brief

> Ausgefüllt anhand von SUMMARY.md / STATE.md (bisherige Chat-Klärungen), nicht per Interview. Felder ohne bisherige Aussage sind explizit als OFFEN markiert — bitte bestätigen/ergänzen, dann gilt die Datei als freigegeben.

## 1. Projektname & Vision
**Name:** ⚠️ OFFEN — noch nicht festgelegt.
**Vision (Entwurf, bitte bestätigen):** Ein solo-entwickeltes, faires 2D-Gacha-RPG mit Auto-Battle-Grid-Kämpfen, serverseitig garantiertem Pity-System und selbst produzierter, KI-gestützter Charakterkunst.

## 2. Primärer Nutzer & Problem
⚠️ **Noch nicht explizit besprochen — folgender Entwurf ist eine Ableitung aus der bisherigen Fair-Gacha-Ausrichtung, kein bestätigtes Nutzerprofil:**
- Vermuteter primärer Nutzer: Spieler:innen von Gacha-/Collector-RPGs, die transparente Drop-Raten und ein spürbares Pity-System schätzen, aber keine Lust auf aggressive Monetarisierung haben.
- Vermutetes Problem: Viele etablierte Gacha-Titel wirken für Einzelspieler intransparent oder zeitintensiv; ein kleineres, faires Spiel mit kurzen Sessions könnte diese Lücke füllen.
→ Das ist eine Annahme aus der bisherigen "Fair-ish Gacha"-Ausrichtung in SUMMARY.md, **keine von dir bestätigte Zielgruppendefinition**. Bitte im nächsten Schritt bestätigen oder korrigieren.

## 3. Die 3 Kernfeatures (MVP-Scope)
Direkt aus Projektplan A (Vorabversion, SUMMARY.md Abschnitt 3) abgeleitet:
1. **Auto-Battle-Grid-Kampfsystem** — reduzierter Animations-/Code-Aufwand gegenüber Echtzeit-Action, passend zu Solo-Entwicklung
2. **Server-autoritatives Gacha mit sichtbarem Pity-Counter** (1 Banner zum Start) — Kernversprechen "fair"
3. **Charakter-Progression** (Level-System, 4–6 Charaktere) — genug Tiefe für einen spielbaren Vertical Slice, ohne Ausrüstungs-/Dupe-System in Phase 1

## 4. Harte Rahmenbedingungen (Constraints)
| Constraint | Status | Wert |
|---|---|---|
| Budget | abgeleitet | Solo-Projekt, kein externes Funding erwähnt → kostenlose/günstige Tools bevorzugt (Unity Personal, Firebase Free Tier, o. Ä.) |
| Offline-Fähigkeit | abgeleitet | **Nein** — Gacha-Fairness erfordert serverseitige RNG, also zwingend Online-Verbindung nötig |
| Privacy | ⚠️ OFFEN | noch nicht besprochen (z. B. DSGVO-Relevanz bei EU-Nutzern, Datenspeicherung) |
| Plattform | ⚠️ OFFEN | noch nicht beantwortet (Mobile / PC / beides) — **blockiert u. a. Unity-Build-Settings und Input-System-Wahl in 02-system.md** |
| Zeitbudget | bestätigt | 5–15 Std./Woche (Teilzeit) |
| Team | bestätigt | Solo, unterstützt durch Cursor Pro |

## Freigabe-Status
🟡 **Entwurf — noch nicht freigegeben.** Offene Punkte: Projektname, Nutzerprofil-Bestätigung, Privacy-Constraint, Plattform.
