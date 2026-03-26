# Product Requirements Document (PRD)
**Project Name:** Manus Daily News & AI Daily Source

## 1. The Vision
An autonomous AI news intelligence system that discovers, verifies, synthesizes, and delivers a polished daily AI news briefing, which is then transformed into short-form video content for social media. This is a two-track system:
*   **Track 1: Manus Daily News** — The core AI news briefing system that acts as the editorial engine.
*   **Track 2: AI Daily Source** — The content pipeline that turns those briefings into engaging TikTok and Instagram videos.

## 2. Target Audience
*   **Track 1 (Briefings):** Manu (The User) — wants to stay on top of AI industry news without spending hours reading. Values accuracy and relevancy over volume.
*   **Track 2 (Videos):** Social media users (TikTok/Instagram) — professionals, students, and tech enthusiasts who want digestible, visual AI news updates in under 60 seconds.

## 3. The Problem
The AI industry moves incredibly fast. Keeping up requires reading dozens of sources daily. Most aggregators are noisy, lack editorial judgment, and don't synthesize trends. Manu needs a system that does the heavy lifting — finds what matters, verifies it, and presents it clearly. Furthermore, there is a massive audience on social media hungry for this verified information, but they require it in a short-form video format rather than text.

## 4. System Architecture & Tooling
The project utilizes a specific stack of tools and platforms to manage the two tracks:

### Storage & Organization Architecture
*   **GitHub Repository (`mrdgtl/Manus-Daily-News`):** The single source of truth for project documentation, raw Markdown briefings, and generated scripts.
*   **Google Sheets:** The content pipeline tracking system (one row per script/video).
*   **Google Drive:** Storage and delivery mechanism for final video files.
*   **Telegram:** Live delivery channel for the text briefings.

### Track 1: Briefing Engine (Agentic Workflow)
The system operates using an agentic workflow with 6 specialized agents:
| Agent | Role / Purpose |
| :--- | :--- |
| **NewsCrawler** | Discovers fresh AI news from predefined high-quality sources. |
| **RelevanceFilter** | Filters for high-impact, trending, trustworthy items (aiming for 8-12 stories). |
| **Summarizer** | Generates concise, accurate summaries of the selected stories. |
| **FactCheck** | Cross-checks details across multiple sources to ensure accuracy. |
| **TrendAnalyst** | Detects themes and long-term trends across the daily news cycle. |
| **Presenter** | Assembles and formats the final briefing into the structured output. |

### Track 2: Video Production Pipeline
*   **Voiceover (TTS):** ElevenLabs (Paid subscription, using custom voice "MQ-4-news").
*   **Visual Generation:** Manus native tools (Veo 3.1 for video clips, Nano Banana Pro for images).
*   **Video Assembly:** FFmpeg for programmatic assembly of audio, visuals, and text.
*   **Automation:** Make.com (free tier) planned for future pipeline automation.

## 5. Output Formats

### Track 1 Output: The Briefing
A structured Markdown document containing:
*   **Section 1: Quick Headlines** (8-12 items with Title, Source, Date)
*   **Section 2: Detailed Summaries** (Title, Source, Date, Verified Link, 1-3 sentence summary, Category Tag)
*   **Section 3: Big Picture / Trends** (3-5 bullet points synthesizing macro patterns)

### Track 2 Output: The Videos
*   **Format:** Vertical 9:16 video optimized for TikTok and Instagram Reels.
*   **Duration:** 30-60 seconds (proven end-to-end with a successful 37-second sample).
*   **Volume Target:** 3-5 high-quality videos per briefing day (prioritizing quality over volume).
*   **Channels:** Starting with "AI Daily Source" (TikTok/Instagram). A secondary channel, "Volcaneer," is planned for the future.

## 6. Schedule & Delivery
*   **Schedule:** Every Tuesday, Thursday, and Saturday at 5:00 AM ET. Each briefing covers the period since the last run.
*   **Configurable Schedule:** The active briefing days are user-configurable. Manu can change which days are enabled at any time by simply requesting the change.
*   **Manual Trigger:** Available at any time via the "Start AI news briefing" command.
*   **Video Production:** Videos are produced from the stories generated in each scheduled briefing.

## 7. Validation Framework & Risk
*   **Risk Tier:** Tier 2 (Moderate). The content is public-facing and represents a brand, but compliance risk is low.
*   **Validation:** The team uses a strict validation workflow document for all AI system decisions to ensure quality and safety before public release.

## 8. Phased Roadmap Status

| Phase | Focus | Status | Description |
| :--- | :--- | :--- | :--- |
| **Phase 1** | Automation | Nearly Complete | Scheduled briefings running Tue/Thu/Sat at 5am ET. Manual trigger working. |
| **Phase 2** | Content Pipeline | In Progress | Scripts pipeline working. Video production proven (ElevenLabs + Manus + FFmpeg). Google Drive integration in progress. |
| **Phase 3** | Pipeline Hardening | Not Started | Enforce source diversity, validate links, detect duplicates. |
| **Phase 4** | Intelligence Layer | Not Started | Track cross-day trends, detect story evolution. |
| **Phase 5** | Distribution | Not Started | Expand delivery channels (automated social posting, newsletters). |
| **Phase 6** | Scale & Clone | Not Started | Launch Volcaneer channel and broader community features. |

## 9. Key Project Files
For detailed operational guidelines and historical context, refer to the following files in the repository:
*   `AI-DAILY-SOURCE-PROJECT.md` — Comprehensive content pipeline project document.
*   `TOOLING-RESEARCH.md` — Tooling and architecture research notes.
*   `SHEET-GUIDE.md` — Operational guide for the Google Sheets pipeline.
*   `templates/content-pipeline-template.csv` — The schema template for the Google Sheet.
*   `scripts/` — Directory containing working scripts and TTS-ready scripts.
*   `workflow-design.md` — Detailed architecture of the Track 1 agentic workflow.
