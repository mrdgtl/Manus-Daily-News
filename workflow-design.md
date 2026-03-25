# AI News Agentic Workflow Design

This document outlines the architecture and orchestration logic for the AI-News-Agentic-Flow system.

## 1. Agent Definitions

The system is composed of six specialized agents, each with a distinct role in the pipeline.

### NewsCrawler Agent
*   **Purpose:** To discover and retrieve recent AI-related articles from a predefined list of high-quality sources.
*   **Inputs:** List of target URLs/RSS feeds (e.g., OpenAI blog, TechCrunch AI, MIT Tech Review), time window (e.g., "past 24 hours").
*   **Outputs:** Raw text content, metadata (title, source, date, URL) for all discovered articles.
*   **Decision Rules:** Only retrieve articles published within the specified time window. Ignore paywalled content if full text cannot be extracted.

### RelevanceFilter Agent
*   **Purpose:** To sift through the raw articles and select the 8-12 most important, high-impact stories.
*   **Inputs:** Raw articles and metadata from NewsCrawler.
*   **Outputs:** A filtered list of 8-12 top articles.
*   **Decision Rules:** Prioritize major model releases, significant policy changes, major business moves (M&A, large funding, layoffs), and breakthroughs in research. Discard minor updates, opinion pieces without news value, and duplicate coverage of the same event.

### Summarizer Agent
*   **Purpose:** To condense the selected articles into concise, structured summaries.
*   **Inputs:** Filtered list of top articles.
*   **Outputs:** 1-3 sentence summaries for each article, plus a category tag (e.g., [Model Release], [Policy], [Business]).
*   **Decision Rules:** Summaries must explicitly state: 1) What happened, 2) Why it matters, and 3) Who it impacts. Maintain a neutral, professional tone.

### FactCheck Agent
*   **Purpose:** To verify the credibility and accuracy of the selected stories and their summaries.
*   **Inputs:** Summaries and original URLs from the Summarizer Agent.
*   **Outputs:** Verified summaries, or flags indicating discrepancies.
*   **Decision Rules:** Cross-reference key claims (numbers, dates, quotes) against at least one other credible source. If a claim cannot be verified or appears false, flag the story for removal or correction.

### TrendAnalyst Agent
*   **Purpose:** To identify macro patterns and synthesize the "Big Picture" from the day's news.
*   **Inputs:** The final, verified list of 8-12 summaries.
*   **Outputs:** 3-5 bullet points explaining broader industry trends.
*   **Decision Rules:** Look for common themes across multiple stories (e.g., "Multiple companies focusing on enterprise agents," "Increasing regulatory pressure"). Avoid simply repeating the summaries; provide synthesis and forward-looking analysis.

### Presenter Agent
*   **Purpose:** To format the final output into a clean, readable Markdown document.
*   **Inputs:** Verified summaries, category tags, metadata, and trend analysis bullet points.
*   **Outputs:** A formatted `briefing-YYYY-MM-DD.md` file.
*   **Decision Rules:** Strictly adhere to the predefined Markdown template (Section 1: Quick Headlines, Section 2: Detailed Summaries, Section 3: Big Picture / Trends). Ensure all links are properly formatted.

---

## 2. Step-by-Step Pipeline / Orchestration Logic

The workflow executes sequentially, triggered daily at 5:00 AM ET via a cron job or orchestration tool (e.g., GitHub Actions, Prefect, or a dedicated agent framework).

1.  **Trigger:** Scheduler initiates the run.
2.  **Crawl Phase:** Orchestrator calls `NewsCrawler` with the target source list and a 24-hour lookback window.
3.  **Filter Phase:** Orchestrator passes the raw crawl results to `RelevanceFilter`. If fewer than 8 stories are found, the orchestrator may instruct the crawler to expand the search window to 48 hours.
4.  **Summarize Phase:** The filtered list is passed to `Summarizer`. This can be done in parallel (map-reduce) for each article to save time.
5.  **Verification Phase:** Summaries are passed to `FactCheck`. If a story fails verification, it is dropped, and the orchestrator may request a replacement from the `RelevanceFilter` backlog.
6.  **Analysis Phase:** The verified summaries are passed to `TrendAnalyst` to generate the big picture insights.
7.  **Formatting Phase:** All components are passed to `Presenter` to generate the final Markdown string.
8.  **Delivery Phase:** The orchestrator saves the Markdown string to the `briefings/` directory and commits/pushes it to the GitHub repository.

---

## 3. Quality Checks

To ensure the briefing remains high-quality and reliable, the following automated checks are implemented between agent handoffs:

*   **Source Diversity Check:** Ensure no single source (e.g., TechCrunch) accounts for more than 40% of the final stories.
*   **Link Validity Check:** The Presenter agent must verify that all URLs return a 200 OK status before finalizing the document.
*   **Length Constraints:** Summaries must be strictly bounded (e.g., under 75 words) to maintain the "quick briefing" format.
*   **Hallucination Guardrails:** The FactCheck agent uses a low-temperature LLM setting and is instructed to only use information present in the source text or verified via search.

---

## 4. Improvement and Maintenance Plan

*   **Source List Updates:** The list of target URLs for the NewsCrawler will be reviewed monthly to add new high-quality sources and remove degraded ones.
*   **Prompt Refinement:** The prompts for the Summarizer and TrendAnalyst agents will be version-controlled. We will review the output weekly and tweak the prompts to adjust tone, length, or focus as needed.
*   **Error Logging:** Any failures in the pipeline (e.g., crawler blocked, LLM timeout) will be logged to a central dashboard for immediate debugging.
*   **Future Expansion:** Plan to add a "Delivery Agent" to distribute the final Markdown via email newsletter (e.g., Substack/Mailchimp API) or Slack/Discord webhooks.
