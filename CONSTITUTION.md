# lpm-pl project Constitution

**Version**: 1.0.0
**Date adopted**: 2026-05-21
**Status**: ratified by Wieslaw Mazur (MateMatic)

The Constitution overrides the specifications of individual skills. If a SKILL.md conflicts with a Constitution article - the Constitution wins. Changing the Constitution requires a SEMVER bump and an entry in CHANGELOG.md.

---

## Mission

Give a Polish law firm a tool that removes from the managing partner the weekly ritual of "assembling reports from 40 cases" without sending client data to the cloud and without every partner having to learn a new SaaS.

## Article 1 - RODO-safe by default

Every skill MUST work on material that the user pastes or points to locally. The skill DOES NOT SEND case files, client correspondence, or personal data to any external API other than the LLM the user deliberately uses (Claude, a local model via Ollama, etc.).

The skill DOES NOT save client data outside the case folder indicated by the user. No telemetry, no "cloud sync", no analytics.

Default mode: offline-compatible. Integrations with M365 / SharePoint / Outlook are OPTIONAL and require the user's deliberate consent with a warning about data transfer.

## Article 2 - Polish billing conditions

The skills DO NOT assume fee models adopted in common-law jurisdictions (AFA - Alternative Fee Arrangements, phase-based budgets, retainer-plus-success). Default models:

- **Hourly rate** (most common) - the skill computes the budget as rate x forecast hours per phase.
- **Flat fee** ("ryczalt", price per case or per stage) - the skill computes flat-fee burn vs work progress.
- **Success fee** - with statutory limits (art. 16 of the Code of Advocate Ethics ("Kodeks Etyki Adwokackiej", KEA); analogously KEZRP for legal counsels). The skill DOES NOT propose pactum de quota litis without a warning about the limits.
- **Hybrid** (rate + success cap) - most common in commercial cases.

The skills DO NOT insert specific rates or names of competing firms. They insert "firm rate", "billing policy", "managing partner".

## Article 3 - Output to .docx with the firm's brand template

Status reports, scope-change requests, risk registers - all produced as **.docx** with the firm's letterhead (the template is defined per deployment in `templates/`).

The skill DOES NOT ask "do you prefer markdown or Word". Industry standard: the firm sends the client a .docx or a PDF generated from a .docx. Markdown inside the chat window is proof that you did nothing.

Exception only when the user EXPLICITLY asks "give me bullets in the chat" / "paste it here". A user's earlier preference for markdown in other skills is NOT sufficient justification.

## Article 4 - Email-first input, format-agnostic

By default the skill accepts pasted threads from Outlook / Gmail / Teams. It also accepts: meeting notes, raw transcripts (from LEX, Otter, Whisper), attached .docx/.xlsx/.pdf. It does not assume a specific email-management workflow - a firm may use Comarch IBARD, Outlook, Apple Mail, anything.

## Article 5 - Judgment calls embedded, not just summarization

The skill is NOT a summarizer. The skill recognizes:

- **Progress vs activity**: "30 emails this week" is activity. "Three rulings overturned, two settlements signed" is progress. Activity without progress = an escalation signal.
- **Silence as a signal**: a lack of updates is a status, not a gap. "No reply from the court for 3 weeks" is reportable.
- **A mandatory deadline for every action**: "contact the expert" does not exist. "By Friday 28.02; if no reply - escalation on Monday 3.03" does.
- **Forward-looking RAG**: the color assesses the risk that the case will not deliver a milestone in the next 2 weeks, not whether there was a lot of work in the last 2 weeks.
- **Escalation triage**: team-resolves / partner-decides / client-must-know. Test: "if the case goes wrong and the partner/client did not know - is that our reporting fault?".

## Article 6 - Composability over completeness

Better to have 3 well-composed skills (status reads from the risk register, scope feeds status) than 16 separated ones. Every new skill MUST declare:

- **Reads from**: which skills / files it consumes.
- **Writes to**: which skills / files it produces.
- **Handoff points**: where it touches other skills.

No silos. No "I do everything myself".

## Article 7 - Pattern attribution and content independence

The architectural pattern (email-first input, output to the case folder, judgment calls embedded, composability, RAG framework, escalation triage) is a cherry-pick from [legalopsconsulting/lpm-skills](https://github.com/legalopsconsulting/lpm-skills) under the Apache 2.0 license snapshot. Attribution in README.md and every SKILL.md.

The skill content (specific judgment calls, terminology, billing models, integrations, Polish legal aspects) is written from scratch for the Polish model of firm-client cooperation. This is NOT a 1:1 translation - it is a rewrite.

The upstream announces a move to AGPL-3.0 in future versions. lpm-pl stays on the Apache 2.0 snapshot from the cherry-pick date (2026-05-21). Later upstream changes are NOT automatically pulled in.

---

## MateMatic gates (checked before every merge)

1. **License**: per Art. 7. New dependencies only permissive (MIT/Apache/BSD/CC BY).
2. **Anti-OS**: zero ToS bypass, zero scraping of services without consent, zero "hidden" transfer of client data.
3. **Quality**: every skill has a test case on an anonymized case + passes the maintainers' internal QA pipeline (PL proofreading, readability, no slop patterns).
4. **Strategy**: the skill solves a real pain of a Polish law firm. If it requires 3 SaaS subscriptions at 400 USD/month - we reject it.

## Roles

- **Product owner**: Wieslaw Mazur (MateMatic) - strategic decisions, ratification of the Constitution.
- **Maintainer**: the MateMatic team - PR review, release, documentation.
- **Content reviewer**: MateMatic maintainers' internal QA pipeline (charge rigor + anti-slop for Polish) before every merge.
- **Legal validator**: a licensed lawyer designated by the client - every firm ratifies the skill before production use.

## Antigoals (what we do NOT do)

- We do NOT build a SaaS - these are local skills, the firm installs them on its own machines.
- We do NOT compete with LEX/LegalBase/Mecenas IT - we are the AI layer **on top of** those systems, not instead of them.
- We do NOT take legal responsibility for the output - the skill is a draft for a lawyer to accept.
- We do NOT build a time-tracker or a billing system - the skill consumes data from an existing system (Mecenas/Kancelaria/Excel), it does not generate it.

---

## Refinement log

- **2026-05-21** - Constitution v1.0.0 ratified. Cherry-pick of the pattern from lpm-skills Apache 2.0 snapshot. MVP: 3 skills (status/scope/risk), starting only with status-report-drafter-pl.
