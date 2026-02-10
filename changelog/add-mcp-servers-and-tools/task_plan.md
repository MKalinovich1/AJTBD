# Task Plan: Landing Page Copy Command

## Objective
Create `/project:prd-landing-page` — an optional post-pipeline command that generates AJTBD-structured landing page copy from PRD data.

## Status: READY FOR APPROVAL

## Decisions Made
- **Format:** Command (`.claude/commands/prd-landing-page.md`)
- **Standalone prompt:** Yes, copy to `prompts/landing-page-en.md`

---

## Implementation Steps

### Step 1: Create `.claude/commands/prd-landing-page.md`
The command will:
1. Read `prd-output/.current` to locate the active product
2. Read all available phase files (phase-1 through phase-7 + prd-final.md)
3. Validate that at minimum Phase 2 (segments) and Phase 3 (jobs) exist
4. Generate landing page copy following the 9-section AJTBD template
5. Write output to `prd-output/[folder]/landing-page-copy.md`

The 9 sections and their data sources:
1. **Oneliner** → Phase 2 segment + Phase 3 Core Job/Big Job
2. **Core Job + features + Micro Jobs** → Phase 3 job graphs + PRD Section 6
3. **Aha-moment** → Phase 3 (easiest micro job) + Phase 1 product info
4. **Value communication** → Phase 3 Core Jobs/Micro Jobs + PRD features
5. **Recognition ("Do you recognize yourself?")** → Phase 2 triggers + Phase 3 Point A
6. **How we accomplish your job** → Phase 3 full job graph
7. **Point B loading** → Phase 2/3 Big Job outcomes + Point B emotions
8. **Lower barriers** → Phase 4 risks + PRD Section 5
9. **Fire competitors** → Phase 4 competitive intel + PRD Section 11

### Step 2: Copy standalone prompt to `prompts/landing-page-en.md`
Copy the user's `landing-page-for-segment-en.md` to `prompts/landing-page-en.md` (matching existing naming convention: `{topic}-en.md`).

### Step 3: Update CLAUDE.md
- Add "Optional Tools" section after the pipeline table with the landing page command
- Add `landing-page-copy.md` to the file structure
- Add `landing-page-en.md` to the standalone prompts list

---

## Files to Create/Modify
| Action | File |
|--------|------|
| CREATE | `.claude/commands/prd-landing-page.md` |
| CREATE | `prompts/landing-page-en.md` |
| EDIT | `CLAUDE.md` (add optional tools section + update file structure + update prompts list) |
