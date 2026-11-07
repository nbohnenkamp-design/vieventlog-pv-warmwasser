# ViEventLog PV-Warmwasser – Community-Prototyp V0.6.5

**Status: Planung und Sicherheitsdiagnose, noch keine produktive Warmwassersteuerung.**
Diese Community-Anwendung ist **getrennt** von der privaten produktiven
Zappi–ViEventLog-Bridge. Keine Dateien aus der produktiven Anwendung werden
bei Installation oder Test geändert.

## Was enthalten ist

- `konfigurator/index.html`: vollständig offline nutzbarer HTML-Konfigurator.
  Beispielkoordinaten Berlin sind Platzhalter und müssen vor echtem Einsatz
  ersetzt werden.
- `konfigurator/expr_tagesplan_v043_beispiel_one_time.expr`:
  ViEventLog-Tagesplan mit `get()`/`set()` **ausschließlich für den Skriptzustand**,
  als Beispiel auf Einmalladung eingestellt. Die vom HTML exportierte Fassung
  enthält die tatsächlich gewählte Methode und den eingestellten Standort.
- `src/`: Python-Planer, lesende ViEventLog-/Zappi-Adapter, sichere Vorschauen
  für die beiden Steuerungsverfahren und Testzustand.
- `tests/`: Python-, Node.js- und Chromium-Browsertests.
- `examples/`: synthetische Wetter-/Konfigurations-/Wochenplanbeispiele.

## Zwei unabhängig wählbare Verfahren

- **ViCare-Zeitprogramm** (`schedule`): Später zeitweilige Verschiebung des
  vollständig gesicherten WW-Wochenplans und kontrollierte Rückstellung.
- **Einmalige Warmwasserbereitung** (`one_time`): Später über ViEventLog
  `startDhwOneTimeCharge()`, sofern das Gerät dies tatsächlich unterstützt.

Beide Verfahren nutzen ausschließlich **Open-Meteo** als Wetterprognose. Wahlweise
können beim Hybridbetrieb echte Netzeinspeisungswerte (z. B. Zappi, evcc,
Home Assistant) den späteren PV-Frühstart bestätigen. Der Python-Zappi-Adapter
beherrscht HTTP Digest und liest nur; für die Zappi ist **kein zusätzlicher
Adapterdienst** erforderlich. Ein bestätigter Heizbetrieb kann optional und nur
bei aktuellen, plausiblen ViEventLog-Energiedaten angerechnet werden.

## Neu in V0.6.4: fester Tagesplan im ViEventLog-Skriptzustand

Der neue Expr-Export V0.4.3 berechnet beim **ersten zulässigen Lauf** einen
Tagesvorschlag und legt ihn unter `community_ww_plan_v043` ab.
Er speichert Tag, Methode, Startzeit, Konfiguration und getrennte Felder
`mainStatus` / `morningStatus`. Bei späteren Läufen bleibt die Startzeit fest;
eine neue Wetterprognose verschiebt sie nicht. Ein Wechsel der Steuerungsart oder
relevanter Parameter am selben Tag wird als Konflikt angezeigt. Ein älterer
unklarer Auftrag blockiert eine Neuplanung. Nach Ende des PV-Zeitfensters wird
kein neuer Tagesplan mehr ermittelt.

**Wichtig:** In ViEventLog wird `set()` **nur bei einem regulären Skriptlauf**
dauerhaft gespeichert. Im **„Testlauf (ohne Aktionen)“** ist die Speicherung nur
simuliert; ein unmittelbar wiederholter Testlauf sieht deshalb keinen alten
Plan. Wer die echte Persistenz prüfen will, kann das *diagnostische* Skript
anschließend einmal regulär laufen lassen: Es schreibt **nur Skriptzustand**,
nicht das ViCare-Zeitprogramm, nicht die WW-Solltemperatur und startet keine
Einmalladung. Im nächsten regulären Lauf sollte im Log `heutiger gespeicherter
Plan true` stehen. Einmal gesetzte interne Statusfelder werden derzeit noch
**nicht** durch eine physische Geräteüberwachung fortgeschaltet. `planned`
bedeutet ausdrücklich **nicht** `requested`, `running` oder `done`.

Bei einem ersten Test nach Ende des PV-Fensters wird keine Startzeit erstellt;
das ist beabsichtigt. Keine Cron-Automatik zur Gerätesteuerung aktivieren.

## Prüfung offline

Python 3.11+ und `requests` installieren. Von der Projektwurzel aus:

```bash
python3 -m pip install -r requirements.txt
PYTHONPATH=src python3 -m unittest discover -s tests -v
node tests/test_expr_export_v065.js
```

Für den optionalen Chromium-Test werden `playwright` und ein lokaler
Chromium-Browser benötigt; die Datei `tests/browser_ui_v065.py` verwendet
standardmäßig `/usr/bin/chromium`.

Reine Beispielplanung:

```bash
python3 src/ww_cli.py plan \
  --config examples/beispiel_konfiguration_einmalladung.json \
  --forecast-file examples/beispiel_openmeteo_berlin.json \
  --schedule-file examples/beispiel_wochenplan.json \
  --now '2026-11-07T10:00:00+01:00' \
  --pin-preview --data ./nur-test-daten
```

Der Python-Testplan `--pin-preview` ist eine **separate lokale
Testzustandsdatei**, kein ViCare-/ViEventLog-Status. Ein späterer Aufruf am
selben Tag darf den dort festgehaltenen Zeitpunkt nicht verschieben.
`--live-meter` liest auf ausdrücklichen Befehl ausschließlich aktuelle
Zappi-Messwerte; der CLI enthält **keine POST-Befehle**.

## Sicherheitsgrenzen

- Eine echte Warmwasserladung wurde mit dieser Version nicht ausgelöst oder
  nachgewiesen. Geräteseitige Schreibfähigkeiten müssen pro Anlage geprüft
  werden.
- Die Statusmaschine für spätere **echte** Aufträge, Bestätigung der
  Wärmebereitung, Wiederherstellung des Wochenplans und Crash-Recovery ist
  noch nicht mit Anlagenaktionen verbunden. Unbekannte Zustände dürfen nicht
  zum wiederholten Senden eines Befehls führen.
- **Keine API-Schlüssel, Zugangsdaten oder `.env`-Dateien ins Repository**.
  Solche Daten nur lokal als Umgebungsvariablen vorhalten.
- Die Beispieldaten in `examples/` sind keine realen Anlagendaten.

## Entwicklung und Veröffentlichung

Dieses Paket ist für die Versionierung in einem eigenständigen GitHub-Repository
`vieventlog-pv-warmwasser` vorbereitet. Es wurde durch die Paketerstellung
**noch nicht auf GitHub hochgeladen**. Vor einer öffentlichen Freigabe sollte
außerdem eine Lizenz bewusst ausgewählt und eine geräteseitige Testmatrix
abgearbeitet werden. Bis dahin nicht als produktive Steuerung verteilen.
