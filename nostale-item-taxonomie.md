# NosTale – Item-Taxonomie (Referenz für Gacha-RPG-Adaption)

Vollständige Kategorisierung aller Itemtypen aus NosTale, mit generischen Attributen (Stats/Properties) und Assets (technischer Produktionsaufwand pro Item). Keine konkreten Itemnamen – nur die zugrunde liegenden Typen, wie man sie als `ItemBase`-Subtypen in Unity modellieren würde.

---

## A. Ausrüstung (Equipment)

| Slot-Typ | Attribute | Assets |
|---|---|---|
| Hauptwaffe (klassengebunden) | Angriffswert-Bereich (min/max), Treffer-/Konzentrationsbonus, Elementaraffinität, Seltenheitsstufe (-2 bis +8/+10), Levelanforderung, Klassenbindung | Icon, Equipped-Sprite/Model, Angriffs-VFX, Trefferschall, Upgrade-Glanzeffekt |
| Sekundärwaffe | wie Hauptwaffe, meist geringerer Wert | Icon, Sprite |
| Rüstung/Brust | Verteidigungswert (Nah-/Fernkampf getrennt), HP-Bonus, Ausweichwert, Elementarresistenz | Icon, Vollkörper-Sprite/Model pro Klasse, Materialglanz-Shader |
| Handschuhe | Verteidigung, ggf. Elementarresistenz-Slot (fusionierbar) | Icon, Hand-Sprite-Overlay |
| Stiefel | Verteidigung, Bewegungsgeschwindigkeit, Elementarresistenz-Slot | Icon, Fuß-Sprite-Overlay |
| Kopfbedeckung/Hut | Verteidigung, optisch klassenspezifisch | Icon, Kopf-Overlay-Sprite |
| Halskette | Sekundärstat (HP/MP/Crit) | Icon, ggf. Glow-VFX |
| Ring (2 Slots) | Sekundärstat, teils Set-Bonus | Icon |
| Armband (2 Slots) | Sekundärstat, teils Set-Bonus | Icon |
| Maske | kosmetisch + kleiner Stat | Icon, Gesichts-Overlay |
| Kostüm/Anzug | rein kosmetisch (meist ohne Kampfwert), Set-Zugehörigkeit | Vollkörper-Sprite/Model, Farbvarianten |
| Flügel | kosmetisch + geringer Stat, Partikeleffekt beim Laufen/Springen | Sprite-Overlay, Idle/Walk-VFX |
| Fee (Fairy) | Elementarschadensbonus, Levelbalken bis Max-Potenzial | Icon, Begleiter-Sprite, Idle-Animation |

**Meta-Attribute, die JEDES Ausrüstungsteil zusätzlich trägt:** Seltenheitsstufe (Farbe im Namen), Verstärkungslevel (+0 bis +10), Sockel für Zusatzattribute (Shells), Haltbarkeit/Zerstörungsrisiko bei Fehlversuch, Handelbarkeits-Flag.

---

## B. Transformations-Items (Spezialistenkarten-Analogon)

| Typ | Attribute | Assets |
|---|---|---|
| Klassentransformation | komplett eigenes Skillset, Elementbindung, Levelanforderung, Fusionsstufe | Vollständiges Charaktermodell/Sprite-Set, eigene Skill-VFX-Bibliothek, eigene Idle/Attack-Animationen |

---

## C. Begleiter-Items

| Typ | Attribute | Assets |
|---|---|---|
| Haustier-Beschaffung (Ei/Kapsel) | Seltenheit, Basiswerte-Rolle, Wachstumsrate | Icon, Schlupf-VFX |
| Begleiter-Ausrüstung | eigene reduzierte Stat-Range, 4 Slots (Waffe/Rüstung/Handschuh/Schuh) | separate Sprite-Sets pro Begleiter |
| Begleiter-Nahrung | Levelboost, Zuneigungswert | Icon |

---

## D. Verbrauchsgegenstände (Consumables)

| Typ | Attribute | Assets |
|---|---|---|
| Heilung (HP/MP) | Regenerationsmenge, Cooldown, Stapelgröße | Icon, Nutzungs-VFX/SFX |
| Buff-Elixier | Stat-Boost-Wert, Dauer, Stapelbarkeit mit anderen Buffs | Icon, Buff-Icon-Overlay, Partikel |
| Statusaktivierung (z. B. PvP-Trank) | Dauer, Gebietsbindung | Icon, Aura-VFX |
| Teleport/Rückruf | Zielort, Cooldown | Icon, Teleport-VFX/SFX |
| EXP-/Dropboost | Multiplikator, Dauer | Icon |

---

## E. Aufwertungsmaterialien

| Typ | Attribute | Assets |
|---|---|---|
| Seltenheits-Erhöhung | Erfolgschance, Ziel-Item-Kompatibilität | Icon |
| Verstärkung (+Level) | Erfolgschance je Level, Materialkosten-Kurve | Icon, Erfolgs-/Fehlschlag-VFX |
| Schutz vor Zerstörung | Nutzungsanzahl, Kompatibilität | Icon |
| Sockelsteine/Shells | Zusatzattribut-Pool, benötigtes Item-Level/-Seltenheit | Icon, Sockel-VFX |
| Identifikationsitem | deckt zufällige Attributoptionen auf | Icon |
| Fusionsmaterial | kombiniert gleiche Items zu höherer Stufe | Icon, Fusions-VFX |

---

## F. Quest-/Zugangsgegenstände

| Typ | Attribute | Assets |
|---|---|---|
| Questgegenstand | questgebunden, nicht handelbar, Stapelgröße | Icon |
| Instanz-/Stage-Zugang (Time-Space-Stein) | Zielinstanz-ID, Verbrauch bei Nutzung, Zeitlimit | Icon, Portal-VFX |
| Raid-Zugang (Siegel) | Gruppenbindung, Verfallszeit | Icon |
| Schlüssel/Werkzeug für versteckten Content | Materialkombination nötig | Icon |

---

## G. Behälter (Lootboxen/Gacha-Boxen)

| Typ | Attribute | Assets |
|---|---|---|
| Zufallsbox | Drop-Pool mit Gewichtungen, garantierte Mindestrarität, Pity-Zähler-Bindung | Icon, Box-Öffnungs-VFX/SFX, Reveal-Animation |
| Event-/Zeitbox | zusätzlich Zeitfenster-Flag | wie oben |

---

## H. Kosmetika

| Typ | Attribute | Assets |
|---|---|---|
| Titel | reiner Text/UI-Flag, kein Kampfwert (teils kleiner Bonus) | UI-Textstil |
| Frisuren-/Farbitem | Permanent-Flag, Charaktermodell-Variante | Farbvariante-Textur |
| Waffen-/Ausrüstungs-Skin | rein visuell, überschreibt Basis-Sprite ohne Statänderung | Sprite-Override |
| Reittier/Mount | Bewegungsgeschwindigkeitsbonus, kosmetisches Modell | Sprite/Model, Reit-Animation |

---

## I. Wirtschafts- & Housing-Items

| Typ | Attribute | Assets |
|---|---|---|
| Währung (Gold/Premium) | kein Inventarplatz, reiner Zähler | Icon für UI |
| Handelsrohstoff | Marktpreis-Referenz, Stapelgröße | Icon |
| Housing-Dekoration | Platzierungsposition/-rotation, Interaktionsflag | 3D/2D-Deko-Asset, Platzierungs-Ghost-Preview |

---

## Generisches Basis-Attribut-Set (jedes Item erbt davon)

`ID`, `Name`, `Icon`, `Beschreibung`, `Itemtyp/Kategorie`, `Stapelgröße`, `Seltenheit`, `Levelanforderung`, `Klassenbindung (optional)`, `Handelbar (bool)`, `Verkaufspreis`, `Questgebunden (bool)`

## Generisches Asset-Set pro Item (Produktionsaufwand)

1 Icon (Inventar/UI) → optional 1 Equipped-Sprite/Overlay → optional VFX (Nutzung/Upgrade/Sockel) → optional SFX → optional Beschreibungstext/Lore-String

---

## MVP-Empfehlung (gAAAcha — Adventurer L1–20)

Für den MVP-Scope zuerst umsetzen (siehe auch `adventurer_class_prompt.md`):

| Priorität | Taxonomie | gAAAcha |
|-----------|-----------|---------|
| P0 | **A** Haupt- + Sekundärwaffe | Dual slots; Adventurer = Schwert + Bogen |
| P0 | **A** Rüstung/Helm/Stiefel/Handschuhe/Accessoire | Vorhandene thin slots |
| P0 | **D** Consumables | Ration/Stew/Homestone |
| P1 | **G** Boxen / Gacha | Banner + Pity |
| P1 | **F** Stage-Zugang | Tower / Instanzen |
| P2 | **E** Verstärkung + Seltenheit | Enhance / Shells später |
| Später | Fee, Pets, Kostüme, Flügel, Mounts, Housing, Raid-Siegel | Nach Class-Change L20+ |

Adventurer-Skillkit = **8 inkl. AA** bis Level 20; Class-Cards erst ab Level 20.

Das deckt die Kern-Progressionsschleife ab, ohne sofort 15+ Itemkategorien an Assets produzieren zu müssen.
