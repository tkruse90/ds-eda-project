# Vorlage für das EDA-Projekt King County Housing

Dies ist die Startvorlage für das Projekt zur explorativen Datenanalyse (EDA). Du arbeitest mit dem King-County-Immobiliendatensatz (Hausverkäufe in und um Seattle, USA), findest heraus, was die Hauspreise bestimmt, und machst aus deinen Ergebnissen Erkenntnisse und Empfehlungen für eine Kundin oder einen Kunden deiner Wahl.

## Lernziele

Nach Abschluss dieses Repositorys solltest du in der Lage sein:

- Dich aus Python mit einer PostgreSQL-Datenbank zu verbinden und Abfrageergebnisse in einen pandas-DataFrame zu laden.
- Eine explorative Datenanalyse an klaren Forschungsfragen und Hypothesen auszurichten.
- Einen realen Datensatz zu bereinigen und aufzubereiten, indem du fehlende Werte, Ausreißer und Transformationen von Merkmalen behandelst.
- Verteilungen sowie die Zusammenhänge zwischen Merkmalen und der Zielvariable (Preis) zu erkunden.
- Deine Analyse in mindestens drei Erkenntnisse und drei auf die Kundschaft zugeschnittene Empfehlungen zu übersetzen.
- Deine Arbeit einem Publikum ohne technischen Hintergrund zu präsentieren.

## Lernpfad

Bearbeite die Dateien der Reihe nach. Beginne mit der Aufgabenstellung, um das Ziel zu verstehen, nutze den Workflow als Leitfaden, hol dir die Daten und führe dann deine Analyse im EDA-Notebook durch.

> [!TIP]
> Die Daten liegen im Schema **eda** der Datenbank und sind auf zwei Tabellen verteilt. Bevor du irgendetwas im Code abrufst, verbinde dich mit DBeaver und erkunde dieses Schema: Sieh dir beide Tabellen an, schlag in den [**Spaltennamen**](column_names.md) nach, was die einzelnen Felder bedeuten, und finde heraus, wie du sie verbinden kannst. Sobald du einen funktionierenden `JOIN` hast, verwende ihn als Abfrage in [**03 - Daten abrufen**](03_fetching_the_data_eda_de.ipynb), um den kombinierten Datensatz in pandas zu laden.

| Datei / Ordner                                                  | Beschreibung                                                                                                             |
| --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| [**01 - Aufgabenstellung**](01_assignment_de.md)                | Das Projekt-Briefing: der Datensatz, deine Aufgaben, die Ergebnisse und die Liste der Kundinnen und Kunden zur Auswahl.  |
| [**02 - Workflow**](02_workflow_de.md)                          | Ein empfohlener EDA-Workflow, vom Verstehen und Hinterfragen der Daten über Bereinigung und Zusammenhänge bis zur Präsentation. |
| [**03 - Daten abrufen**](03_fetching_the_data_eda_de.ipynb)     | Mit psycopg2 und SQLAlchemy eine Verbindung zur PostgreSQL-Datenbank herstellen und die Daten in einen pandas-DataFrame laden. |
| [**04 - EDA**](04_eda_de.ipynb)                                 | Starter-Notebook für deine explorative Datenanalyse.                                                                     |
| [**Spaltennamen**](column_names.md)                             | Datenwörterbuch, das jede Spalte des King-County-Immobiliendatensatzes beschreibt.                                       |

### Weitere Ordner und Dateien

| Datei / Ordner                          | Beschreibung                                                                                   |
| --------------------------------------- | ---------------------------------------------------------------------------------------------- |
| [**Daten**](data/)                      | Hier speicherst du die CSV des Datensatzes. Der Ordner wird versioniert, die Datendateien darin aber nicht. |
| [**.env.example**](.env.example)        | Vorlage für die Datenbank-Zugangsdaten. Kopiere sie nach `.env` und trage deine Werte ein.    |
| [**pyproject.toml**](pyproject.toml)    | Projektkonfiguration und Abhängigkeiten.                                                       |
| [**uv.lock**](uv.lock)                  | Lock-Datei der Abhängigkeiten.                                                                 |

## Einrichtung

> [!NOTE]
> In diesen Schritten ist Text in spitzen Klammern wie `<repo-name>` ein **Platzhalter**. Ersetze ihn einschließlich der Klammern `< >` durch deinen eigenen Wert. Aus `cd <repo-name>` wird zum Beispiel `cd ds-eda-project-template`.

### 1. Das Repository aus der Vorlage erstellen

Klicke auf GitHub auf **Use this template**.

Beim Erstellen des Repositorys:

- Lege dich selbst als **Owner** fest
- Wähle einen Namen für das Repository
- Deaktiviere **Include all branches**
- Klicke auf **Create repository**

> [!IMPORTANT]
> Wenn ihr zu zweit oder in Gruppen arbeitet, sollte nur **eine Person** diesen Schritt ausführen.
---

### 2. Mitwirkende hinzufügen (nur bei Paaren/Gruppen)

Wenn du mit Teammitgliedern arbeitest:

1. Öffne das Repository auf GitHub
2. Gehe zu **Settings → Collaborators**
3. Füge deine Teammitglieder als Collaborators hinzu
4. Teile den Link zum Repository mit deinem Team

Die Teammitglieder sollten die Einladung annehmen, bevor es weitergeht.

---

### 3. Das Repository klonen

Kopiere über den Button **Code** auf GitHub die SSH-URL und führe dann Folgendes aus:

```bash
git clone <copied-ssh-url>
```

Die kopierte SSH-URL sieht etwa so aus: `git@github.com:<your-username>/<repo-name>.git`.

---

### 4. In den Projektordner wechseln und Abhängigkeiten installieren

Damit werden alle Abhängigkeiten installiert und eine virtuelle Umgebung in `.venv/` erstellt.

```bash
cd <repo-name>
uv sync
```

> [!TIP]
> Du brauchst eine Bibliothek, die noch nicht installiert ist (zum Beispiel für Karten)? Füge sie mit `uv add <package-name>` hinzu. Dadurch werden `pyproject.toml` und `uv.lock` aktualisiert und die Bibliothek in deine `.venv` installiert. Committe beide Dateien; deine Teammitglieder führen nach dem Pullen `uv sync` aus, um dieselbe Umgebung zu erhalten.

---

### 5. Deine Datenbank-Zugangsdaten einrichten

Das Notebook zum Datenabruf liest die Verbindungsdaten der Datenbank aus einer `.env`-Datei. Kopiere die Vorlage und trage deine eigenen Werte ein:

```bash
cp .env.example .env
```

Öffne `.env` und ersetze die Platzhalter durch die Zugangsdaten für die King-County-Immobiliendatenbank (dieselben, die du in DBeaver verwendest). Diese Werte werden in [**03 - Daten abrufen**](03_fetching_the_data_eda_de.ipynb) verwendet.

> [!CAUTION]
> `.env` enthält Geheimnisse und darf niemals committet werden. Die Datei ist bereits in `.gitignore` aufgeführt. Nur `.env.example` mit Platzhalterwerten gehört ins Repository.

---

### 6. Die Notebooks öffnen

> [!NOTE]
> Öffne VS Code unbedingt im Stammverzeichnis des Projekts, damit die von uv sync erstellte Umgebung automatisch erkannt wird.

Starte VS Code im Stammverzeichnis des Projekts:

```bash
code .
```

Öffne dann ein Notebook und wähle die von `uv sync` erstellte Python-Umgebung als Kernel aus.

## Quellen & weiterführende Literatur

- [**House Sales in King County dataset**](https://www.kaggle.com/datasets/harlfoxem/housesalesprediction): Der Quelldatensatz mit Spaltenbeschreibungen und Notebooks aus der Community.
- [**Pandas user guide**](https://pandas.pydata.org/docs/user_guide/index.html): Der offizielle Leitfaden zur Datenbearbeitung mit pandas.
- [**Seaborn tutorial**](https://seaborn.pydata.org/tutorial.html): Statistische Datenvisualisierung in Python.
- [**SQLAlchemy documentation**](https://docs.sqlalchemy.org/en/20/): Das Datenbank-Toolkit, mit dem PostgreSQL aus Python abgefragt wird.
- [**Hypothesis generation for EDA**](https://www.analyticsvidhya.com/blog/2020/11/an-efficient-way-of-performing-eda-hypothesis-generation/): Wie man Forschungsfragen und Hypothesen formuliert, bevor man in die Daten eintaucht.
- [**EDA Checklist**](https://github.com/neuefische/datascience-infographics/blob/main/EDA_Checklist.md): Eine Checkliste, die eine explorative Analyse Phase für Phase begleitet.
- [**Detailed EDA with Python**](https://www.kaggle.com/code/ekami66/detailed-exploratory-data-analysis-with-python): Ein ausgearbeitetes Beispiel für ein gründliches EDA-Notebook mit realen Daten.
- [**Tips for data science presentations**](https://www.dataknowsall.com/storytelling.html): Storytelling-Techniken, um Ergebnisse einem Publikum ohne technischen Hintergrund zu präsentieren.
