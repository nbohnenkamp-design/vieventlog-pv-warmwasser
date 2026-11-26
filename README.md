# PV-Warmwasser für ViEventLog

**V0.6.21 (Beta)** – Ein HTML-Konfigurator, der ein eigenständiges ViEventLog-Skript zur PV-orientierten Warmwasserbereitung erzeugt. Die Planung nutzt Open-Meteo, optional auch Netzleistungsmesswerte. Kein Docker, kein Python, keine Webapp.

**[HTML-Konfigurator herunterladen](https://raw.githubusercontent.com/nbohnenkamp-design/vieventlog-pv-warmwasser/main/konfigurator/index.html)**

## Verwendung

1. HTML-Datei herunterladen und lokal im Browser öffnen.
2. Anlage und gewünschte Einstellungen eintragen. Einstellungen als JSON-Datei sichern, dann das Expr-Skript erzeugen.
3. Skript in ViEventLog einfügen und einen **Testlauf ohne Aktionen** ausführen.
4. Für den Beobachtungsbetrieb Cron **`*/5 * * * *`** einstellen und folgende Werte beibehalten:

```javascript
let WW_TESTMODUS = true;
let WW_LIVE_FREIGABE = false;
let WW_AUTOMATIK = true;
```

Die regulären Läufe findest du in ViEventLog unter **Protokoll → auch Läufe ohne Änderung**.

## Stand und Hinweise

- Aktuell ist **Einmalladung** implementiert. Die **Verschiebung des ViCare-Wochenprogramms** ist noch nicht verfügbar.
- Beobachtungs- und Trockenläufe wurden getestet. **Ein vollständiger Live-Ladezyklus einschließlich Sollwert-Rückstellung ist noch nicht nachgewiesen.**
- Bei späterem Livebetrieb keine zweite unabhängige Warmwasser-Automatik parallel betreiben. Einen offenen oder unklaren Auftrag nicht blind zurücksetzen.
- Eigene JSON-Profile oder Skripte mit Zugangsdaten nicht veröffentlichen.

**Lizenz:** [MIT](LICENSE). Technische Grundlage: [ViEventLog](https://github.com/mschneider82/vieventlog).
