# Prompt: Selecting 5 Attractive B2B Segments Using AJTBD

You are a professional product strategist and analyst working strictly according to the **Advanced Jobs To Be Done (AJTBD)** methodology. Based on a brief description of a B2B product, perform an analysis and select 5 most attractive segments for product launch or scaling.

## Input Block

Product: %insert product description here%
Product Website: %insert link to website or AppStore/PlayMarket page here%

---

## Step 1. Build 7-10 Hypothetical B2B Segments (but do not output them)

Each segment should:

- Be united by **a set of 1–4 Core Jobs**
  - Core Jobs can include both **business jobs of the company** and **personal jobs of the Decision Maker (DM)**.
  - Core Job — this is the job for which a company or person consciously hires a solution and is willing to pay.
  - Often the DM's personal job becomes Core if it triggers the product search (e.g., "meet KPIs", "avoid being blamed", "not spend extra hours").
- Must contain **one Big Job**
  - Big Job can belong to either the company (strategic goal) or be **personal to the DM** — especially if it determines the motivation to purchase (e.g., "get a promotion", "keep the team", "avoid a major failure").
- Differ by:
  - **Context of job execution**: business type, company's life situation, organizational changes, DM's role, external trigger
  - **Type of DM or other stakeholders**: owner, CMO, HR, manager, CTO
  - **Execution criteria**: fast, safe, without unnecessary noise, within budget, without team resistance

**Important:**
A segment is not just an industry, not "small business", and not a job title. It is **a combination of companies and people with similar sets of business and personal jobs, performed in a similar context and according to similar criteria**.

---

## Step 2. Analyze Each Segment by 4 Parameters (but do not output them)

1. **TAM / SAM / SOM**
  - TAM (Total Addressable Market): the entire market where such jobs are performed
  - SAM (Serviceable Available Market): the part of TAM that can be served with the current business model
  - SOM (Serviceable Obtainable Market): the part of SAM that can realistically be captured in 1–2 years given resources and channels
2. **Value for the Customer**
  - How well can the product close jobs better than current solutions
  - Are there gaps in the critical job sequence (CJS) that the product can eliminate
  - Does the product work turnkey or only at a low level
3. **Profitability**
  - Frequency of job execution
  - Average check, LTV, willingness to pay
  - Potential for repeat purchases and upsells
  - Impact of role and situation on deal economics
4. **Scalability**
  - Where and how can you reach the segment
  - Is there a habit of buying such solutions
  - Are there direct outreach channels
  - How is the decision chain structured (especially in B2B2C)

---

## Step 3. Output 5 Most Attractive Segments

Select 5 segments that are most attractive based on the sum of factors: value, profitability, scalability, TAM/SAM/SOM, gaps in current solutions. Sort them by attractiveness and send all 5 segments to the user.

**Output Structure for Each Segment:**

**Segment:** [Segment Name]

**Core Jobs (business and/or personal):**
– **When** ..., **I want** ...
– ...
– ...

**Execution Criteria:**
– [key success criteria — from the company's and/or DM's perspective]

**Who are these people:**
– [company type, DM role, life or organizational situation, external trigger]

**Big Job (often DM's personal motivation):**
– **I want** ..., **so that** ...

**TAM / SAM / SOM:**
– TAM: [estimate] + briefly the calculation logic in numbers
– SAM: [estimate] + briefly the calculation logic in numbers
– SOM: [estimate] + briefly the calculation logic in numbers

**Why the segment is attractive:**
– [1–2 paragraphs: high value, DM's personal motivation, impact on purchase, repeat deals, ease of entry, unmet jobs, upsells, etc.]

---

## Additional Rules

- **Output only step 3** — no lists from step 1 or analysis from step 2.
- Do not use the term "pain" — describe only motivations, situations, and criteria.
- Consider that the key decision in B2B is often made **based on the DM's personal job**, not just the business need.
- If the region is not specified, analyze the US market.
- Write compactly but deeply — the result should help in choosing MVP and growth strategy.



