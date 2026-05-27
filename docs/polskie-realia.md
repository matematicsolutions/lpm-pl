# Polskie realia LPM - czym sie roznimy od upstream

Notatki maintainera dla kontrybutorów i recenzentów. Wyjasniaja decyzje produktowe ktore odroznia lpm-pl od [legalopsconsulting/lpm-skills](https://github.com/legalopsconsulting/lpm-skills).

## 1. Model rozlicen (billing)

### Upstream (common-law US/UK):

- **AFA - Alternative Fee Arrangements**: capped fee, success fee, conditional fee, blended rate, portfolio pricing.
- **Phase-based budgeting**: budzet per faza sprawy (e.g. discovery / motion / trial / appeal).
- **Write-off analysis**: wycofanie nieblowalnych godzin z ksiazki.
- **Realization rate**: % wystawionych godzin zafakturowanych.

### Polskie realia:

- **Stawka godzinowa** (dominujacy) - wspolnik X PLN/h, senior associate Y PLN/h, junior Z PLN/h.
- **Ryczalt** (stala kwota za sprawe lub etap) - czesty w sprawach gospodarczych standardowych.
- **Success fee z ograniczeniami ustawowymi**:
  - Adwokaci: art. 16 Kodeksu Etyki Adwokackiej - zakaz wynagrodzenia wylacznie z udzialu w zasadzonej kwocie (pactum de quota litis), dozwolone success fee jako dodatek do stawki podstawowej.
  - Radcowie prawni: KEZRP - analogicznie.
  - Sprawy karne: bardziej restrykcyjne.
- **Hybryda**: stawka godzinowa + cap budzetu + success fee jako bonus.

### Implementacja w skillach:

Skill NIE proponuje AFA / phase-based / write-off. Skill operuje na 4 modelach: stawka / ryczalt / success / hybryda. Jezeli wykryje w korespondencji propozycje pactum de quota litis - **ostrzezenie o ograniczeniach ustawowych** i prosba o decyzje prawnika.

## 2. Struktura kancelarii

### Upstream:

- Partner (equity / non-equity) / Of Counsel / Senior Associate / Associate / Paralegal.
- Often US/UK-style billable hours target (1800-2200h/rok).
- Practice groups, sector teams.

### Polskie realia:

- Wspolnik (partner) / radca prawny lub adwokat / aplikant / paralegal.
- Mniejsze kancelarie (1-15 prawnikow) dominuja - inny model niz duzy magic circle.
- W malych kancelariach wspolnik jest tez billerem - rzadko ma dedykowanego LPM/Office Managera.
- LegalTech adopcja: LEX (Wolters Kluwer), Legalis (CH Beck), Lexis (rzadziej), Mecenas IT, Comarch ERP (billing), Kancelaria (Wolters), Praktyczne (Beck).

### Implementacja:

Skille mowia jezykiem polskiej kancelarii (wspolnik / radca / adwokat / aplikant). Nie zakladaja istnienia LPM czy COO - target user to wspolnik lub paralegal. M365 connector pozostawiamy opcjonalnym (faza 3) bo polska kancelaria czesto uzywa LEX/Mecenas zamiast SharePoint.

## 3. RODO i tajemnica zawodowa

### Upstream:

- US/UK przepisy o privilege (attorney-client privilege, litigation privilege).
- Bez bezposrednich wymogow lokalizacji danych.

### Polskie realia:

- **RODO** + **ustawy zawodowe** (Ustawa Prawo o adwokaturze art. 6, Ustawa o radcach prawnych art. 3) = tajemnica zawodowa absolutna.
- **Tajemnica obronna** w sprawach karnych - jeszcze silniejsza.
- Transfer danych poza EOG (np. Microsoft 365, Google Workspace, US AI APIs) wymaga DPA + ocena adekwatnosci (DPF Data Privacy Framework od 2023-07-10, podtrzymany 2025-09-03 - patrz fakty DPF i AI Act na 2026-05).
- AI Act od 2026-08-02 (general-purpose AI obowiazki) - wymaga oceny ryzyka.

### Implementacja:

- Artykul 1 Konstytucji: RODO-safe by default. Skill pracuje na materiale ktory uzytkownik EXPLICIT wskazuje. Brak telemetrii, brak cloud sync.
- Integracje z M365 / SharePoint sa OPCJONALNE i wymagaja zgody uzytkownika z OSTRZEZENIEM o transferze.
- Companion stack: stack zero-cloud MateMatic (zero-cloud RODO-safe self-hosted) (Cline + noScribe + DocuSeal + lokalny RAG) dla kancelarii ktore chca pelne self-host.
- Roadmap: lokalny model przez Ollama (Llama 3.1 / Qwen 3) jako alternatywa dla Claude API w fazie 3.

## 4. Jurysdykcja - rodzaje spraw

### Upstream:

- M&A, litigation, regulatory, IP, antitrust.
- Multi-jurisdictional matters (zazwyczaj US-UK-DE-FR-JP) z local counsel.

### Polskie realia:

- Sprawy gospodarcze (Sad Gospodarczy), cywilne (sad rejonowy / okregowy / apelacyjny / SN), karne, administracyjne (WSA / NSA), pracownicze.
- Postepowania przed organami: UODO, UOKiK, KNF, UKE, URE, KIO, US, ZUS.
- Sprawy unijne (TS UE, TSUE, ETPC).

### Implementacja:

Skill nie zaklada konkretnego rodzaju sprawy ("practice-area agnostic" jak upstream). Ale terminologia w przykladach i templatkach pasuje do polskiego systemu (apelacja, kasacja, skarga konstytucyjna, WSA, etc).

Dla najczestszych rodzajow spraw w fazie 3 mozemy dodac warianty: status-report-drafter-pl-litigation (z timeline rozpraw), status-report-drafter-pl-ma (z deal milestones).

## 5. Output - dlaczego .docx z brand szablonem

### Upstream:

- "All outputs MUST be .docx files. No exceptions" (z status-report-drafter v2.2.0).
- Bez specyfikacji konkretnego template.

### Polskie realia:

- Polskie kancelarie wysylaja klientowi .docx (z naglowkiem) ALBO PDF wygenerowany z .docx.
- Naglowek MUSI zawierac: nazwe kancelarii, adres, NIP, KRS (jezeli sp.), siedzibe.
- Czesto stopka z numerami telefonow, mail, RODO disclaimer.

### Implementacja:

- Skill produkuje .docx (zgodnie z upstream).
- Naglowek/stopka definiowane w `templates/[Kancelaria]_brand.docx` per wdrozenie (kancelaria daje swoj template podczas onboardingu).
- W open source repo lezy generic template do podmiany.

## 6. Etapy sprawy - jezyk

### Upstream:

- "Phase I / II / III" lub "discovery / motion / trial".

### Polskie realia:

Zaleznie od rodzaju sprawy:

- **Cywilna**: rejestracja sprawy / postepowanie dowodowe / wyrok / apelacja / kasacja / egzekucja.
- **Gospodarcza**: due diligence / negocjacje / podpisanie umowy / wykonanie / spor (jezeli wypadnie).
- **Karna**: postepowanie przygotowawcze / akt oskarzenia / postepowanie sadowe / wyrok / apelacja.
- **Administracyjna**: postepowanie pierwszej instancji / odwolanie / WSA / NSA.

### Implementacja:

Skill akceptuje dowolne nazewnictwo etapow per sprawa - kalibracja w `[Sprawa]_LPM_Memory.md`. Default propozycja w templatce: "Etap 1 / 2 / 3" z prosba o doprecyzowanie przez prawnika.

---

## Konsekwencje dla planu rozwoju

- **Faza 1 (MVP)**: skill #1 status-report-drafter-pl. Polonizujemy judgment calls, RAG framework, escalation triage. Modele billingu z punktu 1. Brak AFA.
- **Faza 2**: skille #2 (scope) i #3 (risk). Te potrzebuja: zasob "polityka rozlicen kancelarii" jako kontekst, tabela ryzyk z polskich pojec (terminy procesowe, zarzuty zazalenia, zawieszenia postepowania).
- **Faza 3**: integracja z LEX / Mecenas IT / Comarch IBARD (przez tych dostawcow API, jezeli udostepniaja). Lokalny model przez Ollama jako alternatywa dla Claude.
- **Faza 4** (jezeli sie powiedzie): inne skille z upstream (timeline-generator, document-approval-tracker, stakeholder-comms-planner). Selektywnie - nie wszystkie 16.
