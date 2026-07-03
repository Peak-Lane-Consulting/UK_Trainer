# Personal UK Trainer — Prototyp

Ein interaktiver Selbst-Check und wöchentlicher Begleiter für die ORF-Unternehmenskultur. Verhaltensökonomisch fundiert: Der Trainer misst nicht Meinungen, sondern Verhalten in konkreten Situationen — und macht die Lücke zwischen Selbstbild und Verhalten (Blind Spots) erlebbar.

**Ein einziges File:** `index.html` — kein Build, kein Server, keine Abhängigkeiten. Im Browser öffnen, fertig.

---

## Version 2 — „Erlebnis-Update" (Juli 2026)

Diese Version verwandelt den Check von einem Fragebogen in ein Erlebnis mit Neugier-Schleifen, sofort umsetzbaren Tipps und einem persönlichen KI-Companion.

### Neu in dieser Version

| Feature | Beschreibung |
|---|---|
| 🎲 **Selbst-Wette** | Vor jedem Handlungsfeld gibt man eine Prognose ab (1–10). Nach dem Feld: Wette vs. Ergebnis. Am Ende eine Kalibrierungs-Auswertung („Wie gut kennst du dich?"). Macht Blind Spots erlebbar statt nur benannt. |
| 💡 **Aha-Karten** | Die „Warum das zählt"-Kontexte erscheinen erst **nach** der Antwort. Vorher stand die Erklärung über der Frage und hat die „richtige" Antwort verraten (Priming) — jetzt ist sie ein Erkenntnismoment. |
| 🃏 **Denkfallen-Sammelkarten** | Jedes abgeschlossene Feld schaltet eine Karte frei: eingedeutschter Bias-Name, Alltagsbeispiel, Gegen-Trick, Sammelzähler („3 von 7"). Endauswertung zeigt die komplette Sammlung. |
| 🎯 **21 feldspezifische Tipp-Texte** | Pro Handlungsfeld × Score-Stufe (stark/solide/Entwicklungsfeld) ein eigener Text nach fester Formel — statt drei generischer Textbausteine. |
| 🌸 **Ghost-Blüte** | Die Ziel-Silhouette (Woche 13) liegt als gestrichelter Umriss hinter der Ist-Blüte — das Wachstumsversprechen wird sichtbar. |
| 🤖 **Personalisierter Companion** | Der KI-Companion kennt jetzt die konkreten Self-Check-Antworten, Blind-Spot-Deltas und die Wette. Sein Opener zitiert eine echte Antwort statt generisch zu grüßen. |
| ✍️ **Sprachliche Schärfung** | Kostenehrliche Antwortoptionen, Fortschritts- statt Defizit-Framing, „Was das für dich bedeutet" vor der ORF-Perspektive. |
| 🐛 **Bugfix** | Die Zwischenauswertung zeigte wegen der randomisierten Feld-Reihenfolge oft das **falsche Handlungsfeld** (Index-Lookup in ungemischter Key-Liste). Jetzt wird das Feld direkt aus der beantworteten Frage abgeleitet. |

### Die Tipp-Formel

Alle Auswertungstexte folgen derselben vierteiligen Struktur (`TIPS` in `index.html`):

1. **Spiegel** — das Ergebnis in einem konkreten Satz: *„Im Normalbetrieb bist du prozesstreu – aber unter Zeitdruck wird die Abkürzung verlockend."*
2. **Warum** — die Denkfalle in Alltagssprache: *„Das ist die Gewohnheitsfalle unter Stress: Dein Gehirn greift automatisch zum schnellsten Weg, nicht zum besten."*
3. **Micro-Move** — genau EIN Wenn-Dann für diese Woche: *„WENN du diese Woche eine Abkürzung nehmen willst, DANN sprich es vorher laut aus."*
4. **Neugier-Haken** — eine Frage, die hängen bleibt: *„Wetten, dass es genau am Freitagnachmittag passiert?"*

### Die 7 Denkfallen (eingedeutscht)

| Handlungsfeld | Denkfalle | Fachbegriff |
|---|---|---|
| Prozessmanagement & Effizienz | Die Gewohnheitsfalle | Status Quo Bias |
| Wissensmanagement & Generationenwechsel | Der Mein-Schatz-Effekt | Endowment Effect |
| Führung & Leadership | Der Irgendwer-macht's-schon-Effekt | Verantwortungsdiffusion |
| Weiterentwicklung & Empowerment | Die Bin-ich-halt-nicht-Denke | Fixed Mindset |
| Kommunikation & Transparenz | Die Das-wissen-doch-eh-alle-Falle | Transparenz-Illusion |
| Feedback & Fehlerkultur | Die Verlust-Angst | Loss Aversion |
| Kooperation & Zusammenarbeit | Der Wir-zuerst-Reflex | In-Group Bias |

---

## Ablauf (User Journey)

```
Login (Code + API-Key) → Vision (3 Screens) → Zielbild-Blüte
  → pro Handlungsfeld (randomisierte Reihenfolge):
      🎲 Selbst-Wette (1–10)
      → 10 Fragen (mit 💡 Aha-Karten nach jeder Antwort)
      → Zwischenauswertung: Wette-Reveal + Tipp der Woche + 🃏 Denkfallen-Karte
  → Ergebnis: Kultur-Blüte (mit Ghost-Silhouette) + Kalibrierung + Kartensammlung
  → Zielwahl (ein Handlungsfeld)
  → Companion-Chat (KI, wöchentlich) → WENN-DANN-Impuls annehmen
```

Abkürzung möglich: Aus jeder Zwischenauswertung kann man direkt ein Ziel setzen und zum Companion springen („Zwischenergebnis").

## Architektur

`index.html` ist in Schichten organisiert (Kommentare im Code markieren sie):

- **Schicht 1 — Datenschicht:** `QUESTIONS` — 70 Fragen, 7 Handlungsfelder × 10. Fragetypen:
  - `sj` — Situational Judgment (4 Optionen, Score 0/1/2)
  - `bs_scale` + `bs_scenario` — Blind-Spot-Paare: Selbsteinschätzung (Skala 1–7) vs. Verhalten im Szenario. Delta > 3 Punkte ⇒ Blind Spot
  - `sob` — Second Order Belief (Fremdbild, Skala 1–7)
  - `yn` — Ja/Nein-Ehrlichkeitsfragen zur letzten 4 Wochen (`yn_score: 1` = Ja ist gut, `-1` = Ja ist Gegenindikator)
- **Schicht 1b — Denkfallen & Tipps:** `BIAS_INFO` (Name de/en, Icon, Alltagsbeispiel, Gegen-Trick) und `TIPS` (7 Felder × 3 Stufen × 4 Formel-Bausteine)
- **Schicht 2 — Scoring:** `computeHFScore()` gewichtet: SJ 40 % + Verhalten 30 % + Fremdbild 20 % + Ja/Nein 10 % → Score 0–10. Stufen: ≥ 7 stark · ≥ 5 solide · < 5 Entwicklungsfeld
- **Schicht 3 — UI-Logik:** Screen-Navigation (`G()`), Fragen-Rendering, Selbst-Wette (`predictions`), Blüten-SVG (`buildFlower`, mit `ghost`-Parameter für die Ziel-Silhouette)
- **Schicht 4 — KI-Companion:** direkter Anthropic-API-Call aus dem Browser (`claude-sonnet-4-6`). `buildSystemPrompt()` übergibt Profil, Denkfalle, Blind-Spot-Delta, Wette und die konkreten nicht-kulturkonformen Antworten (`collectWeakAnswers()`). Der Companion endet Sessions mit einem `IMPULS: WENN … DANN …`, den die UI als annehmbare Karte extrahiert.

## Datenschutz / Sicherheit (Prototyp-Stand)

- Keine Personendaten; Zugangscode ist Attrappe. Antworten bleiben im Browser (kein Backend, kein Tracking).
- Der Anthropic-API-Key wird nur im `sessionStorage` gehalten (weg beim Tab-Schließen) und direkt an die Anthropic-API gesendet (`anthropic-dangerous-direct-browser-access`). **Für einen Produktivbetrieb muss der API-Call über einen Server-Proxy laufen** — Key im Browser ist nur für Demos vertretbar.
- Companion-Systemprompt enthält Schutzgrenzen (Verweis auf Betriebsrat/Telefonseelsorge 142, keine Diagnosen, keine Namen).

## Testen

Smoke-Test (simuliert einen kompletten 70-Fragen-Durchlauf inkl. Wetten, Zwischenauswertungen, Endauswertung, Companion-Prompt) — Skript liegt nicht im Repo, Muster:

```bash
node --check <(sed -n '/<script>/,/<\/script>/p' index.html | sed '1d;$d')   # Syntax
open index.html                                                              # manuell durchklicken
```

## Ideen für die nächste Version

- **Kultur-Archetypen** am Ende („Die Brückenbauerin", „Der Klartext-Typ") — Identität statt Score, teilbar
- **Variierte Reward-Momente** (Halbzeit-Moment, „Blüte komplett") statt 7× Konfetti
- **Recheck Woche 13** mit Vorher/Nachher-Blüte (aktuell nur als Versprechen im Text)
- **Server-Proxy** für den API-Call + echte Zugangscodes
- Persistenz der Wochen-Impulse (aktuell geht der Verlauf beim Neuladen verloren)
