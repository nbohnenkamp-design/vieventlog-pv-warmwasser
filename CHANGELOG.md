# Änderungsverlauf

## Veröffentlichungshinweis — 26.11.2026
- Projekt mit MIT-Lizenz versehen, entsprechend ViEventLog; eigener Urhebervermerk.
- Dokumentationshinweise zur Lizenz aktualisiert, keine Änderung der Steuerlogik.

## V0.6.21 — aktueller Community-Teststand
- Bereits gespeicherte Sonnenpläne enthalten nun Sonnenstundenbeginn und angewendeten Startversatz, um zukünftige Terminänderungen nachvollziehbar zu machen.
- Geänderte Startverschiebung: noch ausstehende Sonnenpläne werden aus gespeicherter Berechnungsgrundlage umgerechnet; ältere Pläne werden nur im erlaubten Zeitfenster über begrenzte Wetter-Neuabfragen aktualisiert.
- Verstrichene Starttermine werden nicht nachgeholt.
- Kein neues Startverfahren; die echte Verschiebung des ViCare-Wochenprogramms fehlt weiterhin.

## V0.6.20
- Einstellbarer Startversatz (−60 bis +60 Minuten in 10-Minuten-Schritten, Standard −20).
- Begrenzte Auffrischung der Open-Meteo-Prognose vor der geplanten Ladung.
- Ein fehlendes Ladestatusmerkmal führt allein nicht mehr zur permanenten Startsperre; ein bestätigter aktiver Ladevorgang verhindert Doppelstarts.
- Verbesserte Diagnose und keine blinde Wiederholung unklarer Aufträge.

## V0.6.19
- Warmwasser-Betriebsart wird gelesen, optionale PV-Netzmessungen erfordern aktuellen Quell-Zeitstempel.
- Warnung bei offenen Aufträgen und beim Verpassen des Ladefensters.

## V0.6.17–V0.6.18
- Lesbare Sonnenprognose für heute/morgen auch nach dem PV-Fenster und kurze Zusammenfassung der besten morgigen Sonnenstunde.

## V0.6.15–V0.6.16
- JSON-Einstellungen speichern/laden; dokumentierter Einstellungsblock im generierten Skript.
- Koordinaten gemeinsam oder getrennt eingeben; Schnell-Simulation aus dem Einrichtungsablauf entfernt.

**Hinweis:** Die frühen Versionen enthielten zeitweise Python/Webapp-Komponenten und eine Auswahl „Zeitprogramm/Einmalladung“. Diese ältere Auswahl war noch keine vollständige native Expr-Zeitprogrammsteuerung; die aktuellen Dateien sind eine eigenständige, reduzierte Architektur.
