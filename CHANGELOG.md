# Änderungsprotokoll

## V0.6.5 – 07.11.2026
- Doppeltes Dropdown „Warmwasser-Auslösung“ aus „Weitere Einstellungen“ entfernt.
  Die Auswahlkarten unter „Warmwasser“ bleiben die einzige Bedienmöglichkeit.
- Irritierenden sichtbaren Verweis „Vollständige Einstellungen aus V0.4.47“ gestrichen.
- Export/Validierung unverändert; beide Verfahren werden weiter unterstützt.
- Browsertest gegen doppeltes Bedienelement ergänzt.

## V0.6.4 – 07.11.2026
- HTML-Konfigurator exportiert ViEventLog Expr V0.4.3 mit festem Tagesplan.
- `get()`/`set()` nur für den ViEventLog-Skriptzustand; keine Anlagenaktionen.
- Tagesplan einmal pro Kalendertag, mit unveränderlicher Startzeit und
  getrenntem Haupt-/Morgenstatus sowie Erkennung von Methoden-/Konfigurationskonflikten.
- Alte ungeklärte Geräteaufträge blockieren spätere Neuplanung.
- Keine Wetterabfrage mehr bei vorhandenem Tagesplan oder nach PV-Fensterende.
- Neu strukturierte GitHub-bereite Quelltexte mit `konfigurator/`, `src/`,
  `tests/`, `examples/`.

## V0.6.3 – 07.11.2026
- Optionale lokale Python-Vorschau `--pin-preview` hält Startzeit am selben Tag fest.
- Lesender Expr-Prognoseplaner V0.4.2 mit korrigiertem Tagesbezug.
- Auswahl zwischen Zeitprogramm und Einmalladung.

## V0.6.1 – 07.11.2026
- Echte, ausschließlich lesende myenergi-Zappi-Digest-Abfrage.
- Plausibilitäts- und Frischeprüfungen für optionalen elektrischen Heizbonus.

## V0.6.0 – 07.11.2026
- Offline-HTML-Konfigurator, Open-Meteo-Prognose, sicherer Zeitplan-Testmotor.
