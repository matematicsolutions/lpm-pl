# SPEC.md - specyfikacja MVP lpm-pl

**Wersja**: 0.1.0-alpha
**Data**: 2026-05-21
**Zakres**: 3 skille (status / scope / risk), w tym dokumencie szczegolowo opisany skill #1.

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

## Skill #2 - scope-change-controller-pl (faza 2, po walidacji #1)

Skrocony spec - rozbudujemy po walidacji #1.

- **Cel**: wykryc scope creep w korespondencji, zaproponowac wniosek o zmiane zakresu (do klienta) albo notatke wewnetrzna "to bylo w pierwotnej ofercie".
- **Input**: korespondencja z klientem + pierwotna oferta/umowa.
- **Output**: .docx "Wniosek o zmiane zakresu sprawy [X]" LUB notatka wewnetrzna.

## Skill #3 - risk-and-issues-manager-pl (faza 2)

- **Cel**: rejestr ryzyk, zalozen, problemow i decyzji per sprawa.
- **Input**: korespondencja + status raporty historyczne.
- **Output**: .xlsx rejestr (RAID log) + draft mailowy do eskalacji.

---

## Plan implementacji

### Faza 0 (DZISIAJ, 2026-05-21)
- [x] README.md
- [x] LICENSE (Apache 2.0)
- [x] CONSTITUTION.md v1.0.0
- [x] SPEC.md v0.1.0-alpha (ten plik)
- [ ] CHANGELOG.md
- [ ] skills/status-report-drafter-pl/SKILL.md
- [ ] skills/status-report-drafter-pl/templates/status_report_template.md
- [ ] examples/sample_status_report.md (zanonimizowany przyklad)
- [ ] docs/polskie-realia.md (notatki o roznicach vs common-law LPM)

### Faza 1 (najblizsze 7-10 dni)
- [ ] Walidacja status-report-drafter-pl na zanonimizowanej sprawie z vault Wieslawa.
- [ ] marko-pl + humanizer-pl audit caly content.
- [ ] legal-ai-plugin-governance audit (RODO, tajemnica zawodowa).
- [ ] Repo push: `matematicsolutions/lpm-pl` public.
- [ ] Aktualnosc na matematic.co + post LI (linkedin-voice-wieslaw-mazur).

### Faza 2 (po walidacji #1, ~2 tygodnie)
- [ ] Skille #2 i #3 (scope + risk) - pelne SKILL.md, templates, examples.
- [ ] Composability test (#1 czyta od #3).
- [ ] v0.2.0-beta release.

### Faza 3 (post-MVP, otwarte)
- [ ] Pozostale 13 skilli z upstream - selekcja co ma sens pod polskie realia.
- [ ] Integracja z Patron (opcjonalna konsumpcja).
- [ ] M365 / SharePoint connector (opcjonalny, RODO-flag).
