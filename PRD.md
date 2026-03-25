# Product Requirements Document (PRD)
**Project Name:** Manus Daily News — AI News Agentic Flow

## 1. The Vision
An autonomous AI news intelligence system that discovers, verifies, synthesizes, and delivers a polished daily AI news briefing. This is not a simple aggregator — it is a system with editorial judgment that gets smarter over time.

## 2. Target Audience
**Manu (The User):** Someone who wants to stay on top of AI industry news without spending hours reading. Values accuracy and relevancy over volume.

## 3. The Problem
The AI industry moves incredibly fast. Keeping up requires reading dozens of sources daily. Most aggregators are noisy, lack editorial judgment, and don't synthesize trends. Manu needs a system that does the heavy lifting — finds what matters, verifies it, and presents it clearly.

## 4. How It Works (Agentic Workflow)
The system operates using an agentic workflow with 6 specialized agents:

| Agent | Role / Purpose |
| :--- | :--- |
| **NewsCrawler** | Discovers fresh AI news from predefined high-quality sources. |
| **RelevanceFilter** | Filters for high-impact, trending, trustworthy items (aiming for 8-12 stories). |
| **Summarizer** | Generates concise, accurate summaries of the selected stories. |
| **FactCheck** | Cross-checks details across multiple sources to ensure accuracy. |
| **TrendAnalyst** | Detects themes and long-term trends across the daily news cycle. |
| **Presenter** | Assembles and formats the final briefing into the structured output. |

## 5. Sources
The system will monitor the following sources at a minimum:
*   **Core AI Providers:** OpenAI, Google/DeepMind, Anthropic, Meta, xAI, Mistral, Cohere
*   **News Outlets:** TechCrunch AI, MIT Technology Review AI, VentureBeat AI, Ars Technica AI, The Verge AI
*   **Optional/Secondary:** Wired, trusted AI newsletters, and reputable industry blogs

## 6. Output Format
The final output will be a structured Markdown briefing containing three main sections:

### Section 1: Quick Headlines
A list of 8-12 items, each including:
*   Title
*   Source
*   Date

### Section 2: Detailed Summaries
For each story included in the headlines:
*   **Title**
*   **Source + Date**
*   **Link:** Real, verified URL
*   **Summary:** 1-3 sentences explaining what happened, why it matters, and who it impacts.
*   **Category Tag:** One of `[Model Release]`, `[Research]`, `[Policy]`, `[Safety]`, `[Business]`

### Section 3: Big Picture / Trends
*   3-5 bullet points synthesizing patterns across the day's stories and highlighting what to watch for.

## 7. Prioritization Criteria
Stories are selected and ranked based on the following criteria:
1.  Major product releases or model launches
2.  Significant research breakthroughs
3.  Policy/regulation changes
4.  Notable safety/alignment/governance developments
5.  Large funding, partnerships, or strategic moves

## 8. Schedule & Delivery
*   **Schedule:** Every Tuesday, Thursday, and Saturday at 5:00 AM ET. Each briefing covers the period since the last run (approximately 2-3 days).
*   **Configurable Schedule:** The active briefing days are user-configurable. Manu can change which days are enabled at any time by simply requesting the change. The system must support enabling or disabling any combination of days without requiring technical changes.
*   **Manual Trigger:** Available at any time via the "Start AI news briefing" command, independent of the scheduled days.
*   **Delivery Methods:**
    *   Primary: Telegram
    *   Secondary: Files committed to the GitHub repository (`mrdgtl/Manus-Daily-News`) under the `briefings/` folder.

## 9. Technical Constraints
*   **Cost:** Keep costs as low/free as possible.
*   **Capabilities:** Prefer native Manus capabilities over external APIs. Use `gpt-4.1-mini` natively for summarization and analysis.
*   **Operations:** Use the provided Classic GitHub PAT for repository operations.
*   **Approach:** Build a proof of concept first, prioritizing accuracy and relevancy over volume.

## 10. Tone & Style
*   **Tone:** Professional, clear, neutral — like a tech-savvy news anchor.
*   **Style:** Use headings, bullet points, and short paragraphs. Avoid excessive jargon and explain technical terms briefly when necessary.

## 11. Phased Roadmap

| Phase | Focus | Description |
| :--- | :--- | :--- |
| **Phase 1** | Automation (Current/Next) | Set up daily 5am ET scheduled trigger. Establish manual trigger. Deliver briefings to Telegram and commit to GitHub. |
| **Phase 2** | Refinement | Run daily for 1-2 weeks. Collect user feedback. Tune editorial judgment, sources, and filtering criteria. |
| **Phase 3** | Pipeline Hardening | Enforce source diversity, validate links, and detect duplicates across days. Build resilience to adapt when sources fail. |
| **Phase 4** | Intelligence Layer | Track cross-day trends. Detect story evolution ("this story is developing"). Develop system memory across briefings. |
| **Phase 5** | Distribution | Expand beyond Telegram — add weekly digest emails, Slack integration, and a simple web dashboard. |
| **Phase 6** | Community/Sharing | Launch a newsletter, public page, and reach a broader audience if desired. |

## 12. Success Criteria
*   **Accurate:** No hallucinated stories or broken links.
*   **Relevant:** Stories are genuinely top-10 caliber, not filler.
*   **Timely:** Delivered reliably by 5:00 AM ET daily.
*   **Concise:** Can be read in under 5 minutes.
*   **Reliable:** System runs without manual intervention.
*   **Cost-Effective:** Minimal cost, utilizing native capabilities primarily.

## 13. Non-Functional Requirements
*   **Honesty:** Be transparent about limits — if real-time data is unavailable, state it clearly.
*   **Credibility:** Cross-check facts from multiple outlets. Discard low-credibility or purely speculative items.
*   **Diversity:** Ensure source diversity so no single outlet dominates the briefing.
