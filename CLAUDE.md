# AJTBD PRD Generator

This project is a 7-phase pipeline that takes a product idea through AJTBD (Advanced Jobs To Be Done) segmentation, job mapping, risk assessment, and PRD generation using Claude Code slash commands.

## Terminology Rules (MANDATORY — all phases)

- **NEVER** use the words "pain", "fear", or "pain point" anywhere in outputs
- Use only: jobs, motivations, criteria, situations, and triggers
- Core Job format: **"When ..., I want ..."**
- Big Job format: **"I want ..., so that ..."**
- A segment is NOT a demographic, industry, or role — it is people/companies with similar jobs and similar execution criteria

## Pipeline Overview

| Phase | Command | Output File | Key Action |
|-------|---------|-------------|------------|
| 1 | `/project:prd:1-start` | `phase-1-intake.md` | Collect product info |
| 2 | `/project:prd:2-segment` | `phase-2-segments.md` | Generate 5 segments, user picks 1 |
| 3 | `/project:prd:3-jobs` | `phase-3-jobs.md` | Map sub-jobs (uses parallel sub-agents) |
| 4 | `/project:prd:4-rat` | `phase-4-risks.md` | 5 risk cards with P×I scoring |
| 5 | `/project:prd:5-questions` | `phase-5-answers.md` | 20 clarifying questions |
| 6 | `/project:prd:6-generate` | `prd-final.md` | Full 14-section PRD |
| 7 | `/project:prd:7-review` | `phase-7-review.md` | 3 parallel review agents |

### Optional Tools (run after pipeline)

| Command | Output File | Key Action |
|---------|-------------|------------|
| `/project:extras:landing-page` | `landing-page-copy.md` | AJTBD landing page copy (9 sections) |
| `/project:extras:prd-to-features` | `feature-specs.md` | Individual feature specs with job mapping & acceptance criteria |
| `/project:extras:interview-script` | `interview-script.md` | User interview script organized by Core Job |
| `/project:extras:prd-export-html` | `prd-export.html` | Self-contained HTML export with TOC & print styles |
| `/project:extras:prd-export-notion` | `prd-notion.md` | Notion-formatted export (pushes via MCP if configured) |
| `/project:extras:prd-export-gdocs` | `prd-gdocs.html` | Google Docs-optimized HTML import file |
| `/project:extras:prd-export-tickets` | `project-tickets.md` | Project tickets for Linear/Jira/GitHub Issues |

## File Structure

```
.mcp.json                        ← MCP servers (sequential-thinking, fetch)
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
│   ├── landing-page-copy.md  ← optional (extras:landing-page)
│   ├── feature-specs.md      ← optional (extras:prd-to-features)
│   ├── interview-script.md   ← optional (extras:interview-script)
│   ├── prd-export.html       ← optional (extras:prd-export-html)
│   ├── prd-notion.md         ← optional (extras:prd-export-notion)
│   ├── prd-gdocs.html        ← optional (extras:prd-export-gdocs)
│   └── project-tickets.md    ← optional (extras:prd-export-tickets)
└── another-product/
    └── ...
```

- `prd-output/.current` tracks which product is active — all phases read this first
- Each product gets its own subfolder under `prd-output/`
- Phase files are numbered and must be created in order (each phase reads prior ones)

## Shared Rules (all phases)

1. **Default market:** United States (unless user specifies otherwise in Phase 1)
2. **Phase continuity:** Every phase reads `prd-output/.current` first, then loads prior phase files. If a required file is missing, tell the user which `/project:prd:N-X` command to run
3. **User control:** User can say "skip" to move past a phase or "go back" to redo one
4. **B2B vs B2C:** Determined in Phase 1, affects segmentation methodology in Phase 2

## Sub-Agent Architecture

Four phases use parallel sub-agents for speed and depth:

### Phase 2 (prd-segment) — Market Research
- Spawns **1 sub-agent** after selecting top 5 candidates internally
- Agent uses WebSearch to find TAM/SAM/SOM data, competitors, pricing benchmarks, growth signals
- Main agent enriches segments with sourced data before presenting to user

### Phase 3 (prd-jobs) — Parallel Job Mapping
- Spawns **one sub-agent per Core Job** (1-4 agents)
- All launched simultaneously via Task tool
- Each agent independently maps the full sub-job sequence for its Core Job
- Main agent consolidates, checks cross-job consistency, presents to user

### Phase 4 (prd-rat) — Competitive Intelligence
- Spawns **2 sub-agents** simultaneously:
  1. Competitive Research — direct/indirect competitors, pricing, user sentiment, failure stories
  2. Market Validation — demand signals, market reports, regulatory landscape, recent funding/M&A
- Main agent uses research to calibrate P and I scores with evidence before producing risk cards

### Phase 7 (prd-review) — Parallel Review
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
