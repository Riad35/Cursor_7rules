# Prompt: Damage Pipeline Implementation (Unity / C#)

**After MVP — do not ship now.** Parked 2026-08-20.

Keep this file as a **post-MVP reference** (named steps, penetration, dealt/taken %, shield in the result, DoT/reflect through one resolver). Live combat stays **server-authoritative** in `gAAAcha/server/src/combat/damage.ts` per `gAAAcha/docs/combat-rules.md`. Do **not** add a Unity `DamagePipeline` that writes HP. When we pick this up: split the TypeScript resolver into named steps first; optional C# mirror is prediction-only.

---

Kopiere diesen kompletten Prompt in Cursor/deine AI, um `DamagePipeline.cs` implementieren zu lassen.

---

## Kontext

Ich baue ein Gacha-RPG in Unity 6.3 LTS (C#, URP 2D). Ich brauche ein zentrales `DamagePipeline`-System, das JEDE Schadensberechnung im Spiel durchläuft (normale Angriffe, Skills, DoTs, Reflect-Damage). Implementiere es als eine strikt sequenzielle Pipeline mit 14 klar getrennten, testbaren Schritten. Reihenfolge ist bindend — nicht optimieren oder Schritte zusammenlegen, auch wenn es mathematisch möglich wäre. Jeder Schritt muss als eigene private Methode existieren, damit ich später einzelne Schritte unit-testen kann.

## Anforderungen

### Datenstrukturen (zuerst anlegen)

- `CombatStats` struct/class: ATK, DEF, RES, CritChance, CritMultiplier, Penetration%, FlatPenetration, DamageDealtIncrease%, DamageTakenIncrease%, Element (enum)
- `SkillData` struct: Ratio (float), FlatValue (float), DamageType (enum: Physical/Magical/True), Element (enum), CanCrit (bool), HitCount (int)
- `DamageResult` struct: FinalDamage (int), WasCrit (bool), Element, RawDamage (float, vor Rundung), Absorbed (int)
- `ElementChart`: ScriptableObject mit einer Matrix[Element,Element] für Multiplikatoren (default 1.0, Vorteil 1.25, Nachteil 0.75)

### Pipeline-Schritte (in genau dieser Reihenfolge implementieren)

1. **Input-Aggregation**: `AttackerStats`/`DefenderStats` werden als bereits final aggregierte Werte übergeben (Base + Equipment + Buffs + Level). Die Pipeline selbst liest KEINE Rohwerte, nur die fertigen Structs.
2. **Basisschaden**: `BaseDamage = (AttackerStats.ATK * Skill.Ratio) + Skill.FlatValue`
3. **Damage-Type-Weiche**: bei `DamageType.True` überspringe Schritt 6 (Mitigation) komplett — als expliziter early branch, nicht als if-Sonderfall am Ende.
4. **Additive %-Modifikatoren**: alle "+X% Damage Dealt"-Buffs werden INNERHALB der Kategorie addiert (nicht multipliziert), dann einmal auf BaseDamage angewendet: `Damage = BaseDamage * (1 + SumIncrease - SumDecrease)`
5. **Kritischer Treffer**: ein Random-Roll gegen `CritChance`, bei Erfolg `Damage *= CritMultiplier`. Bei Multi-Hit-Skills (`HitCount > 1`) wird pro Hit einzeln gewürfelt.
6. **Elementar-Multiplikator**: Lookup in `ElementChart`, `Damage *= ElementMult`.
7. **Defense-Mitigation** (nur Physical/Magical): NICHT lineare Subtraktion verwenden. Stattdessen Diminishing-Returns-Formel:
   ```
   EffectiveDEF = DEF * (1 - Penetration%) - FlatPenetration
   Mitigation = EffectiveDEF / (EffectiveDEF + K)   // K als [SerializeField], default 100
   Damage *= (1 - Mitigation)
   ```
8. **Zufalls-Varianz**: `Damage *= Random.Range(0.95f, 1.05f)`
9. **Vulnerability-Layer**: separat von Schritt 4 — wendet `Defender.DamageTakenIncrease%` an, weil diese Quelle vom Verteidiger-Debuff kommt, nicht vom Angreifer-Buff.
10. **Clamping & Rundung**: `Damage = Mathf.Clamp(Damage, 1, MaxDamageCap)`, danach EINMALIG `Mathf.RoundToInt`. Vorher darf nirgendwo gerundet werden.
11. **Schild-Absorption**: falls `Defender.Shield > 0`, zieht zuerst vom Schild ab, Rest geht auf HP.
12. **HP-Abzug & Event-Trigger**: `OnDamageTaken`-Event feuern (für Lifesteal, Thorns, Ultimate-Energy-Gain, Aggro-Updates — diese NICHT selbst implementieren, nur den Hook bereitstellen).
13. **Death-Check**: bei HP <= 0 `OnDeath`-Event feuern.
14. **Presentation-Hook & Logging**: Rückgabe eines `DamageResult`, damit VFX/SFX/Damage-Numbers/Logging außerhalb der Pipeline (im Aufrufer) passieren — Pipeline selbst bleibt reine Logik, keine Unity-Rendering-Calls darin.

### Wichtige Constraints

- Pipeline-Klasse ist zustandslos (static Methode oder reines Objekt ohne MonoBehaviour), damit sie aus jedem Kontext (Server-Sim, Client-Prediction, Unit-Tests) aufrufbar ist.
- Jeder der 14 Schritte als eigene `private` Methode mit klarem Namen (`Step1_AggregateInputs`, `Step2_CalculateBaseDamage`, usw.), damit ich sie einzeln testen kann.
- Keine `Debug.Log` im Pipeline-Code — Logging über ein separates `ICombatLogger`-Interface, das injiziert wird.
- Schreibe dazu auch 3–5 Beispiel-Unit-Tests (NUnit, Unity Test Framework), die einzelne Schritte isoliert prüfen (z. B. Diminishing-Returns-Formel bei DEF=0, DEF=1000, mit/ohne Penetration).

### Output, das ich erwarte

1. `DamagePipeline.cs` mit allen 14 Schritten
2. `CombatStats.cs`, `SkillData.cs`, `DamageResult.cs`
3. `ElementChart.cs` als ScriptableObject
4. `DamagePipelineTests.cs` mit den Unit-Tests

Frag nach, falls dir Werte für `K` (Mitigations-Konstante) oder `MaxDamageCap` fehlen — schlage sinnvolle Defaults vor statt sie zu erfinden ohne Kommentar.
