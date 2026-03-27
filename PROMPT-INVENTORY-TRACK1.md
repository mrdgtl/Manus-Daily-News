# PROMPT INVENTORY: TRACK 1 (Briefing Engine)

This document provides a comprehensive inventory of every agent, stage, and workflow component in the Manus Daily News briefing engine (Track 1). It details the exact prompts, inputs, outputs, hidden rules, and alignment status for each component based on the current system architecture and historical outputs.

---

## STAGE 1: NEWS CRAWLER / RESEARCH

**1. Component Name:** NewsCrawler Agent
**2. Purpose:** To discover and retrieve recent AI-related articles from a predefined list of high-quality sources using parallel web research.
**3. Exact Current Prompt / Instruction Block (Implicit in Orchestration):**
> "Execute parallel searches across 10+ predefined source categories. Target sources include:
> - Company blogs (OpenAI, Google DeepMind, Anthropic, Meta AI, Microsoft, Apple)
> - Tech news sites (TechCrunch, The Verge, Wired, Ars Technica, VentureBeat)
> - Research outlets (arXiv, Google Research Blog)
> - Policy sources (White House, EU AI Act updates, Congressional records)
> - Safety/ethics (AI safety organizations, EFF)
> - Business/funding (Crunchbase, Bloomberg)
> 
> Retrieve 20-30 raw stories published within the specified time window (past 24-48 hours). Ignore paywalled content if full text cannot be extracted."
**4. Inputs It Expects:** List of target URLs/RSS feeds, time window (e.g., "past 24 hours").
**5. Outputs It Produces:** Raw text content and metadata (title, source, date, URL) for 20-30 discovered articles.
**6. Hidden Rules / Quality Checks:** 
- **Paywall Bypass Rule:** Ignore paywalled content if the full text cannot be extracted.
- **MapReduce Pattern:** Searches are executed in parallel across the 10+ categories to optimize retrieval time.
**7. Alignment Status:** Aligned with current research specs, but operates independently of the new sheet-first workflow.

---

## STAGE 2: RELEVANCE FILTER

**1. Component Name:** RelevanceFilter Agent
**2. Purpose:** To sift through the 20-30 raw articles and select the 8-12 most important, high-impact stories.
**3. Exact Current Prompt / Instruction Block:**
> "Evaluate each story against specific criteria: impact scope, novelty, source credibility, and category diversity. Prioritize major model releases, significant policy changes, major business moves (M&A, large funding, layoffs), and breakthroughs in research. Discard minor updates, opinion pieces without news value, and duplicate coverage of the same event. Select the top 8-12 stories for the final briefing."
**4. Inputs It Expects:** Raw articles and metadata from the NewsCrawler (20-30 items).
**5. Outputs It Produces:** A filtered list of 8-12 top articles.
**6. Hidden Rules / Quality Checks:**
- **Source Diversity Check:** Ensure no single media outlet (e.g., TechCrunch) represents more than 40% of the selected stories.
- **Deduplication Logic:** Discard duplicate coverage of the exact same event, prioritizing the most credible or comprehensive source.
**7. Alignment Status:** **NOT ALIGNED.** The current prompt filters for general "impact" but fails to execute the aggressive scoring rules required by the `OPERATIONAL-WORKFLOW.md` (Viral Potential, Emotional Impact, Audience Relevance).

---

## STAGE 3: SUMMARIZER

**1. Component Name:** Summarizer Agent
**2. Purpose:** To condense the selected articles into concise, structured summaries.
**3. Exact Current Prompt / Instruction Block:**
> "Condense the selected articles into 1-3 sentence summaries. Summaries must explicitly state: 
> 1) What happened
> 2) Why it matters
> 3) Who it impacts
> 
> Maintain a neutral, professional tone. Assign a standardized category tag to each story (e.g., [Model Release], [Policy], [Business], [Safety], [Research], [Tools], [Open Source]). Ensure summaries are strictly bounded to under 75 words."
**4. Inputs It Expects:** Filtered list of 8-12 top articles.
**5. Outputs It Produces:** 1-3 sentence summaries for each article, plus a category tag.
**6. Hidden Rules / Quality Checks:**
- **Length Constraints:** Summaries must be strictly bounded (under 75 words) to maintain the "quick briefing" format.
- **Structural Enforcement:** Must explicitly answer what, why, and who.
**7. Alignment Status:** Aligned with the text briefing format, but the output schema needs to map directly to the Google Sheet columns (Topic / Headline, Briefing Link) for the Intake stage.

---

## STAGE 4: FACT CHECK / VERIFICATION

**1. Component Name:** FactCheck Agent
**2. Purpose:** To verify the credibility and accuracy of the selected stories and their summaries.
**3. Exact Current Prompt / Instruction Block:**
> "Cross-reference key claims (numbers, dates, quotes) against at least one other credible source. Each story must be independently confirmed by at least two distinct sources. Use a low-temperature LLM setting and only use information present in the source text or verified via search. If a claim cannot be verified or appears false, flag the story for removal or correction."
**4. Inputs It Expects:** Summaries and original URLs from the Summarizer Agent.
**5. Outputs It Produces:** Verified summaries, or flags indicating discrepancies.
**6. Hidden Rules / Quality Checks:**
- **2-Source Verification Rule:** Strict enforcement requiring independent confirmation from a second source.
- **Hallucination Guardrails:** Low-temperature LLM setting is enforced.
- **Replacement Logic:** If a story fails verification, it is dropped, and the orchestrator requests a replacement from the RelevanceFilter backlog.
**7. Alignment Status:** Aligned.

---

## STAGE 5: TREND ANALYST

**1. Component Name:** TrendAnalyst Agent
**2. Purpose:** To identify macro patterns and synthesize the "Big Picture" from the day's news.
**3. Exact Current Prompt / Instruction Block:**
> "Look for common themes across multiple stories (e.g., 'Multiple companies focusing on enterprise agents', 'Increasing regulatory pressure'). Generate 3-5 bullet points explaining broader industry trends. Avoid simply repeating the summaries; provide synthesis and forward-looking analysis."
**4. Inputs It Expects:** The final, verified list of 8-12 summaries.
**5. Outputs It Produces:** 3-5 bullet points synthesizing macro patterns.
**6. Hidden Rules / Quality Checks:**
- **Synthesis Threshold:** A trend is only worth including if it connects at least two distinct stories or highlights a major industry shift. It must not be a mere summary repetition.
**7. Alignment Status:** Aligned with Track 1 goals, though this output is currently discarded/unused by the Track 2 video pipeline.

---

## STAGE 6: PRESENTER / FORMATTER

**1. Component Name:** Presenter Agent
**2. Purpose:** To format the final output into a clean, readable Markdown document.
**3. Exact Current Prompt / Instruction Block:**
> "Strictly adhere to the predefined Markdown template.
> - Section 1: Quick Headlines (List of 8-12 items with Title, Source, Date)
> - Section 2: Detailed Summaries (Title, Source, Date, Verified Link, 1-3 sentence summary, Category Tag)
> - Section 3: Big Picture / Trends (3-5 bullet points synthesizing macro patterns)
> 
> Verify that all URLs return a 200 OK status before finalizing the document."
**4. Inputs It Expects:** Verified summaries, category tags, metadata, and trend analysis bullet points.
**5. Outputs It Produces:** A formatted Markdown string.
**6. Hidden Rules / Quality Checks:**
- **Link Validity Check:** Must verify that all URLs return a 200 OK status.
**7. Alignment Status:** **NOT ALIGNED.** There is significant format drift in historical outputs (e.g., using `**Tag:**` vs `**Category Tag:**`, dashes vs colons for headers). Furthermore, generating a static Markdown file conflicts with the new sheet-first operational mandate.

---

## STAGE 7: DELIVERY

**1. Component Name:** Delivery Orchestrator
**2. Purpose:** To save the briefing, push it to GitHub, and notify the user.
**3. Exact Current Prompt / Instruction Block (System Logic):**
> "Save the Markdown string to the `briefings/` directory with the naming convention `briefing-YYYY-MM-DD.md`. Commit and push to the GitHub repository `mrdgtl/Manus-Daily-News` using the classic Personal Access Token (PAT). Send a Telegram notification to the user."
**4. Inputs It Expects:** Final Markdown string.
**5. Outputs It Produces:** Committed file in GitHub, Telegram notification.
**6. Hidden Rules / Quality Checks:**
- **Naming Convention:** Strictly `briefing-YYYY-MM-DD.md`.
**7. Alignment Status:** **NOT ALIGNED.** The Telegram notification structure is outdated. It needs to match the new workflow: *"Briefing complete — [X] stories loaded to Sheet. Awaiting Top 3 approval."*

---

## ALIGNMENT AUDIT

The following components are currently **NOT ALIGNED** with the latest `OPERATIONAL-WORKFLOW.md` and editorial strategy:

### 1. Sheet-First Operation Requirement (CRITICAL)
- **Failing Components:** Presenter (Stage 6) & Delivery (Stage 7)
- **Issue:** The pipeline currently ends by pushing a Markdown file to GitHub. The new operational workflow mandates that **every story must be added to the Google Sheet (Pipeline tab) immediately after briefing generation**. The Intake stage fields (Story ID, Generation Batch ID, Date Added, Topic / Headline, Briefing Link) are not being automatically populated.

### 2. Aggressive Scoring Rules (CRITICAL)
- **Failing Component:** RelevanceFilter (Stage 2)
- **Issue:** The RelevanceFilter currently selects stories based on generic "impact scope" and "novelty." It does **not** execute the required Selection Gate scoring. It must be updated to explicitly score each story for:
  - Viral Potential (1-5)
  - Emotional Impact (1-5)
  - Audience Relevance (1-5)
  - Total Score (Sum)
  - Rank (Top 1, Top 2, Top 3, Backup, Reject)
  - Selection Reason

### 3. Content Type Diversity Rule
- **Failing Component:** RelevanceFilter (Stage 2) / Summarizer (Stage 3)
- **Issue:** While source diversity (max 40% from one source) is enforced, the system does not strictly enforce content type/category diversity (e.g., ensuring a mix of Policy, Business, and Model Release) to prevent a monolithic briefing.

### 4. Format Drift & Template Instability
- **Failing Component:** Presenter (Stage 6)
- **Issue:** Historical briefings show the Presenter agent hallucinating different formatting styles (e.g., `**Tag:** [Business]` on March 26 vs `**Category Tag:** [Policy]` on March 23). This breaks downstream parsing for the Track 2 script extraction. Strict schema adherence must be enforced.
