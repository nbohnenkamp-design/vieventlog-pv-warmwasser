# Prüfstand V0.6.5

- Python-Unit-Tests: 58 Tests erfolgreich, reproduzierbar mit `PYTHONPATH=src python3 -m unittest discover -s tests -v`.
- Node.js: HTML-Expr-Export aus echten Konfiguratorparametern für beide
  Verfahren, mit `get()`/`set()` nur im Skriptzustand und ohne Geräteaktionen.
- Chromium: HTML mit echten Browser-Ereignissen; Desktop/Mobilansicht,
  Methodenwechsel, JSON- und Expr-Ausgabe ohne JS-Fehler.
- ViEventLog **V0.4.2** wurde am 07.11.2026 vom Anwender auf der echten
  Installation als Testlauf bestätigt (Open-Meteo, Temperaturwerte, Zeit).
- **V0.4.3 noch nicht auf einem echten ViEventLog-System getestet.**
- Der Python-Zeitplanmotor wurde bisher nur mit Testadaptern auf Schreib-
  und Wiederherstellungsverhalten geprüft. Keine reale Anlage geändert.

- V0.6.5: Im Browser wurde die einzige Methodenwahl im Warmwasser-Schritt
  und das Fehlen eines zweiten Dropdowns in „Weitere Einstellungen“ geprüft.
