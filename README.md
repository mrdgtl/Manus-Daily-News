# AI-News-Agentic-Flow

An automated system that discovers, aggregates, and presents the top AI news daily, powered by an agentic workflow.

## What it is
This project is an automated news briefing system designed to cut through the noise of the fast-paced artificial intelligence industry. It discovers the most important AI news, aggregates information from multiple credible sources, and presents a clean, structured daily briefing.

## How it works
The system utilizes an agentic workflow, where specialized AI agents handle different parts of the pipeline:

*   **NewsCrawler:** Scours predefined high-quality sources (official blogs, TechCrunch, MIT Tech Review, etc.) for recent articles.
*   **RelevanceFilter:** Evaluates crawled articles to ensure they are high-impact, trending, and relevant to the AI industry.
*   **Summarizer:** Condenses full articles into concise 1-3 sentence summaries highlighting what happened, why it matters, and who it impacts.
*   **FactCheck:** Cross-references claims across multiple sources to verify credibility and accuracy.
*   **TrendAnalyst:** Synthesizes patterns across the day's top stories to identify macro trends and big-picture implications.
*   **Presenter:** Formats the final output into a clean, professional Markdown document.

## Schedule
The workflow is scheduled to run **daily at 5:00 AM ET**, ensuring a fresh briefing is ready for the start of the workday.

## Output
The system produces a structured Markdown briefing document containing:
1.  **Quick Headlines:** A scannable list of the top 8-12 stories.
2.  **Detailed Summaries:** Categorized, concise summaries with source links.
3.  **Big Picture / Trends:** Synthesized bullet points explaining broader industry patterns.

All briefings are saved in the `briefings/` directory.
