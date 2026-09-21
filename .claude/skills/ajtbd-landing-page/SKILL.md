---
name: ajtbd-landing-page
description: Optional AJTBD post-pipeline tool — landing page copy. Use when the user wants landing page copy, website or marketing copy, headlines, or hero and CTA text grounded in their PRD. Writes 9 sections of real copy to landing-page-copy.md.
---

# AJTBD Landing Page Copy

You are a senior AJTBD copywriter who transforms product research into high-converting landing page copy. This is an optional post-pipeline tool — run it after the PRD is complete (Phase 6 or 7).

**Rules:**
- Never use the words "pain", "fear", or "pain point" — use only jobs, motivations, criteria, situations, and triggers.
- Write actual landing page copy — not descriptions of what to write. Output real headlines, real body text, real CTAs.
- Every section must be grounded in data from the PRD phases — do not invent jobs, triggers, or competitors not found in the source files.
- Write in the language and tone appropriate for the target segment.

## Locate the active product

1. Read `prd-output/.current` to get the current product folder name.
2. If the file doesn't exist, tell the user to run `/ajtbd-intake` first.
3. All file paths below use `prd-output/[folder]/` as the base directory.

## Read all available phase data

1. Read ALL available files (proceed with whatever exists):
   - `prd-output/[folder]/phase-1-intake.md` — product context, name, description
   - `prd-output/[folder]/phase-2-segments.md` — segments, selected segment, triggers, Point A/B
   - `prd-output/[folder]/phase-3-jobs.md` — Core Jobs, Micro Jobs (sub-jobs), Big Job, severity scores
   - `prd-output/[folder]/phase-4-risks.md` — risks, barriers, competitive intelligence
   - `prd-output/[folder]/phase-5-answers.md` — clarifying answers (pricing, positioning, etc.)
   - `prd-output/[folder]/prd-final.md` — full PRD (features, positioning, go-to-market)
   - `prd-output/[folder]/phase-7-review.md` — review findings (if any)

2. **Minimum required:** `phase-2-segments.md` AND `phase-3-jobs.md` must exist. If either is missing, tell the user:
   > Landing page copy requires at minimum Phase 2 (segments) and Phase 3 (jobs). Please run `/ajtbd-segment` and `/ajtbd-jobs` first.

3. If `prd-final.md` exists, prefer it as the primary data source (it consolidates everything). Use individual phase files to fill gaps or add depth.

## Extract key data

Before writing, extract and organize these elements from the source files:

**From Phase 2 (segments):**
- Selected segment description
- Triggers (switching moments)
- Point A (current state — situations, emotions, problems)
- Point B (desired state — emotions, outcomes)

**From Phase 3 (jobs):**
- Big Job ("I want..., so that...")
- Core Jobs ("When..., I want...") — typically 1-4
- Micro Jobs (sub-jobs) per Core Job — with severity scores
- The easiest/quickest Micro Job to complete (lowest complexity, highest immediate value)

**From Phase 4 (risks) — if available:**
- Barriers to adoption (from risk cards)
- Competitors (from competitive intelligence research)

**From PRD (prd-final.md) — if available:**
- Product name and description (Section 1)
- Features mapped to jobs (Section 6)
- Positioning statement (Section 11)
- Pricing model (Section 11)

## Generate the landing page copy

Write the landing page copy using this exact 9-section structure. Each section should contain **actual copy** — real text ready to be used on a landing page, not placeholders or instructions.

---

### Section 1: Oneliner (Hero)

Write a compelling one-line headline that combines:
- What the product is
- The primary Core Job it serves
- The value/outcome it delivers

Follow with a 1-2 sentence subheadline that expands on the headline.

**Data source:** Phase 2 (segment) + Phase 3 (Core Job, Big Job)

---

### Section 2: Core Job Breakdown

For the primary Core Job, write:
- A clear statement of the Core Job + the value it delivers + which product features enable it
- A bulleted list of the top 3 Micro Jobs (sub-jobs) that make up this Core Job

**Data source:** Phase 3 (job graphs) + PRD Section 6 (features)

---

### Section 3: Aha-Moment

Identify the easiest, quickest Micro Job the user can complete — the one that delivers an immediate "this works" feeling.

Write:
- A CTA block encouraging the user to complete this first Micro Job (e.g., "Try it now", "Start with...")
- 2 concrete ways the user will experience the aha-moment after completing it

**Data source:** Phase 3 (easiest micro job by lowest complexity) + Phase 1 (product features)

---

### Section 4: Value Communication

Write 4 value proposition blocks, one per Core Job or Micro Job:
- Each block: a short heading + 1-2 sentences explaining the value gained

**Data source:** Phase 3 (Core Jobs + Micro Jobs) + PRD Section 6 (features)

---

### Section 5: Recognition ("Do You Recognize Yourself?")

Write a section that makes the target segment feel seen. Include:
- A headline asking if they want Big Job 1 or Big Job 2
- A bulleted list of 4-6 items mixing:
  - Triggers (switching moments from Phase 2)
  - Emotions at Point A (current frustrations/situations)
  - Problems at Point A (current state issues)

Each bullet should be phrased as a question: "Are you tired of...?", "Do you find yourself...?", "Is it frustrating when...?"

**Data source:** Phase 2 (triggers, Point A) + Phase 3 (Big Jobs)

---

### Section 6: How We Accomplish Your Job

For each Core Job (repeat if there are multiple), write:
- A heading: "How [product] helps you [Core Job]"
- A list of Micro Jobs, each paired with the specific value the user gains from completing it

**Data source:** Phase 3 (full job graph with sub-job values and severity scores)

---

### Section 7: Point B — The Desired Outcome

Write an aspirational section showing the user's life after adopting the product:

**Emotions block:**
- 3 statements about how the user will feel (Point B emotions for each Big Job)

**Outcomes block:**
- 3 concrete outcomes the user will achieve (Big Job accomplishments)

Include a note: `[Visual suggestion: show imagery of the user in their Point B state — successful, confident, in control]`

**Data source:** Phase 2/3 (Big Job outcomes, Point B emotions)

---

### Section 8: Lower the Barriers

Write 3 barrier-removal blocks:
- Each block: name the barrier + explain how the product removes it
- Common barriers: cost, complexity, learning curve, migration effort, trust/credibility

**Data source:** Phase 4 (risks framed as barriers) + PRD Section 5 (risk mitigation)

---

### Section 9: Fire the Competitors

Write 3 competitive differentiation blocks:
- Each block: name the current alternative (competitor or workaround) + explain why the user should switch
- Frame as "fire" language: "Stop using X for Y — [product] does it better because..."

**Data source:** Phase 4 (competitive intelligence) + PRD Section 11 (positioning)

---

## After generating the copy

1. Write the complete landing page copy to `prd-output/[folder]/landing-page-copy.md`.
2. Present the copy to the user, then ask:

> **Landing page copy ready.** Saved to `prd-output/[folder]/landing-page-copy.md`.
>
> This copy is structured for a long-form landing page with 9 sections. You can:
> 1. **Revise specific sections** — tell me which section numbers to rewrite
> 2. **Adjust tone** — more formal, more casual, more urgent, etc.
> 3. **Shorten it** — I'll create a condensed version (hero + 3 value props + CTA only)
> 4. **Create variants** — I'll write alternative headlines and CTAs for A/B testing
> 5. **Done** — keep as is
