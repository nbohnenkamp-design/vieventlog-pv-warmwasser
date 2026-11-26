# Prüfung und Grenzen — V0.6.21

**Stand: 26.11.2026.** Hier werden ausgeführte Prüfungen und offene Punkte bewusst getrennt.

## Nachgewiesen
- HTML-/JavaScript-Syntax mit Node.js geprüft.
- Chromium-Browsertest des offline eingebetteten HTML-Konfigurators auf Desktop- und Smartphone-Breite: keine JavaScript-Seitenfehler.
- Erzeugung des Expr-Skripts, geänderter Startversatz, andere Mindeststrahlung und eigene Koordinaten im Export überprüft.
- JSON-Profil exportiert, wieder importiert, verändert Werte wiederhergestellt; ungültiger Import abgelehnt.
- Ursprüngliches V0.6.21-ZIP inklusive aller dokumentierten SHA-256-Dateiprüfsummen geprüft.
- **Nutzerseitige native ViEventLog-Testläufe vom 19. und 20.11.2026** wurden als „✓ erfolgreich“ gemeldet; dort wurden Wetterwerte, persistierte Tagespläne und gesperrte Anlagenaktionen angezeigt. Die vom Nutzer übermittelten Auszüge sind **Trockenläufe**, keine Live-Warmwasseraktionen.

## Bei der Veröffentlichungsprüfung korrigiert
- Das mitgelieferte Beispielskript wurde an die **tatsächlichen Standardwerte des aktuellen HTML-Exports** angeglichen (u. a. optionale Messpfadangabe und Schreibweise von Beispielkoordinaten). Keine Änderung am generierenden Programm oder der Steuerlogik.
- Veraltete und widersprüchliche Stellen in README und Testdokumentation wurden ersetzt. Die V0.6.19-Prüfung „beide Ladestatusmerkmale zwingend lesbar“ gilt **nicht** mehr für V0.6.21.
- Historische Python/Webapp-Dateien und alter V0.6.5-Importworkflow wurden aus dem aktuellen Repository-Baum entfernt; Git-Verlauf bleibt erhalten.

## Nicht nachgewiesen
- Kein vollständiger Live-Ladezyklus mit echter Viessmann-Kommandokette `setDhwTemperature` → `startDhwOneTimeCharge` → Rückstellung des ursprünglichen Sollwerts.
- Keine Messung der Verdichter-Taktzahl oder der behaupteten Vorteile eines regulären ViCare-Zeitfensters.
- Keine End-to-End-Verifikation der unbekannten Geräte-Zustandsmerkmale und der Rückstellung bei API-Ausfall.
- Kein unabhängiger Nachweis, wie viele Viessmann-Cloudaufrufe **alle** parallelen Anwendungen zusammen erzeugen.
- Kein verifizierter 24-Stunden-Log-Export sämtlicher regulärer Cron-Läufe; nur die vorgelegten Einzelprotokolle.
- **Zeitprogrammverschiebung über Expr ist nicht implementiert.**
- Die endgültige Entscheidung über eine öffentliche Weiterverwendungslizenz steht aus.

**Verwendung:** Als nachvollziehbar dokumentierten Beobachtungs-Teststand zeigen; nicht als vollständig freigegebene Live-Steuerung bezeichnen.
