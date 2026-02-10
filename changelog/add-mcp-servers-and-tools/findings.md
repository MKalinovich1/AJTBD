# Findings: Landing Page Copy Command

## Date: 2026-02-10

---

## Finding 1: Proposal Already Defines This (S4)
The `proposal-tools-and-skills.md` file lists "S4: landing-page-copy" as a planned skill:
> Takes the selected segment (Phase 2) + Big Job + Core Jobs and generates landing page copy (headline, subhead, 3 value props, CTA) — ready to paste into a landing page builder for RAT validation.

However, the proposal's description is simpler than the actual prompt template the user has. The user's prompt has 9 detailed sections vs the proposal's "headline, subhead, 3 value props, CTA."

## Finding 2: User's Prompt Template Has 9 Sections
The `landing-page-for-segment-en.md` template defines:
1. **Oneliner** — What + Core Job + value
2. **Core Job breakdown** — Core Job + value + features + Micro Jobs
3. **Aha-moment** — easiest first Micro Job + ways to experience it
4. **Value communication** — value per Core Job/Micro Job (4 items)
5. **Recognition** ("Do you recognize yourself?") — Big Jobs, triggers, emotions at Point A, problems at Point A
6. **How we accomplish your job** — Micro Jobs + value gained per each
7. **Point B loading** — emotions at Point B + Big Job accomplishments + visual imagery
8. **Barrier lowering** — barriers + how we remove them
9. **Fire competitors** — competitors + how we replace them

## Finding 3: Existing Pipeline Uses Commands, Not Skills
All 7 phases use `.claude/commands/prd-*.md`. No `.claude/skills/` directory exists. Creating this as a command (`prd-landing-page`) is more consistent.

## Finding 4: Data Availability
The landing page template requires data from multiple phases:
- Phase 2: Segment description, triggers, Point A/B
- Phase 3: Core Jobs, Micro Jobs (sub-jobs), Big Job, job values, severity scores
- Phase 4: Barriers, competitors
- PRD (Phase 6): Features, positioning, pricing

Some sections (barriers, competitors) need Phase 4 competitive intelligence data. The command should gracefully handle missing phases.

## Finding 5: Terminology Compliance
The prompt uses "Problem at Point A" — acceptable per CLAUDE.md (banned: "pain", "fear", "pain point"). No conflicts.

## Finding 6: This Was the User's Own Prompt
The user confirmed they already had this prompt on their desktop (`landing-page-for-segment-en.md`). This is NOT an imported/found skill — it's their original work being wrapped into a pipeline command.
