# AGENTS.md - lpm-pl

Plik standardu [agents.md](https://agents.md) (Linux Foundation / Agentic AI Foundation) - kanoniczne instrukcje dla agentow AI pracujacych z tym repozytorium. Czytany natywnie przez Cursor, Codex (OpenAI), Jules (Google), Devin / Windsurf, Aider, Amp, Factory, GitHub Copilot.

## Cel projektu

`lpm-pl` to otwarty zestaw **skilli AI do zarzadzania portfelem spraw w polskiej kancelarii** (Legal Project Management). Cherry-pick wzorca [legalopsconsulting/lpm-skills](https://github.com/legalopsconsulting/lpm-skills) (Apache 2.0) przepisany pod polskie realia: stawki godzinowe i ryczalt zamiast AFA, integracja z LEX Kancelaria / Mecenas IT / Comarch ERP, output `.docx` z naglowkiem kancelarii.

**To nie jest [Patron](https://github.com/matematicsolutions/patron).** Patron pracuje na sprawie (research, drafting, RAG po aktach). `lpm-pl` pracuje nad portfelem (status raporty, scope, ryzyka, budzet). Dwa rozne narzedzia dla dwoch roznych ludzi.

## Kontekst MateMatic (TWARDE OGRANICZENIA)

Repo prowadzi [MateMatic Solutions](https://matematicsolutions.com). Skille dotyczy:

- **Tajemnica zawodowa** (PoA art. 6, URP art. 3) - skill nie wysyla aktow sprawy do chmury bez decyzji uzytkownika. Email-first input zaklada ze dane przeszly juz przez akceptacje uzytkownika do schowka.
- **RODO** - skill operuje na danych ktore juz sa lokalnie, nie zaciaga z zewnatrz.
- **Bez nazywania firm** ([CONSTITUTION.md](./CONSTITUTION.md) zasada 7) - skill nigdy nie wstawia "Skadden powiedzial" ani "stawka XYZ" - mowi "wspolnik zarzadzajacy", "dzial rozlicen", "potwierdz w polityce kancelarii".

## Struktura repo

```
skills/
  status-report-drafter-pl/    - cotygodniowy/comiesieczny status raport sprawy
  scope-change-controller-pl/  - wykrywanie scope creep w korespondencji
  risk-and-issues-manager-pl/  - rejestr RAID (Ryzyka/Zalozenia/Problemy/Decyzje)
examples/                      - przyklady wejscia/wyjscia
docs/                          - dokumentacja designu
CONSTITUTION.md                - 7 zasad pod ktorymi piszemy skille
SPEC.md                        - specyfikacja techniczna v0.2.0
CHANGELOG.md                   - historia wersji
```

Skille sa **composable**: scope-change-controller karmi risk-and-issues-manager (zmiana scope = nowe ryzyko), risk-and-issues-manager sygnalizuje status-report-drafter (RAG sprawy moze sie zmienic).

**Composability z [matematic-contract-review-pl](https://github.com/matematicsolutions/matematic-contract-review-pl)**: jezeli skill contract-review-pl zwroci czerwone flagi w portfelu umow klienta (np. NDA bez wylaczen, prawo USA w kontrakcie dostawczym), risk-and-issues-manager-pl moze je przejac jako nowe ryzyka sprawy w rejestrze RAID. Dwa skille ortogonalne tematycznie (portfolio spraw vs portfolio umow), composable workflow.

## Build i test

Repo nie ma kompilacji - to skille Markdown dla Claude Code i kompatybilnych. "Testem" jest przepuszczenie skill przez **zanonimizowana sprawe testowa** z [examples/](./examples/) i wizualna walidacja outputu `.docx`.

Instalacja lokalna (Claude Code):

```bash
cd ~/.claude/skills/
git clone https://github.com/matematicsolutions/lpm-pl
ln -s lpm-pl/skills/status-report-drafter-pl status-report-drafter-pl
ln -s lpm-pl/skills/scope-change-controller-pl scope-change-controller-pl
ln -s lpm-pl/skills/risk-and-issues-manager-pl risk-and-issues-manager-pl
```

Na Windows zamiast symlinkow - kopia folderu `skills/<nazwa>/` do `~/.claude/skills/`.

## Zasady pisania skilli

- **Email-first input** - kopiujesz watek z Outlooka, skill wyciaga signals. Nie zakladaj API.
- **Output `.docx` z brand szablonem kancelarii** - status raport ma byc gotowy do wyslania, nie do przepisywania.
- **Kazda akcja ma date** - "skontaktuj sie z X" nie istnieje, istnieje "do piatku 28.02, jezeli brak odpowiedzi - eskalacja w poniedzialek 3.03".
- **RAG framework** - Czerwony / Bursztynowy / Zielony. Triage konsekwentny we wszystkich 3 skillach.
- **Judgment calls embedded** - skill rozumie kiedy "wszystko gra" znaczy "nic sie nie dzieje", kiedy aktywnosc nie jest postepem, kiedy luka jest istotniejsza niz update.
- **Bez polskich znakow w commit messages**.

## Czego NIE robic (twarde reguly)

- **NIE wprowadzaj AFA / phase-based fees** do skilli - w Polsce art. 16 KEA + brak modelu AFA = nie zmieniaj polskiego billingu pod amerykanski upstream.
- **NIE nazywaj firm ani konkretnych stawek** w outputach skilla.
- **NIE wysylaj danych do chmury** w opisach przykladow ([CONSTITUTION.md](./CONSTITUTION.md) zasada 1).
- **NIE commituj danych prawdziwej sprawy** w `examples/` - tylko zanonimizowane lub syntetyczne.

## Zrodla prawdy (kolejnosc czytania)

1. [README.md](./README.md) - opis dla ludzi
2. [CONSTITUTION.md](./CONSTITUTION.md) - 7 zasad konstytucyjnych skilli
3. [SPEC.md](./SPEC.md) - specyfikacja techniczna v0.2.0-alpha
4. [CHANGELOG.md](./CHANGELOG.md) - 0.1.0 -> 0.2.0
5. [examples/](./examples/) - prawdziwe pelne przejscia skilla

## Kompatybilnosc agentow

Ten plik (AGENTS.md) jest standardem [agents.md](https://agents.md). Skille sa pisane pod Claude Code, ale pattern (email-first input, RAG triage, `.docx` output) jest agent-agnostic - mozesz przepisac pod Cursor / Codex / Devin.

Dla Claude Code dodatkowo istnieje plik [CLAUDE.md](./CLAUDE.md) importujacy ten dokument.

## Licencja i atrybucja

- **Apache 2.0** - patrz [LICENSE](./LICENSE).
- Pattern architektoniczny: cherry-pick z [legalopsconsulting/lpm-skills](https://github.com/legalopsconsulting/lpm-skills) (Apache 2.0).
- Tresc skilli: przepisana pod polski model wspolpracy kancelarii z klientem. NIE jest to tlumaczenie 1:1.

Cytowanie: *MateMatic Solutions (2026), lpm-pl - skille zarzadzania projektami prawnymi dla polskich kancelarii, https://github.com/matematicsolutions/lpm-pl, Apache 2.0.*
