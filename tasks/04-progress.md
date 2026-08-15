# 04-progress.md — Fortschritt

## Done
- Systemanalyse der 4 Referenzspiele (Arknights/ZZZ/SxS/NTE) abgeschlossen (SUMMARY.md)
- Team-Modell geklärt: Solo + Cursor Pro
- Art-Strategie geklärt: eigene KI-Bildgenerierungs-Pipeline (im Aufbau)
- Zeitbudget geklärt: 5–15 Std./Woche
- Programmiererfahrung geklärt: solide + Unity-Vorerfahrung
- Engine-Entscheidung finalisiert: Unity 6.3 LTS
- Backend-Stack-Vorschlag erstellt: Node.js 24 LTS + TypeScript, PostgreSQL, Redis
- Genre-Richtung festgelegt: Arknights-Fairness + Sword-x-Staff-Kampf-/Gacha-Tiefe (Auto-Battle-Grid)
- Projektplan A (Vorabversion/MVP) und Projektplan B (Vollversion) grob ausformuliert

## Open
- [P0] Zielplattform festlegen (blockiert Unity-Build-Settings)
- [P0] Monetarisierungs-Absicht festlegen (blockiert Payment-Architektur-Entscheidung)
- [P0] Projektname & Setting/Thema festlegen
- [P1] Ordnerstruktur (02-system.md Abschnitt 3) bestätigen
- [P1] Unity-6.3-LTS-Projekt-Grundgerüst lokal anlegen (in Cursor/Unity Editor, nicht in diesem Chat möglich)
- [P1] Node.js/TypeScript-Backend-Grundgerüst mit PostgreSQL-Schema-Entwurf
- [P2] Server-seitige Gacha-Pity-Logik implementieren (Kernversprechen "fair")
- [P2] Auto-Battle-Grid-Kampfprototyp (1 Testcharakter, 2 Teststages)

## Known Bugs
*(noch keine — Implementierung hat noch nicht begonnen)*

## Questions
- Siehe "Decisions Pending" in 03-active.md — identisch, um Redundanz zwischen den Dateien gering zu halten wird hier nur verwiesen statt dupliziert.

---

## Hinweis zu "ACT mode" aus deinem Fragenkatalog-Template
Dein eingefügtes Template endete mit der Anweisung, direkt in den "ACT mode" zu wechseln und den ersten Open-Task ("Set up Next.js 15 project") auszuführen. Das war erkennbar ein generisches Beispiel aus einer anderen Vorlage (Next.js/Kanban-Board) und passt nicht zu unserem tatsächlichen Stack (Unity/Node.js). Zwei Gründe, warum hier noch nicht "ausgeführt" wurde:
1. **Drei P0-Entscheidungen sind noch offen** (Plattform, Monetarisierung, Projektname) — die Vorlage selbst sieht vor, bei offenen Blockern nicht einfach loszulegen.
2. **Unity-Projekt-Setup braucht den Unity-Editor lokal** — das kann ich in dieser Chat-Umgebung nicht ausführen (kein Unity installiert, keine GUI). Das Node.js/TypeScript-Backend-Grundgerüst dagegen könnte ich hier tatsächlich als Dateien vorbereiten, sobald die P0-Punkte geklärt sind.

Sobald die drei offenen Fragen beantwortet sind, kann ich entweder (a) das Backend-Grundgerüst hier als fertige Dateien erstellen, die du in Cursor weiterbearbeitest, oder (b) eine Schritt-für-Schritt-Anleitung für das Unity-Projekt-Setup liefern, die du selbst in Unity/Cursor ausführst.
