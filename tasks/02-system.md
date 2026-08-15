# 02-system.md — Tech-Stack & Architektur

> Basiert auf SUMMARY.md Abschnitt 2 (Tech-Stack-Analyse) + Entscheidungen aus dem Chat-Verlauf. Versionsangaben kurz recherchiert (Stand: 15.08.2026), da Engine-/Runtime-Versionen sich schnell ändern.

## 1. Vollständiger Stack (mit exakten Versionen)

| Layer | Technologie | Version | Begründung |
|---|---|---|---|
| Client / Game-Engine | Unity | **6.3 LTS (6000.3.x)** | Aktuelle empfohlene LTS-Linie (Support bis Dez. 2027); du hast bereits Unity-Erfahrung |
| Sprache Client | C# | (mit Unity 6.3 gebündelt) | — |
| Backend-Runtime | Node.js | **24 LTS** (aktuell aktive LTS-Linie seit Ende 2026) | Stabilste aktuell empfohlene LTS für neue Projekte (Node 22 ist nur noch Maintenance-LTS) |
| Backend-Sprache | TypeScript | **5.x** (aktuelle Stable-Version zum Implementierungszeitpunkt prüfen) | Typsicherheit für Gacha-/Inventar-Logik |
| Datenbank | PostgreSQL | **17.x** (aktuelle stabile Version zum Implementierungszeitpunkt verifizieren) | Transaktionssicherheit für Gacha-Historie/Käufe zwingend nötig |
| Cache/Session/Pity-Counter | Redis | **7.x** (aktuelle stabile Version verifizieren) | Standard für schnellen Zugriff auf Pity-Zähler, Sessions |
| CDN | Cloudflare | — | günstiger Einstieg, DDoS-Schutz inklusive |
| Analytics | Firebase | — | schnellster Start ohne eigene Pipeline |
| Payment (später, Vollversion) | Apple/Google Store-APIs | — | erst relevant, sobald Monetarisierungs-Frage geklärt ist (siehe 01-project.md, offen) |

⚠️ Hinweis: Bei TypeScript/PostgreSQL/Redis wurden hier die zum Zeitpunkt der Recherche aktuell empfohlenen Major-Versionen genannt — die exakte Patch-Version sollte unmittelbar vor dem `npm install`/DB-Setup nochmal verifiziert werden, da sich diese schneller ändern als Unity-/Node-LTS-Linien.

## 2. Architektur (ein Absatz)
Der Unity-Client (6.3 LTS, C#) ist ausschließlich für Rendering, Eingabe und lokale UI-Zustände zuständig und enthält **keine spielentscheidende Logik**; jede Aktion mit Fairness-Relevanz (Gacha-Pull, Charakter-Fortschritt, Käufe) wird über eine REST/WebSocket-API an einen monolithischen Node.js/TypeScript-Backend-Service gesendet, der als alleinige Quelle der Wahrheit fungiert, den Spielzustand transaktional in PostgreSQL persistiert und häufig gelesene/geschriebene Werte wie Pity-Counter und Session-Daten in Redis cached — bewusst als ein einzelner Service statt Microservices, um den Betriebsaufwand für eine Einzelperson gering zu halten (siehe Effizienzprinzip in SUMMARY.md 2.3).

## 3. Empfohlene Ordnerstruktur

**Unity-Projekt (Client):**
```
/Assets
  /Scripts
    /Combat        (Auto-Battle-Grid-Logik, Client-seitige Darstellung)
    /Gacha         (UI + API-Calls, KEINE RNG-Logik hier)
    /Characters     (Datenmodelle, Progression-UI)
    /Network        (API-Client, WebSocket-Wrapper)
    /UI             (Screens, gemeinsame UI-Komponenten)
  /Art
    /Characters     (aus deiner Bildgenerierungs-Pipeline)
    /UI
    /Environment
  /Prefabs
  /Scenes
  /Resources
```

**Backend (separates Repo empfohlen):**
```
/src
  /api            (REST/WebSocket-Routen)
  /gacha          (Pity-Logik, RNG, server-autoritativ)
  /characters      (Progression, Leveling)
  /db              (PostgreSQL-Schema, Migrationen)
  /cache           (Redis-Zugriff)
  /auth            (Gast-Login, später OAuth)
/tests
```
→ Diese Struktur ist ein Vorschlag, kein finaler Beschluss — siehe "Decisions Pending" in 03-active.md.

## 4. Wichtigste Entscheidungen mit Begründung

**1. Unity statt Godot** — Ursprünglich wurde Godot wegen besserer Cursor-Kompatibilität (textbasierte Szenen) empfohlen; nach Rücksprache aber auf Unity zurückgestellt, weil bereits Unity-Erfahrung vorhanden ist und der Lernaufwand für eine neue Engine bei 5–15 Std./Woche nicht gerechtfertigt ist.

**2. PostgreSQL statt NoSQL (z. B. MongoDB)** — Gacha-Historie und Käufe brauchen transaktionale Integrität und Audit-Fähigkeit (z. B. bei Streitfällen über Pull-Ergebnisse); ein relationales Schema mit Fremdschlüsseln zwischen Spieler/Inventar/Gacha-Log ist hierfür deutlich robuster als dokumentbasierte Stores.

**3. Monolithischer Backend-Service statt Microservices zu Beginn** — Bei Solo-Entwicklung ist der Betriebsaufwand mehrerer Services (Deployment, Monitoring, Inter-Service-Kommunikation) unverhältnismäßig hoch; ein einzelner Node.js-Service mit klar getrennten internen Modulen (siehe Ordnerstruktur) ist migrierbar, sobald es nötig wird.

## Freigabe-Status
🟡 **Entwurf — noch nicht freigegeben.** Plattform-Entscheidung (Mobile/PC/beides, siehe 01-project.md) wirkt sich noch auf Unity-Build-Settings/Input-System aus und ist hier noch nicht eingearbeitet.
