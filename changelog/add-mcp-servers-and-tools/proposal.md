# Proposal: MCP Servers & Skills for AJTBD Pipeline

## Date: 2026-02-10
## Status: FULLY IMPLEMENTED

---

## Category 1: MCP Servers for `.mcp.json` (zero auth, works for everyone)

### M1: Sequential Thinking ✅ DONE
- **Package:** `@modelcontextprotocol/server-sequential-thinking`
- **What it adds:** Structured multi-step reasoning tool
- **Why it helps:** Phases 2 and 4 involve complex analytical reasoning (scoring 7-10 segments by 4 parameters, calculating P×I risk scores). This gives the model a scratchpad for deliberate step-by-step analysis instead of doing it all in a single generation. No API key needed — runs locally via npx.
- **Install:** `claude mcp add --transport stdio --scope project sequential-thinking -- npx -y @modelcontextprotocol/server-sequential-thinking`

### M2: Fetch ✅ DONE
- **Package:** `@anthropic/mcp-fetch`
- **What it adds:** Enhanced web fetching with full page content extraction
- **Why it helps:** Sub-agents in Phases 2 and 4 use WebSearch, but sometimes need to read the actual page (e.g., a competitor's pricing page, a market report). Built-in WebFetch exists but this MCP version handles JS-rendered pages and larger content better. No API key needed.
- **Install:** `claude mcp add --transport stdio --scope project fetch -- npx -y @anthropic/mcp-fetch`

---

## Category 2: MCP Servers needing per-user auth (document in README, don't auto-install)

### M3: Notion ✅ DONE (as command)
- **URL:** `https://mcp.notion.com/mcp`
- **What it adds:** Export final PRD to Notion for stakeholder sharing
- **Auth:** Notion OAuth (optional — command works without it, generates file instead)
- **Command:** `/project:extras:export-notion` — pushes via MCP if configured, otherwise generates `prd-notion.md`
- **Install MCP (optional):** `claude mcp add --transport http notion https://mcp.notion.com/mcp`

### M4: Google Docs ✅ DONE (as command)
- **Package:** `google-docs-mcp`
- **What it adds:** Export final PRD to Google Docs
- **Auth:** Google OAuth (optional — command works without it, generates importable HTML)
- **Command:** `/project:extras:export-gdocs` — pushes via MCP if configured, otherwise generates `prd-gdocs.html`

### M5: Linear / Jira ✅ DONE (as command)
- **What it adds:** Convert PRD features into project tickets
- **Auth:** API key (optional — command works without it, generates `project-tickets.md`)
- **Command:** `/project:extras:export-tickets` — pushes via MCP if configured, otherwise generates ticket specs file

---

## Category 3: Project Skills for `.claude/skills/` (works for everyone, no auth)

### S1: prd-to-features ✅ DONE
- **What it does:** Takes `prd-final.md` Section 6 (Feature Requirements) and generates individual feature spec files — one `.md` per feature with job mapping, acceptance criteria, and technical notes
- **When to use:** After Phase 6 or 7. Natural next step after PRD is done.
- **Location:** `.claude/commands/extras/prd-to-features.md` (implemented as command, not skill)

### S2: interview-script ✅ DONE
- **What it does:** Takes Phase 3 job graphs and generates a user interview script organized by Core Job — questions designed to validate whether the jobs/sub-jobs are real and the severity scores are accurate
- **When to use:** After Phase 3 or during Phase 4 RAT validation. Directly supports the "solution interviews" validation method.
- **Location:** `.claude/commands/extras/interview-script.md` (implemented as command, not skill)

### S3: prd-export ✅ DONE
- **What it does:** Formats `prd-final.md` as a clean, self-contained HTML file with table of contents, styled tables, and print-friendly layout. Opens in browser.
- **When to use:** After Phase 6 or 7 when sharing with stakeholders who don't use markdown.
- **Location:** `.claude/commands/extras/prd-export.md` (implemented as command, not skill)

### S4: landing-page-copy ✅ DONE
- **What it does:** Takes the selected segment (Phase 2) + Big Job + Core Jobs and generates landing page copy (headline, subhead, 3 value props, CTA) — ready to paste into a landing page builder for RAT validation
- **When to use:** During or after Phase 4. Directly supports the "landing pages with test traffic" validation method in RAT.
- **Location:** `.claude/commands/extras/landing-page.md` (implemented as command)

---

## Recommendation

**Install into `.mcp.json`** (works for everyone, no setup):
- M1 (Sequential Thinking)
- M2 (Fetch)

**Install as project skills** (works for everyone, no setup):
- S1 (prd-to-features)
- S2 (interview-script)
- S3 (prd-export)
- S4 (landing-page-copy)

**Document in README as optional** (needs per-user auth):
- M3/M4/M5 (Notion, Google Docs, Linear/Jira)

---

## Implementation Notes

### .mcp.json format (project root)
```json
{
  "mcpServers": {
    "sequential-thinking": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"],
      "env": {}
    },
    "fetch": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-fetch"],
      "env": {}
    }
  }
}
```

### Skills directory structure
```
.claude/skills/
├── prd-to-features/
│   └── SKILL.md
├── interview-script/
│   └── SKILL.md
├── prd-export/
│   └── SKILL.md
└── landing-page-copy/
    └── SKILL.md
```

### README additions ✅ DONE
- "Optional Integrations" section with install commands for M3/M4/M5
- Updated project structure showing `.mcp.json`
- Updated CLAUDE.md file structure to match

---

## Sources
- [Claude Code MCP Docs](https://code.claude.com/docs/en/mcp)
- [Claude Code Skills Docs](https://code.claude.com/docs/en/skills)
- [MCP Registry](https://github.com/modelcontextprotocol/servers)
