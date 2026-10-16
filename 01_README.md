# EDA King County: Wo spart Thomas Hansen beim Hauskauf?

Explorative Datenanalyse von rund 21.600 Hausverkäufen in King County (Seattle), Mai 2014 bis Mai 2015.
Beratungsfall: **Thomas Hansen**, 5 Kinder, wenig Budget, braucht ein großes Haus und möchte eine schöne Nachbarschaft.

**Leitfrage:** An welchen Stellschrauben kann Thomas beim Kauf am meisten sparen: Zeitpunkt, Lage oder Bauqualität?

Die Präsentation liegt als PDF im Projektordner: `261016_eda_project.pdf`.

---

## Ergebnisse auf einen Blick

| Hypothese | Aussage | Ergebnis |
|---|---|---|
| **H1 Zeitpunkt** | Große Häuser (mind. 4 Schlafzimmer) sind im Winter (Nov–Feb) im Median mind. 5 % günstiger als im Restjahr. | **Nicht bestätigt.** Winter −7 %, abgesichert nur −4 % (Bootstrap). Bei gleicher PLZ bleiben ca. −1 %. |
| **H2 Lage** | Je weiter ein großes Haus vom Zentrum Seattle entfernt ist, desto niedriger der Preis pro qm. | **Bestätigt.** Spearman ρ = −0,61, ca. −2,3 % pro km, Zentrum 3.786 $/qm gegenüber über 30 km 1.652 $/qm (−56 %). |
| **H3 Bauqualität** | Häuser mit Grade 7 sind pro qm mind. 15 % günstiger als Häuser mit Grade 9–10. | **Gesamtmarkt nicht bestätigt** (−11,7 %, abgesichert −10,6 %). **Bei gleicher PLZ und Größe bestätigt** (−20,6 %). |

**Empfehlung für Thomas:**
1. Im Süden ab ca. 10 km Entfernung zum Zentrum suchen (Renton, Kent, Federal Way, Auburn). Dort gibt es für 600.000 $ über 300 qm.
2. Auf gehobene Bauqualität verzichten. Das bringt bei vergleichbaren Häusern rund 20 % mehr Fläche fürs Geld.
3. Den Winter nur als kleinen Puffer sehen (Januar/Februar), nicht fest einplanen.

Die Lage ist der größte Hebel, danach die Bauqualität, zuletzt der Zeitpunkt. Die drei Zahlen stammen aus unterschiedlichen Vergleichen und zeigen daher die Größenordnung, kein exaktes Ranking.

---

## Projektstruktur

```
.
├── 02_eda_data_cleaning.ipynb          # Datenbereinigung
├── 03_eda_h1.ipynb                     # Hypothese 1: Zeitpunkt
├── 03_eda_h2.ipynb                     # Hypothese 2: Lage
├── 03_eda_h2_htmlvisual.ipynb          # interaktive HTML-Visualisierung zu H2
├── 03_eda_h3.ipynb                     # Hypothese 3: Bauqualität
├── 261016_eda_project.pdf              # Präsentation
├── seattle_preisspanne_interaktiv.html # interaktive Visualisierung (im Browser öffnen)
├── data/
│   ├── eda.csv                         # Rohdaten
│   └── eda_tk_cleaned.csv              # bereinigte Daten (Ergebnis von 02_eda_data_cleaning)
├── output/                             # exportierte Grafiken und HTML-Dateien
├── pyproject.toml                      # Projekt und Abhängigkeiten
├── uv.lock                             # festgelegte Paketversionen
├── LICENSE
└── README.md
```

**Reihenfolge:** Zuerst `02_eda_data_cleaning.ipynb` ausführen, das erzeugt `data/eda_tk_cleaned.csv`. Die Hypothesen-Notebooks `03_*` lesen diese Datei und sind voneinander unabhängig.

---

## Vorgehen

Alle Hypothesen-Notebooks folgen demselben Ablauf: Kunde und Hypothese, Daten verstehen, erkunden, Zusammenhänge, Hypothese prüfen, Folgefrage, Fazit.

### Datenbereinigung
- 21.597 Zeilen, **keine gelöscht**. Jede Zeile ist ein echter Verkauf, auch Ausreißer (Villen, Seelagen) bleiben drin.
- Duplikate geprüft: 177 Häuser mehrfach vorhanden, aber an verschiedenen Tagen verkauft (Weiterverkäufe), daher behalten.
- Fehlende Werte je Spalte passend gefüllt: Kellerfläche exakt aus Wohnfläche minus oberirdischer Fläche, Wasserlage und Aussicht mit dem häufigsten Wert 0, Renovierungsjahr mit 0 und in einer eigenen Spalte (`yr_renovated_unbekannt`) markiert.
- Datenfehler korrigiert: 33 Schlafzimmer auf rund 150 qm sind ein Tippfehler (3), Renovierungsjahre wie 20150 wurden durch 10 geteilt, nachdem geprüft war, dass alle auf 0 enden.
- Umrechnung von sqft in qm, neue Spalten für den Preis pro qm Wohnfläche und Grundstück.

### Methoden und warum

| Methode | Eingesetzt bei | Begründung |
|---|---|---|
| **Median** statt Mittelwert | überall | Preise sind rechtsschief, einzelne Villen verzerren den Mittelwert. |
| **Mann-Whitney-U-Test** (einseitig) | H1, H3 Gesamtmarkt | Vergleich zweier unabhängiger Gruppen, keine Normalverteilung nötig. |
| **Bootstrap** (2.000 Ziehungen, `seed=42`) | H1, H3 Gesamtmarkt | Zeigt, wie sicher die *Höhe* des Unterschieds ist (95-%-Untergrenze), da der Test nur „ja/nein“ sagt. |
| **Spearman-Korrelation** | H2, H3 | Rangbasiert, robust gegen Ausreißer, passt auch zur Rangskala Grade. |
| **Pearson-Korrelation** | nur Überblicksmatrix in H1 | Reiner Überblick, die Schlussfolgerung stützt sich auf Mann-Whitney und Bootstrap. |
| **Wilcoxon-Vorzeichen-Rang-Test** | H3 fairer Vergleich | Jede Kombination aus PLZ und Größenklasse ist ein Paar (Grade 7 und gehoben am selben Ort), keine Normalverteilung nötig. |
| **Haversine-Formel** | H2 | Luftlinie zwischen zwei Koordinaten auf der Erdkugel. Kontrolle: Bellevue 9,8 km. |
| **Lineare Gerade auf log(Preis)** | H2 Faustregel | Steigung lässt sich als Prozent pro km lesen. |
| **Vergleich bei gleicher PLZ / Größe** | H1, H3 | Rechnet Störfaktoren (Lage, Größe) heraus, um den eigentlichen Effekt zu sehen. |

---

## Einschränkungen

- **Nur ein Winter** in den Daten (H1): Ob der Effekt jedes Jahr auftritt, ist offen. Außerdem lässt sich aus den Verkaufszahlen nicht trennen, ob im Winter das Angebot oder die Nachfrage sinkt.
- **Luftlinie statt Fahrzeit** (H2), keine Daten zu Schulen, Kriminalität oder Infrastruktur.
- **Grade** ist eine Einstufung des Bewertungsamts, keine Messung. Grade-7-Häuser sind im Median rund 30 Jahre älter (Baujahr 1963 gegenüber 1995). Das Alter wurde nicht herausgerechnet.
- H3 nutzt alle Häuser, nicht nur solche mit mind. 4 Schlafzimmern wie H1 und H2.
- Beim fairen Vergleich in H3 gibt es keine Bootstrap-Untergrenze, sondern Anteile (95 % im Plus, 65 % mit mind. 15 %).
- Es werden **Zusammenhänge** gezeigt, keine Ursachen. Marktdaten von 2014/15.
- Multivariat wird über Korrelationsmatrizen und das gezielte Herausrechnen von Störfaktoren gearbeitet, ein Regressionsmodell fehlt.

Mögliche nächste Schritte: multiple Regression, Fahrzeiten statt Luftlinie, Inseratsdaten für Angebot und Nachfrage, mehrere Jahre für die Saisonalität.

---

## Installation und Ausführung

Das Projekt wird mit [uv](https://docs.astral.sh/uv/) verwaltet. Python-Version und Abhängigkeiten stehen in `pyproject.toml`, die exakten Versionen in `uv.lock`.

```bash
uv sync
uv run jupyter lab
```

Die Notebooks erwarten die Ordner `data/` und `output/` im Projektverzeichnis. Zufallsprozesse (Bootstrap) verwenden `seed=42`, die Ergebnisse sind damit reproduzierbar.

---

## Lizenz

Siehe `LICENSE`.

## Tobias Kruse

[Tobias Kruse] · [Data Science Bootcamp - aieng-onl-de-210926] · [16.10.2026]
