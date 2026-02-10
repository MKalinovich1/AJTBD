# AJTBD PRD Generator

A 7-phase pipeline that takes a product idea through AJTBD segmentation, job mapping, and risk assessment — then produces a comprehensive PRD.

## How to use

In Claude Code, run each slash command **in the same conversation**, one at a time. Each phase builds on the prior ones — the AI carries context forward automatically. At the end of each phase, the AI will suggest running the next one.

### Starting a new product

```
/project:prd:1-start My Cool App
```

This creates a dedicated folder for the product (e.g., `prd-output/my-cool-app/`) and walks you through intake. All subsequent phases write to this folder automatically.

### Continuing an existing product

```
/project:prd:1-start
```

If you've already started products before, the command will list them and let you pick one to continue. It shows your saved context and points you to the next incomplete phase — no need to redo intake.

### Output structure

Each product gets its own folder under `prd-output/`:

```
.mcp.json                        ← MCP servers (sequential-thinking, fetch)
prd-output/
├── .current                  ← tracks which product is active
├── my-cool-app/
│   ├── phase-1-intake.md
│   ├── phase-2-segments.md
│   ├── phase-3-jobs.md
│   ├── phase-4-risks.md
│   ├── phase-5-answers.md
│   ├── prd-final.md
│   ├── phase-7-review.md
│   ├── landing-page-copy.md  ← optional
│   ├── feature-specs.md      ← optional
│   ├── interview-script.md   ← optional
│   ├── prd-export.html       ← optional
│   ├── prd-notion.md         ← optional
│   ├── prd-gdocs.html        ← optional
│   └── project-tickets.md    ← optional
└── another-product/
    ├── phase-1-intake.md
    └── ...
```

## Pipeline

| Phase | Command | What it does |
|-------|---------|-------------|
| 1 | `/project:prd:1-start` | Collects product name, description, B2B/B2C, stage, market |
| 2 | `/project:prd:2-segment` | Outputs 5 AJTBD segments, user picks one |
| 3 | `/project:prd:3-jobs` | Maps sub-jobs for all Core Jobs in the chosen segment (parallel sub-agents) |
| 4 | `/project:prd:4-rat` | Identifies Top 5 riskiest assumptions with validation methods |
| 5 | `/project:prd:5-questions` | Asks 20 clarifying questions across 7 categories |
| 6 | `/project:prd:6-generate` | Synthesizes everything into a full Markdown PRD (14 sections) |
| 7 | `/project:prd:7-review` | Reviews PRD with 3 parallel sub-agents, consolidates findings |

### Optional Tools (run after pipeline)

| Command | What it does |
|---------|-------------|
| `/project:extras:landing-page` | Generates AJTBD landing page copy (9 sections) |
| `/project:extras:prd-to-features` | Breaks PRD into individual feature specs with job mapping & acceptance criteria |
| `/project:extras:interview-script` | Generates a user interview script organized by Core Job for validation |
| `/project:extras:prd-export-html` | Exports PRD as a self-contained HTML file with TOC & print styles |
| `/project:extras:prd-export-notion` | Exports PRD to Notion (pushes directly if Notion MCP is configured) |
| `/project:extras:prd-export-gdocs` | Exports PRD as Google Docs-compatible HTML for import |
| `/project:extras:prd-export-tickets` | Converts PRD features into project tickets (Linear/Jira/GitHub Issues) |

## Flow

```
/project:prd:1-start     → user provides product info        → AI confirms
/project:prd:2-segment   → AI outputs 5 segments             → user picks 1
/project:prd:3-jobs      → AI maps sub-jobs (parallel agents) → user confirms
/project:prd:4-rat       → AI outputs 5 risk cards           → user confirms
/project:prd:5-questions → AI asks 20 questions              → user answers
/project:prd:6-generate  → AI outputs full Markdown PRD      → user reviews
/project:prd:7-review    → 3 agents review PRD in parallel   → user picks fixes
```

## Optional Integrations

The pipeline includes two MCP servers out of the box (`.mcp.json`) — Sequential Thinking for structured reasoning and Fetch for enhanced web content extraction. These require no setup.

For exporting to external tools, the pipeline can optionally integrate with:

### Notion

Push your PRD directly to Notion instead of generating a markdown file.

```bash
claude mcp add --transport http notion https://mcp.notion.com/mcp
```

Requires Notion OAuth. Without it, `/project:extras:prd-export-notion` generates `prd-notion.md` instead.

### Google Docs

Push your PRD directly to Google Docs instead of generating an HTML file.

```bash
# Install a Google Docs MCP server (requires Google OAuth)
```

Without it, `/project:extras:prd-export-gdocs` generates `prd-gdocs.html` that you can import manually.

### Linear / Jira / GitHub Issues

Push project tickets directly to your issue tracker.

```bash
# Install your preferred issue tracker MCP server (requires API key)
```

Without it, `/project:extras:prd-export-tickets` generates `project-tickets.md` with structured ticket specs.

## Rules (shared across all phases)

- **No "pain" language** — AJTBD uses jobs, motivations, criteria, situations, triggers. Never "pain", "fear", or "pain point".
- **US market default** — unless the user specifies otherwise in Phase 1.
- **Skip / go back** — user can say "skip" to move past a phase or "go back" to redo one.

## Standalone prompts (`prompts/`)

The original standalone AJTBD prompts. These work independently — use them for one-off analysis without the full pipeline:

- `prompts/segment-b2b-en.md` — B2B segmentation
- `prompts/segment-b2c-en.md` — B2C segmentation
- `prompts/job-graph-en.md` — Job graph
- `prompts/rat-en.md` — Riskiest Assumption Test
- `prompts/questions-en.md` — Clarifying questions for PRD
