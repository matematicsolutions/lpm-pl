# lpm-pl — skille zarzadzania projektami prawnymi (polskie kancelarie)

Otwarty zestaw skilli AI do zarzadzania portfelem spraw w polskiej kancelarii. Cherry-pick wzorca [legalopsconsulting/lpm-skills](https://github.com/legalopsconsulting/lpm-skills) (Apache 2.0) z przepisaniem pod polskie realia: stawki godzinowe i ryczalty zamiast AFA, polski model billingu, integracja z LEX Kancelaria / Mecenas IT / Comarch ERP, output do .docx z naglowkiem kancelarii.

**To nie jest [Patron](https://github.com/matematicsolutions/patron).** Patron pracuje na sprawie (research, drafting, RAG po aktach). lpm-pl pracuje nad portfelem (status raporty, scope, ryzyka, budzet). Dwa rozne narzedzia dla dwoch roznych ludzi w kancelarii.

## Dla kogo

- Managing Partner / wspolnik zarzadzajacy ktory tonie w status mailach z 40 spraw rownolegle.
- Legal Project Manager (jezeli kancelaria ma) - ale wiekszosc polskich kancelarii nie ma, wiec skille mowia do partnera lub paralegala.
- Dyrektor operacyjny / Office Manager pilnujacy budzetow i scope creep.

## Co jest w MVP (faza 1)

Trzy skille pilotazowe pokrywajace najwiekszy bol operacyjny:

1. **status-report-drafter-pl** - tworzy cotygodniowy/comiesieczny status raport sprawy z surowych maili, notatek, korespondencji. Output: .docx z naglowkiem kancelarii, ocena RAG (Czerwony/Bursztynowy/Zielony), eskalacje, luki.
2. **scope-change-controller-pl** *(faza 2)* - wykrywa scope creep w korespondencji z klientem, generuje wniosek o zmiane zakresu albo notatke "to bylo w pierwotnej ofercie".
3. **risk-and-issues-manager-pl** *(faza 2)* - rejestr ryzyk, zalozen, problemow i decyzji per sprawa. Wyciaga ukryte ryzyka z mailowych watkow.

W tej wersji repo jest tylko skill #1. #2 i #3 dolacza po walidacji #1 na zywej sprawie.

## Instalacja (Claude Code)

```
cd ~/.claude/skills/
git clone https://github.com/matematicsolutions/lpm-pl
ln -s lpm-pl/skills/status-report-drafter-pl status-report-drafter-pl
```

Albo skopiuj folder `skills/status-report-drafter-pl/` do `~/.claude/skills/`.

## Filozofia

Patrz [CONSTITUTION.md](CONSTITUTION.md) - 7 zasad pod ktorymi piszemy te skille. Skrocony spis:

1. RODO-safe by default - skill pracuje na danych ktore juz masz lokalnie, nie wysyla aktow sprawy do chmury bez Twojej decyzji.
2. Polskie realia billingu - stawki godzinowe, ryczalt, success fee z limitami ustawowymi. Bez AFA, bez phase-based fees.
3. Output do .docx z brand szablonem kancelarii - status raport ma byc gotowy do wyslania, nie do przepisywania.
4. Email-first input - kopiujesz watek z Outlooka, skill wyciaga signals.
5. Judgment calls embedded - skill rozumie kiedy "wszystko gra" znaczy "nic sie nie dzieje", kiedy aktywnosc nie jest postepem, kiedy luka jest istotniejsza niz update.
6. Kazda akcja ma date - "skontaktuj sie z X" nie istnieje, istnieje "do piatku 28.02, jezeli brak odpowiedzi - eskalacja w poniedzialek 3.03".
7. Bez nazywania firm - skill nigdy nie wstawia "Skadden powiedzial" ani "stawka XYZ" - mowi "wspolnik zarzadzajacy", "dzial rozlicen", "potwierdz w polityce kancelarii".

## Licencja

Apache 2.0 - patrz [LICENSE](LICENSE). Mozesz wziac, zmienic, sprzedawac wdrozenie. Wymagamy zachowania atrybucji.

## Pochodzenie i atrybucja

- Pattern architektoniczny (email-first input, output do folderu sprawy, judgment calls embedded, composability) - cherry-pick z [legalopsconsulting/lpm-skills](https://github.com/legalopsconsulting/lpm-skills) (Apache 2.0).
- Tresc skilli (judgment calls, RAG framework, escalation triage) - przepisana pod polski model wspolpracy kancelarii z klientem. NIE jest to tlumaczenie 1:1.
- Brand i utrzymanie: [MateMatic](https://matematic.co) - wspieramy polskie kancelarie w adopcji AI z poszanowaniem tajemnicy zawodowej.

## Status

`v0.1.0-alpha` - przed walidacja na zywej sprawie. Nie uzywaj produkcyjnie zanim nie sprawdzisz na zanonimizowanej sprawie testowej.
