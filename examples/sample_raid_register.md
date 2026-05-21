# Przyklad output - RAID Register (zanonimizowany)

> Zanonimizowany przyklad output skilla risk-and-issues-manager-pl. Plik logiczny - w produkcji renderowany do .xlsx z 4 arkuszami. Tutaj jako markdown dla wygody review.

---

# RAID Register - Klient-ABC v Skarb Panstwa - apelacja podatkowa

| Klient | Klient-ABC sp. z o.o. |
|---|---|
| Sprawa / kod | MAT-2026-014 |
| Data ostatniej aktualizacji | 2026-05-21 |
| Wlasciciel rejestru | Wspolnik prowadzacy |

---

## Arkusz 1 - Ryzyka (Risks)

| ID | Data ident. | Opis ryzyka | Prawd. | Wplyw | RAG | Mitigacja | Wlasciciel | Termin walidacji | Status |
|---|---|---|---|---|---|---|---|---|---|
| R-2026-04-001 | 2026-04-22 | Brak opinii bieglej do terminu zlozenia pisma do WSA (5.06.2026). | Wysokie (>60%) | Wysoki (zagrozenie wyniku) | Czerwony | (1) Telefon eskalacyjny do bieglej 24.05; (2) jezeli brak - wniosek o alternatywna bieglosc do 27.05; (3) scenariusz B - pismo z zapowiedzia uzupelnienia. | Wspolnik prowadzacy | 24.05.2026 | Otwarte, mitigacja w toku |
| R-2026-05-003 | 2026-05-08 | Mozliwe zazalenie kontrkancelarii na postanowienie SR z 28.04 (termin do 12.05 - mineli). | Niskie (<25%, termin uplynal) | Sredni | Zielony | Brak dzialan, monitorujemy. | Aplikant | 2026-06-01 (potwierdzenie zamkniecia okna) | Otwarte, do zamkniecia |
| R-2026-05-007 | 2026-05-19 | Klient moze zmienic strategie po wyniku rozprawy 28.05 (sygnal z konferencji 19.05 - "zastanowimy sie nad ugoda jezeli SA zasugeruje"). | Srednie | Sredni | Bursztynowy | Przygotowac scenariusz ugodowy przed rozprawa. | Radca prawny | 27.05.2026 | Otwarte |

---

## Arkusz 2 - Zalozenia (Assumptions)

| ID | Data ident. | Opis zalozenia | Zrodlo zalozenia | Termin walidacji | Status walidacji | Notatki |
|---|---|---|---|---|---|---|
| Z-2026-04-002 | 2026-04-12 | Bieglosc przygotuje opinie do 26.05.2026 (10 dni przed deadline'em pisma). | Postanowienie sadu z 28.03 + standardowy czas opinii (4-6 tyg). | 2026-05-10 | Falsyfikowane (24.05) - opinia nie wplynela, biegla nie odpowiada. Konwersja do R-2026-04-001. | Zalozenie bledne, przenosimy do ryzyk i problemow. |
| Z-2026-05-005 | 2026-05-15 | Klient ma wewnetrzny deadline biznesowy w polowie czerwca (slowa "musimy to dowiezc do polowy czerwca"). | Mailowy watek klienta 14.05. | 2026-05-22 | W toku - aplikant ma zadac wprost. | Walidacja przez konkretne zapytanie. |

---

## Arkusz 3 - Problemy (Issues)

| ID | Data wystapienia | Opis problemu | Wplyw | Plan rozwiazania | Wlasciciel | Termin | Status |
|---|---|---|---|---|---|---|---|
| P-2026-05-001 | 2026-05-05 (pierwsze ponaglenie) | Bieglosc nie odpowiada na ponaglenia (1: 5.05, 2: 12.05, 3: 18.05). Pisemne kanaly nie dzialaja. | Wysoki - blokuje przygotowanie repliki. | (1) Telefon eskalacyjny 24.05; (2) wniosek o alternatywna 27.05; (3) pismo z zapowiedzia 03.06. | Wspolnik prowadzacy | 24.05.2026 | Eskalowane do wspolnika |
| P-2026-05-004 | 2026-05-09 | Brak potwierdzenia z US o przyjeciu naszego pisma uzupelniajacego z 9.05 (standardowo 5-7 dni, obecnie 12). | Sredni - potencjalna kwestia formalna. | Ponaglenie pisemne do US do 25.05; jezeli brak - kontakt telefoniczny. | Aplikant | 25.05.2026 | Otwarte |

---

## Arkusz 4 - Decyzje (Decisions)

| ID | Data | Decyzja | Decydent | Uzasadnienie | Wplyw na sprawe | Notatki |
|---|---|---|---|---|---|---|
| D-2026-04-008 | 2026-04-22 | Akceptacja scope-change: rozszerzenie zakresu o opinie biegley sadowej. | Klient (CFO Klient-ABC) | Akceptacja wniosku z 28.04. Dodatkowy budzet zaakceptowany. | + 15h budzetu, opoznienie milestone'a o 4 tyg. | Wniosek w pliku `MAT-2026-014_wniosek_rozszerzenie_2026-04-22.docx`. |
| D-2026-05-001 | 2026-05-15 | Akceptacja rozszerzonej wersji repliki do WSA (dodanie argumentacji z postanowieniem SN III FSK XXX/24). | Wspolnik prowadzacy | Postanowienie SN korzystne dla naszej linii, zwiekszy szanse uchylenia decyzji organu II instancji. | Wydluzenie pisma o 8 stron, ale silniejsza argumentacja. | Klient zaakceptowal mailem 16.05. |

---

## Konwencja ID

Format: `{R|Z|P|D}-YYYY-MM-XXX`, gdzie XXX = kolejny numer w danym miesiacu w obrebie tej sprawy.

---

## Eskalacje powiazane

| Data eskalacji | RAID ID | Adresat | Status | Plik eskalacji |
|---|---|---|---|---|
| 2026-05-21 | P-2026-05-001 | Wspolnik prowadzacy | Wyslana, oczekujemy decyzji | MAT-2026-014_eskalacja_2026-05-21.md |

---

## Trend (z `[Sprawa]_LPM_Memory.md`)

- Sprawa miala RAG Zielony przez 6 tyg (mid-marzec - koniec kwietnia).
- Eskalacja zaczela sie 28.04 (decyzja D-2026-04-008 o opinii bieglej).
- Od pierwszego ponaglenia bieglej (5.05) trend Bursztynowy.
- Od 18.05 (3 ponaglenie bez odzewu) - Czerwony.
- Glowne ryzyko: brak wsparcia bieglej. Sciezka B (alternatywna biegla) gotowa.

---

> *RAID prowadzony przez risk-and-issues-manager-pl v0.1.0. Skill jest asystentem. Decyzja o eskalacji do klienta i o zgloszeniu RODO incydentu wymaga konsultacji wspolnika zarzadzajacego.*
