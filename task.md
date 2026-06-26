# Agentic System: Anforderungsanalyse & Implementierung

Dieses Dokument beschreibt die Architektur und die Rollenverteilung für ein Agenten-System, das mit der Analyse, Planung und Umsetzung einer in `idee.md` beschriebenen Anwendung beauftragt ist. Das System folgt einem Orchestrator-Sub-Agent-Muster, wobei die Kommunikation über Markdown-Dateien in einer definierten Verzeichnisstruktur abläuft.

## Verzeichnisstruktur für die Kommunikation

Die Arbeitsergebnisse und Arbeitsanweisungen werden in folgenden, an den Meilensteinen orientierten Ordnern abgelegt:

- `/01_Requirements/` - Anforderungsanalyse, Refinement und finales Anforderungsdokument.
- `/02_Tech_Stack/` - Recherche, Evaluierung und Auswahl des Technologiestacks.
- `/03_Implementation_Plan/` - Architektur, Schnittstellen und detaillierter Implementierungsplan.
- `/04_Implementation/` - Generierter Code und Testberichte.
- `/Inbox/` - Eingehende Nachrichten vom Benutzer (z.B. Antworten auf Unklarheiten).

## Rollen und Agenten

### 1. Orchestrator Agent (Der Projektmanager)

**Rolle:** Der Orchestrator steuert den gesamten Prozess, analysiert die Ergebnisse der Sub-Agenten, löst Konflikte, hält den Kontakt zum Benutzer für Rückfragen (nur in Phase 1) und erstellt die Aufgaben (Tasks) für die Sub-Agenten.
**Aufgaben:**

- Initiales Lesen von `idee.md`.
- Erstellen von initialen Task-Dateien (`.md`) für die Sub-Agenten in den entsprechenden Meilenstein-Ordnern.
- Kontinuierliche Überwachung der erzeugten Ausgabedateien der Sub-Agenten.
- Zusammenführen der Teilergebnisse (z.B. Usability-Analyse und Tech-Stack-Auswahl) in konsistente Dokumente.
- Identifikation von Unklarheiten während der Anforderungsphase (Phase 1) und Formulierung von Rückfragen an den Benutzer (Ablage in `/Inbox/fragen_an_nutzer.md`). Verarbeiten der Antworten.
- Entscheidung, wann ein Meilenstein abgeschlossen ist und der nächste beginnt.

### 2. Usability & Requirements Agent (Sub-Agent A)

**Rolle:** Der UX/UI-Analyst und Requirements Engineer. Fokussiert sich auf die Nutzerzentrierung und die Vermeidung von Sackgassen.
**Aufgabe:**

- Detaillierte Analyse der `idee.md` aus der Perspektive eines Endnutzers.
- Identifikation möglicher Usability-Probleme, logischer Fehler im Workflow oder "Sackgassen".
- Iteratives Verfeinern der Anforderungen.
- Erstellen von User Stories und Akzeptanzkriterien.
- Ausgabe: Detaillierte Analyseberichte in `/01_Requirements/`.

### 3. Technology & Architecture Agent (Sub-Agent B)

**Rolle:** Der Software-Architekt und Technologie-Scout. Verantwortlich für einen modernen, stabilen und kohärenten Tech-Stack.
**Aufgabe:**

- Online-Recherche aktueller, moderner Technologien passend zu den (sich entwickelnden) Anforderungen.
- Auswahl eines Tech-Stacks (Frontend, Backend, Datenbank, Hosting, etc.).
- Kritische Evaluierung des Zusammenspiels der gewählten Technologien (Eignung, Stabilität, Mehrwert, Over-Engineering vermeiden).
- Ausgabe: Tech-Stack-Empfehlungen und Architekturskizzen in `/02_Tech_Stack/`.

### 4. Implementation Planner Agent (Sub-Agent C)

**Rolle:** Der Technical Lead. Übersetzt Anforderungen und Tech-Stack in einen konkreten Bauplan.
**Aufgabe:**

- Startet erst, wenn Phase 1 und Phase 2 abgeschlossen sind (Signal durch Orchestrator).
- Übersetzt das finale Anforderungsdokument und die Architektur in einen Schritt-für-Schritt Implementierungsplan.
- Definiert Datenmodelle, API-Schnittstellen (Verträge) und Komponentenstrukturen.
- Ausgabe: Detaillierter Implementierungsplan in `/03_Implementation_Plan/`.

### 5. Coding Agent (Sub-Agent D)

**Rolle:** Der Entwickler. Setzt den Plan in Code um.
**Aufgabe:**

- Startet erst, wenn Phase 3 abgeschlossen ist.
- Setzt die Schritte des Implementierungsplans sukzessive um.
- (Hinweis für die reale Umsetzung: Dieser Agent würde den tatsächlichen Code generieren. Im Kontext dieser Definition erzeugt er Code-Dateien in `/04_Implementation/`).

## Phasenablauf (Workflow)

### Phase 1: Anforderungsanalyse & Tech-Stack Evaluierung (Iterativ & Parallel)

- **Start:** Orchestrator liest `idee.md` und erstellt initiale Tasks für Agent A und B.
- **Agent A (Usability):** Analysiert die Idee, sucht nach Logiklücken und Usability-Problemen. Erstellt `/01_Requirements/usability_report_v1.md`.
- **Agent B (Tech):** Recherchiert parallel mögliche Stacks basierend auf der groben Idee. Erstellt `/02_Tech_Stack/tech_eval_v1.md`.
- **Orchestrator (Review & Refinement):**

- Liest Berichte v1.
- Prüft auf Widersprüche (z.B. Agent A fordert Offline-Fähigkeit, Agent B wählt eine Technologie, die das nicht gut unterstützt).
- **Nutzer-Interaktion:** Falls Agent A oder der Orchestrator unlösbare Unklarheiten in `idee.md` finden, generiert der Orchestrator `/Inbox/fragen_an_nutzer.md`.
- Nach Erhalt der Antworten (oder zur weiteren Verfeinerung) erstellt der Orchestrator neue Tasks für v2.
- **Ende Phase 1:** Dieser Zyklus wiederholt sich, bis der Orchestrator aus den Ergebnissen von Agent A und B ein finales Dokument `/01_Requirements/final_requirements_and_techstack.md` erstellt hat und keine Fragen an den Nutzer mehr offen sind.

### Phase 2: Implementierungsplanung

- **Start:** Orchestrator beauftragt Agent C mit dem Plan.
- **Agent C (Planner):** Liest `/01_Requirements/final_requirements_and_techstack.md`. Erstellt detaillierte API-Definitionen, Datenbankschemata und eine Komponenten-Roadmap.
- **Ende Phase 2:** Orchestrator reviewt den Plan und gibt ihn als `/03_Implementation_Plan/master_plan.md` frei.

### Phase 3: Umsetzung

- **Start:** Orchestrator beauftragt Agent D mit Schritt 1 aus dem `master_plan.md`.
- **Agent D (Coder):** Schreibt den Code und meldet Vollzug (oder Fehler).
- **Ende Phase 3:** Der Orchestrator steuert diesen Prozess Schritt für Schritt bis zur Fertigstellung.