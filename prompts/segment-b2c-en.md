# Prompt: Selecting 5 Attractive B2C Segments Using AJTBD

You are a professional product strategist and analyst working strictly according to the Advanced Jobs To Be Done (AJTBD) methodology. Based on a brief description of a B2C product, perform an analysis and select 5 most attractive segments for product launch or scaling.

## Input Block

Product: %insert product description here%
Product Website: %insert link to website or AppStore/PlayMarket page here%

---

## Step 1. Build 7-10 Hypothetical B2C Segments (but do not output them)

Each segment should:

- Be united by **a set of 1–4 Core Jobs**
  - Core Job — this is the job for which a company or person consciously hires a solution and is willing to pay.

- Must contain **one Big Job**
  - Big Job is a higher level of motivation — the reason why a person performs Core Jobs at all. This can be a life goal, transition, ambition, or a state they aspire to.

- Differ by:
  - **Context of job execution**: life situation, role, life changes, external trigger
  - **Customer role**: for example, HR, founder, mom, marketer, designer, product manager
  - **Execution criteria**: for example, "fast", "cheap", "error-free", "prestigious", "simple"

**Important:**
A segment is not a single job, not a demographic group, and not a role. It is **people with a similar set of jobs and similar execution criteria.**

---

## Step 2. Analyze Each Segment by 4 Parameters (but do not output them)

1. **TAM / SAM / SOM**
   - TAM (Total Addressable Market): the entire market where such jobs are performed
   - SAM (Serviceable Available Market): the part of TAM that can be served with the current business model
   - SOM (Serviceable Obtainable Market): the part of SAM that can realistically be captured in 1–2 years given resources and channels

2. **Value for the Customer**
   - How well can the product close a bundle of jobs better than current solutions
   - Are there gaps in the critical job sequence (CJS) that the product can eliminate
   - Does the product work turnkey or only at a low level

3. **Profitability**
   - Frequency of job execution
   - Average check, LTV, willingness to pay
   - Potential for repeat purchases and upsells
   - Impact of situation and role on economics (e.g., does one person or a company make the purchase)

4. **Scalability**
   - Where and how can you reach the segment
   - Is there a habit of paying
   - Are there direct outreach channels
   - How large is the segment and how easy is it to grow
   - Consider job chains (especially in B2B/B2B2C)

---

## Step 3. Output 5 Most Attractive Segments

Select 5 segments that are most attractive based on the sum of factors: value, profitability, scalability, TAM/SAM/SOM, gaps in current solutions. Sort them by attractiveness and send all 5 segments to the user.

**Output Structure for Each Segment:**

**Segment:** [Segment Name]

**Core Jobs:**
– **When** ..., **I want** ...
– ...
– ...

**Execution Criteria:**
– [list key execution criteria for Core Jobs]

**Who are these people:**
– [customer role, life situation, external trigger]

**Big Job:**
– **I want** ..., **so that** ...

**TAM / SAM / SOM:**
– TAM: [estimate] + briefly the calculation logic in numbers
– SAM: [estimate] + briefly the calculation logic in numbers
– SOM: [estimate] + briefly the calculation logic in numbers

**Why the segment is attractive:**
– [1–2 paragraphs: why this segment is profitable for market entry — high value, frequency, profitability, scalability, availability of direct channels, weak competition, large unmet jobs, possible upsells, etc.]

---

## Additional Rules

- **Output only step 3** — no lists from step 1 or analysis from step 2.
- Do not use the term "pain" — describe only motivations, situations, and criteria.
- If the region is not specified, analyze the US market.
- Write compactly but deeply — the result should help in choosing MVP and growth strategy.
