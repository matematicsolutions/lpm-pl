# Changelog

Wszystkie istotne zmiany w projekcie lpm-pl. Format: [Keep a Changelog](https://keepachangelog.com/), SEMVER.

## [Unreleased]

## [0.2.0-alpha] - 2026-05-21

### Added

- skills/scope-change-controller-pl/SKILL.md v0.1.0 - skill #2 MVP. Trzy klasy zadania (w zakresie / poza / niejasne), trzy outputy (notatka wewnetrzna / wniosek do klienta / decyzja wspolnika). Polskie realia: art. 16 KEA, konflikt interesow, brak AFA.
- skills/scope-change-controller-pl/templates/scope_change_request_template.md - szablon wniosku o rozszerzenie zakresu.
- skills/risk-and-issues-manager-pl/SKILL.md v0.1.0 - skill #3 MVP. RAID register (Risks / Assumptions / Issues / Decisions), dwuwymiarowa skala oceny (prawd. x wplyw), 11 typowych polskich ryzyk kancelaryjnych.
- skills/risk-and-issues-manager-pl/templates/raid_register_template.md - szablon rejestru RAID.
- examples/sample_scope_change_request.md - zanonimizowany przyklad wniosku o rozszerzenie zakresu.
- examples/sample_raid_register.md - zanonimizowany przyklad rejestru RAID.

### Changed

- README.md - aktualizacja "Co jest w MVP" - 3 skille aktywne (nie 1 + 2 placeholder), opis composability.
- README.md - dodanie roadmap (v0.3 - v1.0).

### Architecture

- Composability skilli: scope-change-controller -> risk-and-issues-manager (zmiana scope = nowe ryzyko); risk-and-issues-manager -> status-report-drafter (RAG sprawy moze sie zmienic).
- Cross-skill handoff points zdefiniowane explicite w sekcji 6 kazdego SKILL.md.

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
