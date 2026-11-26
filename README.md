# PV-Warmwasser für ViEventLog — V0.6.21 (Beta)

**Status:** Eigenständiger, offline verwendbarer HTML-Konfigurator für ein ViEventLog-Expr-Skript. Der Beobachtungsmodus wurde durch erfolgreiche native ViEventLog-Testläufe mit realen Geräte-Lesewerten bestätigt. **Ein vollständiger Live-Ladezyklus mit anschließender Sollwert-Rückstellung wurde noch nicht nachgewiesen.** Keine Freigabe als unbeaufsichtigt geprüfte Produktionssteuerung.

## Was enthalten ist

- **`konfigurator/index.html`**: HTML-Datei lokal im Browser öffnen. Keine Webapp, kein Docker, kein Python und keine Serverinstallation erforderlich.
- **`beispiele/V0.6.21_Berlin_BEISPIEL_NUR_BEOBACHTEN.expr`**: Vom Konfigurator erzeugtes Beispiel mit *Beispielkoordinaten Berlin* und gesperrtem Livebetrieb.
- **`simulator/index.html`**: Optionaler, separater Tages-Simulator mit nachgestellten Daten; keine echte Anlagensteuerung.
- **`TESTERGEBNISSE.md`**: Was tatsächlich geprüft wurde und was offen ist.

**Derzeit implementiertes Startverfahren:** native ViEventLog-**Einmalladung** über `startDhwOneTimeCharge()`, mit vorherigem Setzen und nachträglichem Wiederherstellen des WW-Sollwerts. **Eine echte Verschiebung des ViCare-Warmwasser-Wochenprogramms ist NICHT implementiert.** Die früher vorgesehene Wahl zwischen beiden Verfahren ist für spätere Entwicklung offen. Der reguläre ViCare-Betrieb kann bei gleichzeitigem Heizbedarf möglicherweise günstiger für den Verdichterübergang sein; ein Vorteil ist nicht nachgewiesen.

## Inbetriebnahme nur zur Beobachtung

1. `konfigurator/index.html` lokal öffnen, Standort, Ausrichtung, PV-Fenster, Temperaturen und Mindeststrahlung eintragen.
2. Über **Einstellungen speichern** eine JSON-Datei anlegen. Für spätere Änderungen mit **Einstellungen laden** wieder öffnen. Die JSON-Datei nicht öffentlich teilen: Sie kann Standort und interne Netzwerkadressen enthalten.
3. Skript erzeugen und in ViEventLog bei der passenden Anlage speichern. Bei Aktualisierungen dieselbe **Skript-ID** behalten, damit gespeicherte Zustände nicht ohne Prüfung verloren gehen.
4. **Testlauf (ohne Aktionen)** durchführen; danach für reguläre Beobachtung einschalten.
5. Cron: **`*/5 * * * *`** (alle 5 Minuten). Unter diesen Einstellungen erfolgt **keine** Geräteaktion:

```expr
let WW_TESTMODUS = true;
let WW_LIVE_FREIGABE = false;
let WW_AUTOMATIK = true;
```

Im ViEventLog-Skript unter **Protokoll** die Option **„auch Läufe ohne Änderung“** aktivieren, um alle Cron-Ausführungen und ihre Logmeldungen zu sehen. **`Ergebnis false` ist das normale Ende des Skripts, kein Fehler**, wenn ViEventLog zugleich „✓ erfolgreich“ meldet.

### Ablauf

- Das Skript holt die prognostizierte Sonneneinstrahlung von **Open-Meteo** und sucht innerhalb des eingestellten PV-Zeitfensters nach einer geeigneten Stunde oberhalb der Mindestschwelle.
- Der Startzeitversatz ist von **−60 bis +60 Minuten in 10-Minuten-Schritten** einstellbar, Standard **−20 Minuten**. Das PV-Startfenster begrenzt frühere Starts.
- Rückfall `latest`: wenn keine geeignete Stunde verbleibt, ist der **späteste PV-Zeitpunkt** vorgesehen (z. B. 16:00 Uhr); `hold`: kein Ersatzstart.
- Zusätzlich ist eine konfigurierbare Morgenreserve vorgesehen.
- Bei optionaler echter Netzleistungsmessung kann eine ausreichend lange, zeitlich belegte Einspeisung vorziehen. Ohne verlässlichen Sensor-Zeitstempel erfolgt **kein** Frühstart auf Basis dieser Messung.
- Stündliche Werte im Log sind die **durchschnittliche Einstrahlung der jeweils vorhergehenden Stunde in W/m²**, keine elektrische PV-Leistung. Bei 12:00 in den Open-Meteo-Rohdaten ist die Stunde 11:00–12:00 gemeint.

## Sicherheit und Grenzen

- **Nicht einfach Live freigeben.** Die echten Viessmann-Steuerbefehle und ihre Rückstellung wurden für V0.6.21 nicht an einer laufenden Wärmepumpe durchgehend erprobt.
- Bei einer unbekannten Antwort wird der Auftrag gesperrt statt blind wiederholt. Die Verfügbarkeit von `heating.dhw.oneTimeCharge.active` und `heating.dhw.charging.active` hängt vom Gerät ab. **Ein fehlendes Merkmal allein sperrt seit V0.6.20 den Start nicht**, ein explizit laufender Ladestatus dagegen schon. Das verbleibende Risiko ist vor dem Livebetrieb gerätespezifisch zu bewerten.
- Wird Livebetrieb bei offenem Auftrag ausgeschaltet, kann auch die automatische Rückstellung des zuvor geänderten WW-Sollwerts stehen bleiben. In diesem Fall zuerst die Anlage kontrollieren; Zustände nicht blind löschen.
- Bei Livebetrieb darf **keine zweite unabhängige Warmwasser-Automatik** parallel regeln. Eine separate Raumheizungsautomatik ist im Beobachtungsmodus unproblematisch.
- Diese Version verwendet für Datums-/Zeitberechnungen **Europe/Berlin**. Die eingetragenen Koordinaten allein ändern die Zeitzone nicht.
- Bei aktivem `latest` kann auch an einem bewölkten Tag eine Ladung nahe dem PV-Fensterende geplant werden.
- Für Home Assistant müssen eventuell erforderliche Tokens **nur lokal im erzeugten Skript** ergänzt werden. Niemals eigene Tokens, Skripte mit Zugangsdaten oder persönliche JSON-Einstellungsprofile ins Forum/GitHub hochladen.
- Ohne eine explizit festgelegte Lizenzdatei ist die rechtliche Nachnutzung noch nicht geregelt.

### API-Nutzung

ViEventLog ruft Open-Meteo **nicht** bei jedem Fünf-Minuten-Cron auf: Tagesplan-Aktualisierung wird zeitlich begrenzt (üblicherweise 120 Minuten, nahe am geplanten Termin frühestens nach 60 Minuten); die reine Abendanzeige frühestens alle vier Stunden. Optionale externe Netzleistungsabfragen können dagegen im PV-Fenster bei jedem Fünf-Minuten-Lauf erfolgen. Viessmann-Gerätewerte werden über den gemeinsamen ViEventLog-Cache gelesen, soweit aktuelle Werte vorhanden sind. **Eine feste Obergrenze für alle Viessmann-Cloudaufrufe sämtlicher parallel laufender Anwendungen ist damit nicht garantiert.**

## Technische Quellen

- [ViEventLog: Skripte und Funktionen](https://github.com/mschneider82/vieventlog/blob/main/docs/SCRIPTS.md)
- [Open-Meteo: API-Dokumentation, Einstrahlung und Stundenbezug](https://open-meteo.com/en/docs)

## Projektstand

V0.6.21 ist ein **Community-Testkandidat mit nachgewiesenen Trockenläufen**, kein als Livebetrieb freigegebenes Fertigprodukt. Änderungsverlauf und bekannte Einschränkungen: `CHANGELOG.md` und `TESTERGEBNISSE.md`.
