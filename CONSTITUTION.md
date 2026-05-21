# Konstytucja projektu lpm-pl

**Wersja**: 1.0.0
**Data przyjecia**: 2026-05-21
**Status**: ratyfikowana przez Wieslawa Mazura (MateMatic)

Konstytucja jest nadrzedna wobec specyfikacji konkretnych skilli. Jezeli SKILL.md koliduje z artykulem Konstytucji - Konstytucja wygrywa. Zmiana Konstytucji wymaga bumpa SEMVER i wpisu w CHANGELOG.md.

---

## Misja

Daj polskiej kancelarii narzedzie ktore zdejmuje z partnera zarzadzajacego cotygodniowy rytual "skladania raportow z 40 spraw" bez wysylania danych klienta do chmury i bez kazdego partnera musiacego nauczyc sie nowego SaaS.

## Artykul 1 - RODO-safe by default

Kazdy skill MUSI dzialac na materiale ktory uzytkownik wkleja lub wskazuje lokalnie. Skill NIE WYSYLA aktow sprawy, korespondencji z klientem ani danych osobowych do zadnego zewnetrznego API poza modelem LLM ktorego uzytkownik uzywa swiadomie (Claude, lokalny model przez Ollama, etc.).

Skill NIE zapisuje danych klienta poza folderem sprawy wskazanym przez uzytkownika. Brak telemetrii, brak "cloud sync", brak analytics.

Domyslny tryb: offline-compatible. Integracje z M365 / SharePoint / Outlook sa OPCJONALNE i wymagaja swiadomej zgody uzytkownika z ostrzezeniem o transferze danych.

## Artykul 2 - Polskie realia billingu

Skille NIE zakladaja modeli fee przyjetych w common-law (AFA - Alternative Fee Arrangements, phase-based budgets, retainer-pluss-success). Domyslne modele:

- **Stawka godzinowa** (najczestszy) - skill liczy budzet jako stawka x prognoza godzin per faza.
- **Ryczalt** (cena za sprawe lub za etap) - skill liczy spalanie ryczaltu vs zaawansowanie pracy.
- **Success fee** - z ograniczeniami ustawowymi (art. 16 Kodeksu Etyki Adwokackiej; analogicznie KEZRP dla radcow). Skill NIE proponuje pactum de quota litis bez ostrzezenia o limitach.
- **Hybryda** (stawka + success cap) - najczestszy w sprawach gospodarczych.

Skille NIE wstawiaja konkretnych stawek ani nazw firm konkurencyjnych. Wstawiaja "stawka kancelarii", "polityka rozlicen", "wspolnik zarzadzajacy".

## Artykul 3 - Output do .docx z brand szablonem kancelarii

Status raporty, wnioski o zmiane scope, rejestry ryzyk - wszystko produkowane jako **.docx** z naglowkiem kancelarii (template definiowany per wdrozenie w `templates/`).

Skill NIE pyta "wolisz markdown czy Word". Standard branzowy: kancelaria wysyla klientowi .docx albo PDF wygenerowane z .docx. Markdown wewnatrz okna czatu to dowod e niczego nie zrobiles.

Wyjatek tylko gdy uzytkownik EXPLICIT prosi "daj mi bullety w czacie" / "wklej tu". Wczesniejsze preferencje uzytkownika do markdown w innych skillach NIE sa wystarczajacym uzasadnieniem.

## Artykul 4 - Email-first input, format-agnostic

Domyslnie skill przyjmuje wklejone watki z Outlooka / Gmail / Teams. Akceptuje takze: notatki ze spotkan, surowe transkrypcje (z LEX, Otter, Whisper), zalaczone .docx/.xlsx/.pdf. Nie zaklada konkretnego workflow zarzadzania mailami - kancelaria moze uzywac Comarch IBARD, Outlook, Apple Mail, wszystkiego.

## Artykul 5 - Judgment calls embedded, nie tylko sumaryzacja

Skill NIE jest sumaryzatorem. Skill rozpoznaje:

- **Postep vs aktywnosc**: "30 maili w tym tygodniu" to aktywnosc. "Trzy postanowienia uchylone, dwie ugody podpisane" to postep. Aktywnosc bez postepu = sygnal eskalacji.
- **Cisza jako sygnal**: brak update'u to status, nie luka. "Od 3 tyg brak odpowiedzi z sadu" jest raportowalne.
- **Termin obowiazkowy dla kazdej akcji**: "skontaktuj sie z bieglym" nie istnieje. "Do piatku 28.02; jezeli brak - eskalacja w poniedzialek 3.03" istnieje.
- **RAG forward-looking**: kolor ocenia ryzyko ze sprawa nie dowiezie milestone'a w nastepnych 2 tyg, nie czy w ostatnich 2 tyg bylo duzo pracy.
- **Eskalacja triage**: zespol-rozwiazuje / wspolnik-decyduje / klient-musi-wiedziec. Test: "jezeli sprawa pojdzie zle a wspolnik/klient nie wiedzial - czy to nasza wina raportowa?".

## Artykul 6 - Composability ponad kompletnoscia

Lepiej miec 3 dobrze skomponowane skille (status czyta z risk register, scope feeduje status) niz 16 odseparowanych. Kazdy nowy skill MUSI deklarowac:

- **Czyta od**: jakie skille / pliki konsumuje.
- **Pisze do**: jakie skille / pliki produkuje.
- **Handoff points**: gdzie dotyka innych skilli.

Brak silosow. Brak "robie wszystko sam".

## Artykul 7 - Atrybucja patternu i niezaleznosc tresci

Pattern architektoniczny (email-first input, output do folderu sprawy, judgment calls embedded, composability, RAG framework, escalation triage) jest cherry-pick z [legalopsconsulting/lpm-skills](https://github.com/legalopsconsulting/lpm-skills) na licencji Apache 2.0 snapshot. Atrybucja w README.md i kazdym SKILL.md.

Tresc skilli (konkretne judgment calls, terminologia, modele billingu, integracje, polskie aspekty prawne) jest pisana od zera pod polski model wspolpracy kancelarii z klientem. NIE jest to tlumaczenie 1:1 - jest to przepisanie.

Upstream zapowiada przejscie na AGPL-3.0 w przyszlych wersjach. lpm-pl pozostaje na Apache 2.0 snapshot z dnia cherry-picku (2026-05-21). Pozniejsze zmiany upstream NIE sa automatycznie wciagane.

---

## Bramki MateMatic (sprawdzane przed kazdym mergem)

1. **Licencja**: zgodnie z Art. 7. Nowe zalezności tylko permissive (MIT/Apache/BSD/CC BY).
2. **Anty-OS**: zero ToS bypass, zero scraping uslug niemajacych zgody, zero "ukrytego" transferu danych klienta.
3. **Jakosc**: kazdy skill ma testowy przypadek na zanonimizowanej sprawie + przejscie marko-pl + humanizer-pl.
4. **Strategia**: skill rozwiazuje realny bol polskiej kancelarii. Jezeli wymaga 3 SaaS subskrypcji za 400 USD/mc - odrzucamy.

## Roles

- **Wlasciciel produktu**: Wieslaw Mazur (MateMatic) - decyzje strategiczne, ratyfikacja Konstytucji.
- **Maintainer**: zespol MateMatic - PR review, release, dokumentacja.
- **Reviewer tresci**: marko-pl (rygor zarzutow) + humanizer-pl (anty-AI-slop) przed kazdym mergem.
- **Walidator** prawny: licencjonowany prawnik wskazany przez klienta - kazda kancelaria ratyfikuje skill przed uzyciem produkcyjnym.

## Antygoals (czego NIE robimy)

- NIE budujemy SaaS - to skille lokalne, kancelaria je instaluje u siebie.
- NIE konkurujemy z LEX/LegalBase/Mecenas IT - jestesmy warstwa AI **nad** tymi systemami, nie zamiast.
- NIE bierzemy odpowiedzialnosci prawnej za output - skill to draft do akceptacji prawnika.
- NIE robimy time-trackera ani billing-systemu - skill konsumuje dane z istniejacego systemu (Mecenas/Kancelaria/Excel), nie je generuje.

---

## Dziennik szlifu

- **2026-05-21** - Konstytucja v1.0.0 ratyfikowana. Cherry-pick patternu z lpm-skills Apache 2.0 snapshot. MVP: 3 skille (status/scope/risk), na start tylko status-report-drafter-pl.
