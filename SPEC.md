# SPEC.md - specyfikacja MVP lpm-pl

**Wersja**: 0.2.0-alpha
**Data**: 2026-05-21
**Zakres**: 3 skille MVP - wszystkie z pelnymi SKILL.md + templates + examples.

---

## Architektura skilla (template dla wszystkich 3)

Kazdy SKILL.md ma identyczna szkielet 8 sekcji H2:

1. **Kiedy uzywac** - triggery (frazy uzytkownika), kiedy NIE uzywac.
2. **Metodyka rdzenia** - 3-5 zasad ktorymi skill sie kieruje (judgment calls).
3. **Wejscie** - co skill przyjmuje (email-first, opcjonalnie zalaczniki).
4. **Wyjscie** - format pliku, struktura sekcji, sciezka zapisu.
5. **Workflow** - 5-6 krokow od inputu do gotowego pliku.
6. **Cross-skill handoff** - czyta od / pisze do innych skilli.
7. **Kalibracja per sprawa** - jak skill uczy sie preferencji kancelarii (plik `[Sprawa]_LPM_Memory.md`).
8. **Antywzorce** - czego skill NIE robi.

Zycia kazdego skilla wymaga: SKILL.md + folder `templates/` (szablony output) + przyklad w `examples/`.

## Skill #1 - status-report-drafter-pl

**Cel**: prawnik/partner wkleja watek mailowy ze sprawy (lub wskazuje folder), skill produkuje gotowy do wyslania status raport .docx z naglowkiem kancelarii.

### Triggery

Frazy: "zrob status dla sprawy [X]", "raport tygodniowy [klient]", "podsumuj sprawe [X] dla wspolnika", "zaktualizuj status [matter]", "co sie dzialo na [klient] w tym tygodniu".

### Wejscie

- **Pierwotne**: wklejony watek mailowy / korespondencja z klientem / notatki ze spotkan / transkrypcja telefonu (Whisper/Otter).
- **Wzbogacenie**: opcjonalnie sciezka do folderu sprawy z .docx/.xlsx/.pdf (skill je czyta przez markitdown).
- **Kalibracja**: opcjonalnie plik `[Sprawa]_LPM_Memory.md` z preferencjami partnera (jak nazywac etapy, jak liczyc RAG, co eskalowac).

### Wyjscie

- **Plik**: `[Sprawa]_status_[YYYY-MM-DD].docx` w folderze sprawy.
- **Sekcje** (template w `skills/status-report-drafter-pl/templates/`):
  - Naglowek: klient, sprawa, okres raportowy, autor.
  - **Ocena RAG** (Czerwony/Bursztynowy/Zielony) z uzasadnieniem "patrzac na nastepne 2 tygodnie".
  - **Postep** (co dowiezione w okresie, z datami).
  - **Aktywnosc bez postepu** (gdy dotyczy - sygnal eskalacji).
  - **Luki** (czego brakuje, kogo czekamy, od kiedy).
  - **Akcje na nastepny okres** (kazda z terminem twardym i eskalacja gdy brak).
  - **Eskalacje** (zespol / wspolnik / klient).
  - **Budzet** (jezeli sa dane: % spalenia, prognoza-do-konca, ostrzezenia overrun).

### Workflow (6 krokow)

1. **Identyfikacja sprawy** - skill prosi o nazwe/kod sprawy. Sprawdza czy istnieje folder `[Sprawa]_LPM_Memory.md`. Jezeli tak - laduje preferencje.
2. **Ekstrakcja signals z inputu** - parsuje watek mailowy, wyciaga: kto co zrobil, kiedy, jaki rezultat, co czeka, kogo czekamy.
3. **Identyfikacja luk** - co NIE wynika z inputu ale powinno (brak update'u z X od Y dni, brak odpowiedzi na pismo z dnia Z).
4. **Ocena RAG forward-looking** - patrzac na nastepne 2 tyg: czy milestone'y w zagrozeniu. Kolor + uzasadnienie.
5. **Draft** - wypelnienie szablonu .docx z naglowkiem kancelarii. Kazda akcja z data twarda + eskalacja.
6. **Review checklist** - skill na koniec wyswietla checkliste "czy partner moze to wyslac": czy RAG ma uzasadnienie / czy akcje maja terminy / czy luki sa nazwane / czy nie ma named-firm atrybucji.

### Cross-skill handoff

- **Czyta od**: `risk-and-issues-manager-pl` (rejestr ryzyk per sprawa - jezeli istnieje, ryzyka aktywne sa zaciagniete do RAG reasoning).
- **Czyta od**: `scope-change-controller-pl` (jezeli istnieje rejestr scope changes - skill je raportuje pod "Budzet").
- **Pisze do**: folderu sprawy (.docx + update `[Sprawa]_LPM_Memory.md` z tym co RAG byl Czerwony - kalibracja na przyszlosc).

### Kalibracja per sprawa

Plik `[Sprawa]_LPM_Memory.md` (w folderze sprawy) trzyma:

- Jak partner nazywa etapy sprawy (np. "due diligence", "negocjacje", "zamkniecie").
- Preferowany ton (formalny vs zwiezly).
- Progi RAG (jezeli wspolnik chce konserwatywniej oceniac).
- Jakie pola sa obowiazkowe w raportach do tego konkretnego klienta.
- Historia RAG (do trendu).

Skill aktualizuje ten plik PO kazdym wygenerowanym raporcie - dopisuje 1-2 linie obserwacji.

### Antywzorce (NIE robimy)

- NIE nazywamy konkurencyjnych kancelarii ("CMS powiedzial") - mowimy "kontrkancelaria".
- NIE wstawiamy konkretnych stawek - mowimy "stawka kancelarii", "polityka rozlicen".
- NIE wymyslamy faktow ktorych nie ma w inputcie. Jezeli czegos brakuje - wymieniamy w sekcji "Luki".
- NIE proponujemy markdown w czacie zamiast .docx (chyba ze EXPLICIT prompt to mowi).
- NIE pomijamy luki bo "wyglada zle". Luki sa wartoscia raportu.

---

## Skill #2 - scope-change-controller-pl

**Status**: LIVE w v0.2.0-alpha. Pelen spec: [skills/scope-change-controller-pl/SKILL.md](skills/scope-change-controller-pl/SKILL.md).

Sygnatura w skrocie:

- **Cel**: wykryc scope creep w korespondencji, klasyfikowac jako (w zakresie / poza / niejasne), generowac wniosek o rozszerzenie albo notatke wewnetrzna.
- **Input**: korespondencja z klientem + pierwotna oferta/umowa.
- **Output**: jeden z trzech: notatka wewnetrzna .md / wniosek do klienta .docx / notatka decyzyjna dla wspolnika .md.
- **Cross-skill**: czyta od status-report-drafter-pl, pisze do risk-and-issues-manager-pl (kazda zmiana scope = nowe ryzyko).

## Skill #3 - risk-and-issues-manager-pl

**Status**: LIVE w v0.2.0-alpha. Pelen spec: [skills/risk-and-issues-manager-pl/SKILL.md](skills/risk-and-issues-manager-pl/SKILL.md).

Sygnatura w skrocie:

- **Cel**: rejestr RAID (Ryzyka / Zalozenia / Problemy / Decyzje) per sprawa, eskalacja gdy RAG Czerwony.
- **Input**: korespondencja + status raporty historyczne + ewentualnie istniejacy RAID.
- **Output**: .xlsx rejestr RAID (4 arkusze) + opcjonalnie draft eskalacji .md.
- **Cross-skill**: czyta od status-report-drafter-pl i scope-change-controller-pl, pisze do status-report-drafter-pl (sygnal o zmianie RAG).

---

## Plan implementacji

### Faza 0 (2026-05-21, zamknieta)
- [x] README.md
- [x] LICENSE (Apache 2.0)
- [x] CONSTITUTION.md v1.0.0
- [x] SPEC.md (ten plik)
- [x] CHANGELOG.md
- [x] skills/status-report-drafter-pl/ (SKILL + template + example)
- [x] skills/scope-change-controller-pl/ (SKILL + template + example)
- [x] skills/risk-and-issues-manager-pl/ (SKILL + template + example)
- [x] docs/polskie-realia.md (notatki o roznicach vs common-law LPM)

### Faza 1 (najblizsze 7-14 dni)
- [ ] marko-pl + humanizer-pl audit caly content.
- [ ] legal-ai-plugin-governance audit (RODO, tajemnica zawodowa).
- [ ] Repo push: `matematicsolutions/lpm-pl` public.
- [ ] Aktualnosc na matematic.co + post LI (linkedin-voice-wieslaw-mazur).
- [ ] Walidacja na 1-2 zanonimizowanych sprawach z vault Wieslawa.

### Faza 2 (po walidacji, ~3-6 tygodni)
- [ ] Refresh skilli po feedbacku.
- [ ] Generic brand template .docx (zamiast markdown placeholder).
- [ ] Onboarding workflow dla nowej kancelarii.

### Faza 3 (post-validacja, ~2-3 miesiace)
- [ ] Pozostale skille z upstream - selekcja co ma sens pod polskie realia (`timeline-generator-pl`, `daily-briefing-pl`, `document-approval-tracker-pl`).
- [ ] Integracja z Patron (opcjonalna konsumpcja).
- [ ] Integracja z LEX Kancelaria / Mecenas IT / Comarch IBARD.
- [ ] M365 / SharePoint connector (opcjonalny, RODO-flag).
