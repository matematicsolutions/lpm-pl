# AGENTS.md - lpm-pl

Standard file [agents.md](https://agents.md) (Linux Foundation / Agentic AI Foundation) - canonical instructions for AI agents working with this repository. Read natively by Cursor, Codex (OpenAI), Jules (Google), Devin / Windsurf, Aider, Amp, Factory, GitHub Copilot.

## Project goal

`lpm-pl` is an open set of **AI skills for managing a case portfolio in a Polish law firm** (Legal Project Management). A cherry-pick of the [legalopsconsulting/lpm-skills](https://github.com/legalopsconsulting/lpm-skills) (Apache 2.0) pattern, rewritten for Polish conditions: hourly rates and flat fee ("ryczalt", flat-fee billing) instead of AFA, integration with LEX Kancelaria / Mecenas IT / Comarch ERP, `.docx` output with the firm's letterhead.

**This is not [Patron](https://github.com/matematicsolutions/patron).** Patron works on the case (research, drafting, RAG over case files). `lpm-pl` works on the portfolio (status reports, scope, risks, budget). Two different tools for two different people.

## MateMatic context (HARD CONSTRAINTS)

The repo is run by [MateMatic Solutions](https://matematicsolutions.com). The skills concern:

- **Professional secrecy** ("tajemnica zawodowa", attorney-client privilege; PoA art. 6, URP art. 3) - the skill does not send case files to the cloud without the user's decision. Email-first input assumes the data has already passed through the user's acceptance into the clipboard.
- **RODO** (Polish GDPR) - the skill operates on data that is already local, it does not pull from outside.
- **No naming firms** ([CONSTITUTION.md](./CONSTITUTION.md) rule 7) - the skill never inserts "Skadden said" or "rate XYZ" - it says "managing partner", "billing department", "confirm in the firm's policy".

## Repo structure

```
skills/
  status-report-drafter-pl/    - weekly/monthly case status report
  scope-change-controller-pl/  - scope creep detection in correspondence
  risk-and-issues-manager-pl/  - RAID register (Risks/Assumptions/Issues/Decisions)
examples/                      - input/output examples
docs/                          - design documentation
CONSTITUTION.md                - 7 rules under which we write skills
SPEC.md                        - technical specification v0.2.0
CHANGELOG.md                   - version history
```

The skills are **composable**: scope-change-controller feeds risk-and-issues-manager (a scope change = a new risk), risk-and-issues-manager signals status-report-drafter (the case RAG may change).

**Composability with [matematic-contract-review-pl](https://github.com/matematicsolutions/matematic-contract-review-pl)**: if the contract-review-pl skill returns red flags in a client's contract portfolio (e.g. an NDA without carve-outs, US law in a supply contract), risk-and-issues-manager-pl can adopt them as new case risks in the RAID register. Two skills orthogonal by topic (case portfolio vs contract portfolio), composable workflow.

## Build and test

The repo has no compilation step - these are Markdown skills for Claude Code and compatible tools. The "test" is running the skill on an **anonymized test case** from [examples/](./examples/) and visually validating the `.docx` output.

Local install (Claude Code):

```bash
cd ~/.claude/skills/
git clone https://github.com/matematicsolutions/lpm-pl
ln -s lpm-pl/skills/status-report-drafter-pl status-report-drafter-pl
ln -s lpm-pl/skills/scope-change-controller-pl scope-change-controller-pl
ln -s lpm-pl/skills/risk-and-issues-manager-pl risk-and-issues-manager-pl
```

On Windows, instead of symlinks - copy the `skills/<name>/` folder to `~/.claude/skills/`.

## Skill-writing rules

- **Email-first input** - you copy a thread from Outlook, the skill extracts signals. Do not assume an API.
- **`.docx` output with the firm's brand template** - the status report must be ready to send, not to rewrite.
- **Every action has a date** - "contact X" does not exist, "by Friday 28.02, if no reply - escalation on Monday 3.03" does.
- **RAG framework** - Red / Amber / Green. Consistent triage across all 3 skills.
- **Judgment calls embedded** - the skill understands when "all good" means "nothing is happening", when activity is not progress, when a gap matters more than an update.
- **No Polish characters in commit messages**.

## What NOT to do (hard rules)

- **DO NOT introduce AFA / phase-based fees** into the skills - in Poland art. 16 KEA ("Kodeks Etyki Adwokackiej", Code of Advocate Ethics) + the absence of an AFA model = do not change Polish billing to match the American upstream.
- **DO NOT name firms or specific rates** in skill outputs.
- **DO NOT send data to the cloud** in example descriptions ([CONSTITUTION.md](./CONSTITUTION.md) rule 1).
- **DO NOT commit real case data** in `examples/` - only anonymized or synthetic.

## Sources of truth (reading order)

1. [README.md](./README.md) - description for humans
2. [CONSTITUTION.md](./CONSTITUTION.md) - 7 constitutional rules for the skills
3. [SPEC.md](./SPEC.md) - technical specification v0.2.0-alpha
4. [CHANGELOG.md](./CHANGELOG.md) - 0.1.0 -> 0.2.0
5. [examples/](./examples/) - real full skill run-throughs

## Agent compatibility

This file (AGENTS.md) is the [agents.md](https://agents.md) standard. The skills are written for Claude Code, but the pattern (email-first input, RAG triage, `.docx` output) is agent-agnostic - you can rewrite it for Cursor / Codex / Devin.

For Claude Code there is additionally a [CLAUDE.md](./CLAUDE.md) file importing this document.

## License and attribution

- **Apache 2.0** - see [LICENSE](./LICENSE).
- Architectural pattern: cherry-pick from [legalopsconsulting/lpm-skills](https://github.com/legalopsconsulting/lpm-skills) (Apache 2.0).
- Skill content: rewritten for the Polish model of firm-client cooperation. This is NOT a 1:1 translation.

Citation: *MateMatic Solutions (2026), lpm-pl - legal project management skills for Polish law firms, https://github.com/matematicsolutions/lpm-pl, Apache 2.0.*
