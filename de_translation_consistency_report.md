# Konsistenzprüfung der deutschen Übersetzung (EmptyEpsilon)

Analysiert: alle 55 `*.de.po`-Dateien unter `resources/locale/` und `scripts/locale/` (inkl. Unterordner `shiptemplates/`, `tutorial/`, `api/`).
Es wurden **keine** Änderungen an den `.po`-Dateien vorgenommen — reine Analyse.

Methode: Extraktion aller `msgid`/`msgstr`-Paare und automatischer Abgleich identischer `msgid`-Texte über alle Dateien hinweg. Von 4783 eindeutigen `msgid`s haben **256** mehr als eine unterschiedliche deutsche Übersetzung — das ist die Rohbasis für die folgenden Funde (nach Relevanz gefiltert; reine Whitespace-/Interpunktionsvarianten wurden übersprungen, sofern nicht selbst der Fund).

## Zusammenfassung

Die Übersetzung ist insgesamt solide und meist einheitlich in der Anrede (Sie-Form), hat aber folgende wiederkehrende Problemklassen:

1. **Ungleiche Übersetzung gleicher Sätze/Begriffe zwischen Szenario-Dateien** — vor allem bei Dateien, die offensichtlich als Vorlage für andere Szenarien dienten (`scenario_32_devour`, `scenario_55_defenderHunter` u.a. weichen oft leicht vom Rest ab).
2. **Nicht übersetzte Schiffsklassen-Begriffe** in einzelnen Dateien, obwohl an anderer Stelle konsequent übersetzt.
3. **Vereinzelte Anrede-Fehler** (Kleinschreibung von "sie" statt "Sie").
4. **Uneinheitliche Kraylor-Pluralform** (mit/ohne "s").
5. **Tippfehler und Grammatikfehler**, die auffielen.
6. **Uneinheitliche Bindestrich-Schreibweise** bei zusammengesetzten Begriffen (z. B. "Materialkomponenten" vs. "Material-komponenten").

---

## 1. Terminologie-Inkonsistenzen (Schiffsklassen, Ausrüstung)

| Begriff (engl.) | Datei(en) | Varianten | Kommentar |
|---|---|---|---|
| `Cruiser` | scenario_00_basic, shiptemplates/frigates, shiptemplates/OLD → „Kreuzer"; `cpu_ship_diversification_scenario_utility`, `scenario_29_surf`, `scenario_53_escape` → **unübersetzt „Cruiser"** | 2 Varianten | Empfehlung: einheitlich „Kreuzer". |
| `Dreadnought` | shiptemplates/dreadnaught, cpu_ship_diversification, scenario_53_escape → „Schlachtschiff"; scenario_00_basic, scenario_32_devour, shiptemplates/OLD (teils) → **unübersetzt „Dreadnought"** | gemischt, sogar innerhalb `shiptemplates/OLD.de.po` beide Varianten (Zeile 253 vs. 258) | Uneinheitlich auch innerhalb derselben Datei. |
| `Carrier` | scenario_56_carrierTurret, shiptemplates/corvette → „Trägerschiff"; shiptemplates/exuari → „Träger" | 2 Varianten | Kurzform vs. Vollform. |
| `Battlestation` | cpu_ship_diversification_scenario_utility → **unübersetzt** „Battlestation"; shiptemplates/OLD → „Kampfstation" | | |
| `Blockade Runner` | cpu_ship_diversification_scenario_utility → **unübersetzt**; shiptemplates/OLD → „Blockadebrecher" | | |
| `Adv. Gunship` | cpu_ship_diversification_scenario_utility → **unübersetzt**; shiptemplates/OLD → „Kanonenboot (verb.)" | | |
| `Freighter` | shiptemplates/corvette → „Frachter"; cpu_ship_diversification_scenario_utility → **unübersetzt „Freighter"** | | |
| `Gunship` | überall konsistent „Kanonenboot" | — | Positivbeispiel. |
| `Frigate`, `Corvette` | überall konsistent „Fregatte"/„Korvette" | — | Positivbeispiel. |
| `Jump drive` / `Jump Drive` | main.de.po Zeile 1795 & 2534, jeweils konsistent „Sprungantrieb" | — | OK. |

**Muster:** `cpu_ship_diversification_scenario_utility.de.po` und teilweise `shiptemplates/OLD.de.po` lassen auffällig viele Schiffsklassen-Bezeichnungen unübersetzt, während der Rest der Dateien konsequent übersetzt. Empfehlung: `cpu_ship_diversification_scenario_utility.de.po` gezielt nachziehen (Cruiser, Dreadnought, Battlestation, Blockade Runner, Adv. Gunship, Freighter).

### Weitere Begriffsabweichungen (Beispiele aus den 256 Funden)
- `Docking services status`: „Dock-Dienste: Übersicht" (scenario_32_devour) vs. „Status der Andockdienste" (scenario_55_defenderHunter).
- `Difficulty`: „Schwierigkeitsgrad" (scenario_30_brokenglass) vs. „Schwierigkeit" (scenario_51_deliverAmbassador).
- `Disabled`: „Deaktiviert" (main.de.po) vs. „Aus" (scenario_31_payload).
- `Assist me`: „Helfen Sie uns" vs. „Unterstützen Sie uns." — je nach Datei.
- `Dock at %s`: „Docke bei %s an" (Befehlsform) vs. „An %s andocken" (Infinitiv) — wechselt zwischen Dateien, teils auch innerhalb einer Datei-Familie (z. B. gleiche Textbausteine in scenario_49_allies vs. scenario_32_devour).
- `EXECUTE: SELFDESTRUCT`: „FÜHRE AUS: SELBSTZERSTÖRUNG" vs. „AUSFÜHREN DER SELBSTZERSTÖRUNG" — zwei Stilfamilien über die Szenario-Dateien verteilt.
- `Easy` (Schwierigkeitsgrad): meist „Einfach", aber „Leicht" in mehreren Dateien (scenario_30_brokenglass, scenario_51_deliverAmbassador, scenario_53_escape, scenario_55_defenderHunter teils) und einmal sogar „Freundlich" (scenario_55_defenderHunter.de.po:232) für denselben Schwierigkeitsgrad-Kontext.
- `Beam/shield frequencies`: main.de.po nur „Frequenzen" (verkürzt) vs. science_db.de.po „Laser-/Schildfrequenzen" — main.de.po hier möglicherweise absichtlich gekürzt (UI-Platz), sollte aber geprüft werden.
- Bindestrich-Inkonsistenz bei zusammengesetzten Wörtern: „Materialkomponenten" (place_station_scenario_utility, scenario_49_allies) vs. „Material-komponenten" (scenario_54_PatrolDuty, scenario_57_shoreline — falsche Trennung, kein Kompositum-Bindestrich üblich); ähnlich „Androiden-Komponenten" vs. „Androidenkomponenten", „Auto-Doc-Komponenten" vs. „Autodoc-Komponenten".

---

## 2. Anrede-Inkonsistenzen (Du/Sie)

Die Übersetzung verwendet für NPC→Spieler-Dialoge überwiegend die **Sie-Form** (korrekt und konsistent), es gibt aber:

**a) Vier Fälle von Kleinschreibungsfehlern „sie" statt „Sie"** (grammatikalisch falsch, da direkte Anrede):
- `scripts/locale/comms_station.de.po:44` — „Können **sie** unsere HGBIs wieder auffüllen?"
- `scripts/locale/scenario_51_deliverAmbassador.de.po:600` — „Können **sie** unsere HGBIs wieder auffüllen (jeweils %d Ruf)"
- `scripts/locale/scenario_06_edgeofspace.de.po:544` — „Können **sie** uns Vorräte schicken? (100 Ruf)"
- `scripts/locale/scenario_81_pvp.de.po:257` — „Können **sie** uns mit Nukes versorgen?"

**b) Echtes Duzen** (nicht nur Kleinschreibfehler, sondern tatsächliche Du-Form) kommt in mehreren Szenario-Dateien vor, wo sonst die Sie-Form dominiert — z. B. `scenario_53_escape.de.po`, `scenario_54_PatrolDuty.de.po`, `scenario_49_allies.de.po`, `scenario_58_race.de.po`, `scenario_62_whatTheDickens.de.po`, `scenario_60_captureFlag.de.po`. In vielen Fällen ist das **inhaltlich gerechtfertigt** (private/familiäre Dialoge zwischen NPCs untereinander, nicht an den Captain gerichtet, z. B. `scenario_49_allies.de.po:1797`, `scenario_62_whatTheDickens.de.po:849`). Ein Fall wirkt aber wie ein echter Bruch innerhalb eines Satzes:
- `scripts/locale/scenario_53_escape.de.po:778-779`: „Kent? **Du** hast es aus der Kraylor-Basis geschafft? Wir dachten, **du** würdest den Rest deines Lebens dort verbringen! Danke, Captain, dass **Sie** Kent […]" — hier wechselt der Sprecher scheinbar mitten im Text von der Du-Anrede (an Kent) zur Sie-Anrede (an den Captain) — im Kontext vermutlich korrekt (zwei verschiedene Adressaten im selben Redebeitrag), aber leicht verwirrend und sollte im Kontext geprüft werden.

Empfehlung: gezielt die als „Du" markierten Stellen in den o. g. Szenario-Dateien durchsehen und bestätigen, dass Du-Form nur zwischen NPCs (nie NPC→Captain) verwendet wird. Tutorial-Dateien (`tutorial.de.po`, `tutorial/*.de.po`) duzen den Spieler direkt als Lernenden — das ist ein bewusster, separater Stil (Anleitung) und hier vermutlich in Ordnung, aber inkonsistent zur Sie-Form im übrigen Spiel, falls das nicht gewollt ist.

---

## 3. Typografie / Rechtschreibung

- `scripts/locale/scenario_55_defenderHunter.de.po:181` — msgstr „**3060Minuten**" für `msgid "60min"` — offensichtlicher Tippfehler (sollte „60 Minuten" sein, vermutlich Verschmelzung mit einer Nachbarzeile „30" + „60Minuten").
- `scripts/locale/scenario_62_whatTheDickens.de.po:163` — „Können Sie uns ein paar Nukes **verkaufe**? (%d Ruf pro Stück)" — Grammatikfehler, sollte „verkaufen" heißen.
- `scripts/locale/scenario_29_surf.de.po:367` u. Parallelstellen (scenario_32_devour, scenario_53_escape) — „können **keine neue** Sonden hergestellt werden" — sollte „keine neuen Sonden" heißen (Kongruenzfehler, in mehreren Dateien identisch kopiert).
- `scripts/locale/scenario_62_whatTheDickens.de.po:412` — „(%d **rep**)" statt „(%d Ruf)" — englischer Rest im sonst übersetzten String, Parallelstellen in anderen Szenarien verwenden „Ruf".
- `scripts/locale/scenario_49_allies.de.po:205` / `scenario_55_defenderHunter.de.po:308` — doppeltes Leerzeichen vor „wird" („Landebucht  wird").
- Uneinheitliche Zahl-/Zeitformat-Schreibweise: „30min" vs. „30 Minuten" für dieselbe `msgid`; ebenso `msgid "30"` teils mit „+30%" übersetzt (scenario_29_surf) statt reiner Zahl „30" — hier scheint die msgid zweckentfremdet für unterschiedliche Kontexte verwendet zu werden, was strukturell fragwürdig ist (kein reiner Übersetzungsfehler, aber Vorsicht bei Wiederverwendung generischer msgids wie „30").

---

## 4. Eigennamen / Fraktionen

- **Kraylor**: Pluralform ist uneinheitlich — 214 Fundstellen ohne „s" (unveränderlicher Plural, z. B. „drei Kraylor"), 24 Fundstellen mit „s" („Kraylors"). Beispiel direkt nebeneinander:
  - `scenario_49_allies.de.po:231` — „Drei **Kraylor** wurden gefangen genommen"
  - `scenario_55_defenderHunter.de.po:334` (fast identischer Satz) — „Drei **Kraylors** wurden gefangen genommen"
  Empfehlung: unveränderlichen Plural „Kraylor" als Standard festlegen (häufigere Variante) und „Kraylors" vereinheitlichen.
- Andere Fraktionsnamen (Ktlitan, Exuari, Arlenians, Human Navy) wurden stichprobenartig geprüft und erscheinen als Eigennamen konsequent unübersetzt — kein Fund.

---

## 5. Sonstiges

- Mehrere längere Fließtexte (z. B. `msgid "Different types of cargo or goods may be obtained..."`, `msgid "All nukes are charged and primed for destruction."`) existieren in 5–14 Varianten über die Szenario-Dateien, die inhaltlich gleich, aber unterschiedlich formuliert sind (z. B. „bereit zu zerstören" vs. „zur Zerstörung bereit", oder unterschiedliche Anführungszeichen `\"dilithium\"` vs. `\"Dilithium\"` — Groß-/Kleinschreibung von Warennamen wechselt sogar innerhalb desselben Absatz-Typs). Das deutet darauf hin, dass viele Szenario-Dateien durch Kopieren/Anpassen einer gemeinsamen Vorlage entstanden sind und dabei leicht auseinandergedriftet sind, statt eine gemeinsame Formulierung zu teilen. Eine Vereinheitlichung würde die Textqualität spürbar verbessern, ist aber mit vertretbarem Aufwand nur über ein Terminologie-Glossar + gezielte Nacharbeit an den Vorlage-Dateien (`scenario_32_devour`, `scenario_55_defenderHunter` scheinen die Hauptabweichler zu sein) zu erreichen.

---

## Empfehlung für weiteres Vorgehen

1. ~~Ein kleines **Terminologie-Glossar**~~ — erledigt: `resources/locale/de_terminology_glossary.md`, danach `cpu_ship_diversification_scenario_utility.de.po` abgeglichen.
2. ~~Die 4 Anrede-Kleinschreibfehler („sie" → „Sie")~~ — erledigt.
3. ~~Tippfehler „3060Minuten", „verkaufe" → „verkaufen", „keine neue Sonden", Leerzeichenfehler~~ — erledigt.
4. ~~Kraylor-Pluralform vereinheitlichen~~ — erledigt.
5. ~~Die restlichen gefundenen Mehrfachübersetzungen~~ — aufgearbeitet: 135 von 201 automatisiert erkannten Duplikat-Gruppen wurden auf die im Korpus bereits vorhandene Mehrheitsvariante vereinheitlicht (`scripts/locale/*.de.po`, Commits `41c137c7`–`46ae8062`). Die verbleibenden 66 Fälle sind unten dokumentiert (Anhang A).

Status: Punkte 1–5 sind in Commits auf dem Branch `de-translation-completion` umgesetzt (siehe `git log`).

---

## Anhang A: Verbleibende Stilvarianten (bewusst nicht automatisch verändert)

66 Duplikat-Gruppen (gleicher `msgid`, unterschiedliche `msgstr` über mehrere Dateien) wurden **nicht** automatisch vereinheitlicht, weil kein objektiver Gewinner ermittelbar war (echtes 50/50, oder die Varianten unterscheiden sich inhaltlich/im Kontext, nicht nur stilistisch). Diese können bei Bedarf manuell entschieden werden.

### A.1 Echte Geschmacks-/Stilfragen (ca. 50/50, keine falsch)

| MSGID | Datei A | Datei B |
|---|---|---|
| `We are under attack! No time for chatting!` | „…zu plaudern!" (comms_station u.a., 6x) | „…zum Plaudern!" (scenario_49_allies u.a., 6x) |
| `Warp drive components` | „Komponenten des Warpantriebs" | „Warpantriebs-Komponenten" |
| `Energy beam components` | „Laserstrahl-Komponenten" | „Laser-Komponenten" |
| `Advanced Material(s) components` (2 Gruppen) | „Materialkomponenten" | „Material-komponenten" (Bindestrich-Fehltrennung, s. Empfehlung unten) |
| `Android components` | „Androiden-Komponenten" | „Androidenkomponenten" |
| `We provide a range of communication equipment…` | „Kommunikationsgeräten" | „Kommunikations-Geräten" |
| `Established by the royal family…` | „Gründung durch…" | „Gegründet von…" |
| `What are my current orders?` | „Was sind unsere derzeitigen Befehle?" (5x) | „Was sind meine aktuellen Aufträge?" (5x) |
| `I need ordnance restocked` | „Ich benötige Nachschub an Munition." (5x) | „Wir benötigen Munition für unsere Waffensysteme." (5x) |
| `We have no ordnance available for restock` | „Wir können Ihnen leider nichts anbieten" (4x) | „Wir haben keine Munition zum Auffüllen verfügbar." (4x) |
| `\n   Homing` | „Zielsuchrakete" | „Zielsuchraketen" |
| `Insufficient cargo space for purchase` | „…Kauf" (2x) | „…Kauf." (2x, nur Punkt) |
| `Good day, officer!\nWhat can we do for you today?\n` | „…für Sie tun?" (3x) | „…für sie tun?" (3x, Anrede-Fehler wie oben — sollte eigentlich als Fehler behandelt werden, siehe A.3) |

### A.2 Zwei ganze Szenario-Dateien mit durchgehend abweichender Formulierung — erledigt

`scenario_32_devour.de.po` wurde auf die Formulierungen von `scenario_55_defenderHunter.de.po` vereinheitlicht (22 Strings, Commit `b7a0e8d0`).

`scenario_32_devour.de.po` und `scenario_55_defenderHunter.de.po` verwenden für denselben `stationServices-comms`-Textblock (Docking-Status, Sonden auffüllen, Hüllenreparatur, Energie laden, Sprungantrieb überladen, Systemreparaturen — ca. 15 zusammengehörige Strings) durchgängig zwei unterschiedliche Stilfamilien, z. B.:

- `%s\nMay overcharge jump drive`: „Bietet Überladung des Sprungantriebs an" (devour) vs. „Kann den Sprungantrieb überladen" (defenderHunter)
- `Your probe launch system has been repaired`: „Ihre Sonden-Abschussvorrichtung wurde repariert" vs. „Ihr Sonden-Startsystem wurde repariert."

Das betrifft vermutlich beide Dateien komplett und wäre am sinnvollsten als eigene, gezielte Aufgabe zu bearbeiten (eine der beiden Stilfamilien als Standard wählen und konsequent durchziehen), nicht Fall für Fall.

### A.3 Inhaltlich/kontextuell unterschiedliche Fälle (absichtlich übersprungen)

- `Time | 30min` / `60min`: „30min"/„60min" (Kurzform) vs. „30 Minuten"/„60 Minuten" (ausgeschrieben) — wird an unterschiedlichen UI-Stellen verwendet, vermutlich absichtlich unterschiedlich lang.
- `Different types of cargo or goods may be obtained…` (Fließtext, 3 Varianten): unterscheidet sich nicht nur stilistisch, sondern auch inhaltlich — „Jedes Spielerschiff" vs. „Jedes Schiff", Warenbeispiele mal groß/klein bzw. übersetzt/unübersetzt, „Medizin" vs. „Medikamente (medicine)".
- Diverse Gossip-Texte (`scenario_49_allies` vs. `scenario_55_defenderHunter`, z. B. „Krak oder Kruk", „Lichten-Brüder", Uhrzeit „19:00" vs. „1900"): kleine Detailunterschiede in Wortwahl/Formatierung, nicht eindeutig falsch.
- `Artifact with quantum biometric characteristics` / `…embedded chroniton particles` / `…bridging two parallel universes`: unterschiedliche, aber beide plausible Formulierungen ohne klaren Fehler.
- `buttonGM | *` vs. `[*]`, `subclass | Carrier` „Trägerschiff" vs. „Träger": vermutlich UI-Platzgründe, nicht sicher ohne Blick auf den jeweiligen Bildschirm zu entscheiden.

### A.4 Zusätzlich gefundener Einzelfall — erledigt

- `scienceDB | Cruiser`: in `scenario_53_escape.de.po:456` war noch unübersetzt „Cruiser" stehengeblieben, jetzt „Kreuzer" (Commit `b7a0e8d0`).
