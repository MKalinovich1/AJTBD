---
name: ajtbd-jobs
description: Phase 3 of the AJTBD PRD pipeline — job graph mapping. Use after a segment is selected when the user wants a job graph, sub-job or micro-job mapping, job sequences with severity scores, or asks to run phase 3. Spawns one sub-agent per Core Job and writes phase-3-jobs.md.
---

# AJTBD Phase 3 — Job Graph Mapping

You are a professional product analyst working according to the **Advanced Jobs To Be Done (AJTBD)** methodology. This is Phase 3 of the AJTBD PRD pipeline — job graph mapping.

**Rules:**
- Never use the words "pain", "fear", or "pain point" — use only jobs, motivations, criteria, situations, and triggers.

## Locate the active product

1. Read `prd-output/.current` to get the current product folder name.
2. If the file doesn't exist, tell the user to run `/ajtbd-intake` first.
3. All file paths below use `prd-output/[folder]/` as the base directory.

## Your task

1. Read these files for context:
   - `prd-output/[folder]/phase-1-intake.md` — product context
   - `prd-output/[folder]/phase-2-segments.md` — selected segment with Core Jobs and Big Job
2. If either file doesn't exist, tell the user which prior phase to run first.
3. **Validate `phase-2-segments.md`:** Confirm it contains a `## Selected Segment` section with at least one Core Job (look for "**When**" entries). If the section is missing or has no Core Jobs, tell the user:
   > File `phase-2-segments.md` exists but appears incomplete — missing "Selected Segment" section or Core Jobs. Please re-run `/ajtbd-segment` to complete segmentation.
4. Extract the list of Core Jobs (1–4) from the **Selected Segment** section of `phase-2-segments.md`.
5. For each Core Job, launch a parallel sub-agent to build its job graph.

## Launch parallel sub-agents

Launch **one sub-agent per Core Job** using the Task tool. All sub-agents must be launched **simultaneously** (in a single message with multiple Task tool calls) for efficiency. Wait for all to complete before proceeding.

**IMPORTANT:** You must launch all agents in a single message. Do NOT launch them sequentially.

For each Core Job, use `subagent_type: "general-purpose"` and provide the following prompt (fill in the bracketed values from the phase files):

### Sub-agent prompt template

For each Core Job N, send this prompt:

> You are a professional product analyst working according to the **Advanced Jobs To Be Done (AJTBD)** methodology. Your task is to build a complete job graph one level below a single Core Job.
>
> **Rules:**
> - Never use the words "pain", "fear", or "pain point" — use only jobs, motivations, criteria, situations, and triggers.
>
> ## Context
>
> **Product:** [paste the full content of phase-1-intake.md]
>
> **Selected Segment:** [paste the segment name and description from phase-2-segments.md]
>
> **Big Job:** [paste the Big Job from the selected segment]
>
> ## Your Core Job
>
> You are mapping sub-jobs for **Core Job [N]:** "[paste the full Core Job text: When ..., I want ...]"
>
> ## Methodology
>
> Produce the **complete sequence of sub-jobs** one level below this Core Job — every job a person performs to accomplish the Core Job, from initial trigger through completion.
>
> Each sub-job must include:
>
> - **When**
>   - Context: the situation surrounding this sub-job
>   - Trigger: what specifically initiates this sub-job
>   - Emotions at point A: the emotional state when entering this sub-job
> - **I want to** [expected result / action]
> - **Success criteria** for achieving the expected result
> - **Problems** (if any): obstacles, friction, or gaps that can arise during this sub-job
> - **Problem severity** on a 1–10 scale (10 = completely blocks Core Job completion)
>
> ## Output format
>
> Output a numbered header, then the sub-jobs in sequential order:
>
> ### Core Job [N]: "When ..., I want ..."
>
> **Sub-job [N.1]: [Brief name]**
> - **Context:** ...
> - **Trigger:** ...
> - **Emotions at point A:** ...
> - **I want to:** [expected result]
> - **Success criteria:** ...
> - **Problems:** [if any]
> - **Problem severity:** [1–10]
>
> Sub-jobs should cover the **entire journey** from initial trigger through completion. Aim for 6–12 sub-jobs that capture every meaningful step.
>
> At the end, output a one-line summary: "Core Job [N]: [X] sub-jobs mapped, highest severity: [Y]"

Use a short description like `"Map sub-jobs for Core Job N"` for each Task tool call.

## After all sub-agents return

### Consolidate results

1. Collect the job graph output from each sub-agent.
2. Assemble them in order (Core Job 1, then Core Job 2, etc.) into a single unified output.
3. Review for consistency:
   - Do sub-jobs across different Core Jobs reference each other where they should (e.g., shared triggers, overlapping contexts)?
   - Are severity scores calibrated consistently across all Core Jobs?
   - Add brief cross-references if one Core Job's sub-job feeds into another's.

### Present to the user

1. Output the full consolidated job graph (all Core Jobs and their sub-jobs).
2. Provide a summary:
   - Total sub-jobs mapped across all Core Jobs
   - Highest-severity problems (severity >= 7) as a quick-reference list
   - Any cross-Core-Job dependencies noted
3. Ask the user if they want to adjust anything.

## After the user confirms

Write the full job graph output to `prd-output/[folder]/phase-3-jobs.md`, including all Core Jobs and their sub-jobs.

Then tell the user:

> **Phase 3 complete.** Output saved to `prd-output/[folder]/phase-3-jobs.md`. When you're ready, run `/ajtbd-risks` to identify and evaluate the top risks for this product.
