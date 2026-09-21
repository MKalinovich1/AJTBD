# AJTBD PRD Generator

This project is a 7-phase pipeline that takes a product idea through AJTBD (Advanced Jobs To Be Done) segmentation, job mapping, risk assessment, and PRD generation. Every phase is packaged as a **Claude Code Agent Skill** under `.claude/skills/`, so Claude can invoke a phase automatically when the request matches its `description`, and the user can also invoke one explicitly by name (e.g. `/ajtbd-segment`).

## Skill Layout

```
.claude/skills/
├── ajtbd-intake/SKILL.md            ← Phase 1
├── ajtbd-segment/SKILL.md           ← Phase 2
├── ajtbd-jobs/SKILL.md              ← Phase 3
├── ajtbd-risks/SKILL.md             ← Phase 4
├── ajtbd-questions/SKILL.md         ← Phase 5
├── ajtbd-prd/SKILL.md               ← Phase 6
├── ajtbd-review/SKILL.md            ← Phase 7
├── ajtbd-landing-page/SKILL.md      ← optional
├── ajtbd-feature-specs/SKILL.md     ← optional
├── ajtbd-interview-script/SKILL.md  ← optional
├── ajtbd-export-html/SKILL.md       ← optional
├── ajtbd-export-notion/SKILL.md     ← optional
├── ajtbd-export-gdocs/SKILL.md      ← optional
└── ajtbd-export-tickets/SKILL.md    ← optional
```

Each `SKILL.md` has YAML frontmatter (`name`, `description`) followed by the full phase instructions. The `description` is the only part Claude sees until the skill is invoked — it must state both what the skill does and when to use it. When editing a phase, edit its `SKILL.md`; there is no separate command file.

## Terminology Rules (MANDATORY — all phases)

- **NEVER** use the words "pain", "fear", or "pain point" anywhere in outputs
- Use only: jobs, motivations, criteria, situations, and triggers
- Core Job format: **"When ..., I want ..."**
- Big Job format: **"I want ..., so that ..."**
- A segment is NOT a demographic, industry, or role — it is people/companies with similar jobs and similar execution criteria

## Pipeline Overview

| Phase | Skill | Output File | Key Action |
|-------|-------|-------------|------------|
| 1 | `ajtbd-intake` | `phase-1-intake.md` | Collect product info |
| 2 | `ajtbd-segment` | `phase-2-segments.md` | Generate 5 segments, user picks 1 |
| 3 | `ajtbd-jobs` | `phase-3-jobs.md` | Map sub-jobs (uses parallel sub-agents) |
| 4 | `ajtbd-risks` | `phase-4-risks.md` | 5 risk cards with P×I scoring |
| 5 | `ajtbd-questions` | `phase-5-answers.md` | 20 clarifying questions |
| 6 | `ajtbd-prd` | `prd-final.md` | Full 14-section PRD |
| 7 | `ajtbd-review` | `phase-7-review.md` | 3 parallel review agents |

### Optional Tools (run after pipeline)

| Skill | Output File | Key Action |
|-------|-------------|------------|
| `ajtbd-landing-page` | `landing-page-copy.md` | AJTBD landing page copy (9 sections) |
| `ajtbd-feature-specs` | `feature-specs.md` | Individual feature specs with job mapping & acceptance criteria |
| `ajtbd-interview-script` | `interview-script.md` | User interview script organized by Core Job |
| `ajtbd-export-html` | `prd-export.html` | Self-contained HTML export with TOC & print styles |
| `ajtbd-export-notion` | `prd-notion.md` | Notion-formatted export (pushes via MCP if configured) |
| `ajtbd-export-gdocs` | `prd-gdocs.html` | Google Docs-optimized HTML import file |
| `ajtbd-export-tickets` | `project-tickets.md` | Project tickets for Linear/Jira/GitHub Issues |

## File Structure

```
prd-output/
├── .current                  ← active product folder name (plain text)
├── my-cool-app/
│   ├── phase-1-intake.md
│   ├── phase-2-segments.md
│   ├── phase-3-jobs.md
│   ├── phase-4-risks.md
│   ├── phase-5-answers.md
│   ├── prd-final.md
│   ├── phase-7-review.md
│   ├── landing-page-copy.md  ← optional (ajtbd-landing-page)
│   ├── feature-specs.md      ← optional (ajtbd-feature-specs)
│   ├── interview-script.md   ← optional (ajtbd-interview-script)
│   ├── prd-export.html       ← optional (ajtbd-export-html)
│   ├── prd-notion.md         ← optional (ajtbd-export-notion)
│   ├── prd-gdocs.html        ← optional (ajtbd-export-gdocs)
│   └── project-tickets.md    ← optional (ajtbd-export-tickets)
└── another-product/
    └── ...
```

- `prd-output/.current` tracks which product is active — all phases read this first
- Each product gets its own subfolder under `prd-output/`
- Phase files are numbered and must be created in order (each phase reads prior ones)

## Shared Rules (all phases)

1. **Default market:** United States (unless user specifies otherwise in Phase 1)
2. **Phase continuity:** Every phase reads `prd-output/.current` first, then loads prior phase files. If a required file is missing, tell the user which skill to invoke (e.g. `/ajtbd-jobs`)
3. **User control:** User can say "skip" to move past a phase or "go back" to redo one
4. **B2B vs B2C:** Determined in Phase 1, affects segmentation methodology in Phase 2

## Sub-Agent Architecture

Four phases use parallel sub-agents for speed and depth:

### Phase 2 (`ajtbd-segment`) — Market Research
- Spawns **1 sub-agent** after selecting top 5 candidates internally
- Agent uses WebSearch to find TAM/SAM/SOM data, competitors, pricing benchmarks, growth signals
- Main agent enriches segments with sourced data before presenting to user

### Phase 3 (`ajtbd-jobs`) — Parallel Job Mapping
- Spawns **one sub-agent per Core Job** (1-4 agents)
- All launched simultaneously via Task tool
- Each agent independently maps the full sub-job sequence for its Core Job
- Main agent consolidates, checks cross-job consistency, presents to user

### Phase 4 (`ajtbd-risks`) — Competitive Intelligence
- Spawns **2 sub-agents** simultaneously:
  1. Competitive Research — direct/indirect competitors, pricing, user sentiment, failure stories
  2. Market Validation — demand signals, market reports, regulatory landscape, recent funding/M&A
- Main agent uses research to calibrate P and I scores with evidence before producing risk cards

### Phase 7 (`ajtbd-review`) — Parallel Review
- Spawns **3 sub-agents** simultaneously:
  1. Product & Strategy Review
  2. Business & Go-to-Market Review
  3. Technical & Analytics Review
- Main agent deduplicates and consolidates findings

## Standalone Prompts

The `prompts/` directory contains standalone AJTBD prompts that work independently (outside the pipeline):
- `segment-b2b-en.md` — B2B segmentation
- `segment-b2c-en.md` — B2C segmentation
- `job-graph-en.md` — Job graph mapping
- `rat-en.md` — Riskiest Assumption Test
- `questions-en.md` — Clarifying questions for PRD
- `landing-page-en.md` — Landing page copy structure

## Adding or Editing a Skill

1. Create `.claude/skills/<skill-name>/SKILL.md` — the directory name must match the `name` in frontmatter (lowercase, hyphens only).
2. Write a `description` that names the trigger conditions, not just the output: Claude matches the user's request against it.
3. Keep every phase self-contained: it reads `prd-output/.current`, validates prior phase files, and names the next skill to run.
4. Run the `skill-judge` skill to audit design quality before committing.
