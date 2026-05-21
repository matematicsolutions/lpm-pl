# Changelog

Wszystkie istotne zmiany w projekcie lpm-pl. Format: [Keep a Changelog](https://keepachangelog.com/), SEMVER.

## [Unreleased]

## [0.1.0-alpha] - 2026-05-21

### Added

- README.md z opisem produktu, instalacja, atrybucja upstream.
- LICENSE - Apache 2.0 z notka o cherry-picku z legalopsconsulting/lpm-skills.
- CONSTITUTION.md v1.0.0 - 7 artykulow + bramki MateMatic + roles + antygoals.
- SPEC.md v0.1.0-alpha - architektura skilla, szczegolowy spec dla status-report-drafter-pl, skrocony dla scope/risk.
- skills/status-report-drafter-pl/SKILL.md v0.1.0 - pierwszy skill MVP.
- skills/status-report-drafter-pl/templates/status_report_template.md - szablon raportu.
- docs/polskie-realia.md - notatki o roznicach vs common-law LPM (AFA, billing, success fee, RODO).
- examples/sample_status_report.md - zanonimizowany przyklad output.

### Architecture

- Cherry-pick patternu z [legalopsconsulting/lpm-skills](https://github.com/legalopsconsulting/lpm-skills) Apache 2.0 snapshot (2026-05-21).
- Skrocenie 11 H2 upstream do 8 H2 w SKILL.md (wyciecie M365 connected mode na faze 3, output templates do osobnego folderu, Memory Store wbudowane w kalibracje).
- Pozycjonowanie: osobne repo siostrzane wzgledem [Patrona](https://github.com/matematicsolutions/patron). Patron moze konsumowac opcjonalnie.

### Known limitations

- Brak walidacji na zywej (zanonimizowanej) sprawie. v0.1.0-alpha jest pre-production.
- Tylko 1 skill aktywny (status). Skille scope/risk w fazie 2.
- Brak integracji z M365 / SharePoint / Comarch IBARD / LEX Kancelaria - planowane na faze 3.
- Templates docx jeszcze nie sa rzeczywistymi .docx z brand szablonem - na razie markdown wzor.
