# Prompt Inventory: Manus Daily News & AI Daily Source

This document provides a comprehensive audit of every agent, stage, and major workflow component across both the Manus Daily News (Track 1) and AI Daily Source (Track 2) systems. It details the purpose, exact prompt or instruction logic, inputs, outputs, hidden rules, and alignment status for each component based on the current repository documentation.

---

## TRACK 1 — BRIEFING ENGINE

### 1. News Crawler / Research Agent
*   **Purpose:** To discover and retrieve recent AI-related articles from a predefined list of high-quality sources.
*   **Exact current prompt:** NOT DOCUMENTED — implicit instruction. Based on system behavior, it executes parallel searches across 10+ predefined source categories (Company blogs, Tech news sites, Research outlets, Policy sources, Safety/ethics, Business/funding) for articles published within the past 2-3 days (or 24-48 hours), retrieving 20-30 raw stories.
*   **Inputs:** List of target URLs/RSS feeds, time window (e.g., "past 24 hours").
*   **Outputs:** Raw text content, metadata (title, source, date, URL) for all discovered articles (20-30 raw stories).
*   **Hidden rules:** Ignore paywalled content if full text cannot be extracted. Expand search window to 48 hours if fewer than 8 stories are found.
*   **Alignment status:** ALIGNED.

### 2. Relevance Filter
*   **Purpose:** To sift through the raw articles and select the 8-12 most important, high-impact stories.
*   **Exact current prompt:** NOT DOCUMENTED — implicit instruction. Based on system behavior, it evaluates each story against specific criteria: impact scope, novelty, source credibility, and category diversity. Prioritize major model releases, significant policy changes, major business moves, and breakthroughs in research. Discard minor updates, opinion pieces without news value, and duplicate coverage.
*   **Inputs:** Raw articles and metadata from NewsCrawler (20-30 raw stories).
*   **Outputs:** A filtered list of 8-12 top articles.
*   **Hidden rules:** None explicitly beyond the criteria.
*   **Alignment status:** ALIGNED.

### 3. Summarizer
*   **Purpose:** To condense the selected articles into concise, structured summaries.
*   **Exact current prompt:** NOT DOCUMENTED — implicit instruction. Based on system behavior, it condenses full articles into concise 1-3 sentence summaries highlighting what happened, why it matters, and who it impacts. Maintain a neutral, professional tone. Assign a category tag (e.g., `[Model Release]`, `[Policy]`, `[Business]`).
*   **Inputs:** Filtered list of top articles.
*   **Outputs:** 1-3 sentence summaries for each article, plus a category tag.
*   **Hidden rules:** Summaries must explicitly state: 1) What happened, 2) Why it matters, and 3) Who it impacts. Length constraints (e.g., under 75 words).
*   **Alignment status:** ALIGNED.

### 4. Fact Check
*   **Purpose:** To verify the credibility and accuracy of the selected stories and their summaries.
*   **Exact current prompt:** NOT DOCUMENTED — implicit instruction. Based on system behavior, it cross-references key claims (numbers, dates, quotes) against at least one other credible source. Each story must be independently confirmed by at least two distinct sources.
*   **Inputs:** Summaries and original URLs from the Summarizer Agent.
*   **Outputs:** Verified summaries, or flags indicating discrepancies.
*   **Hidden rules:** Uses a low-temperature LLM setting. Only use information present in the source text or verified via search. If a story fails verification, it is dropped.
*   **Alignment status:** ALIGNED.

### 5. Trend Analyst
*   **Purpose:** To identify macro patterns and synthesize the "Big Picture" from the day's news.
*   **Exact current prompt:** NOT DOCUMENTED — implicit instruction. Based on system behavior, it looks for common themes across multiple stories. Avoid simply repeating the summaries; provide synthesis and forward-looking analysis.
*   **Inputs:** The final, verified list of 8-12 summaries.
*   **Outputs:** 3-5 bullet points explaining broader industry trends.
*   **Hidden rules:** None explicitly.
*   **Alignment status:** NEEDS UPDATE. The output in `briefing-2026-03-25.md` shows numbered lists instead of bullet points, indicating formatting drift from the documented rules.

### 6. Presenter / Formatter
*   **Purpose:** To format the final output into a clean, readable Markdown document.
*   **Exact current prompt:** NOT DOCUMENTED — implicit instruction. Based on system behavior, it formats the final output into a Markdown document with three sections: Section 1: Quick Headlines, Section 2: Detailed Summaries, Section 3: Big Picture / Trends.
*   **Inputs:** Verified summaries, category tags, metadata, and trend analysis bullet points.
*   **Outputs:** A formatted `briefing-YYYY-MM-DD.md` file.
*   **Hidden rules:** Source diversity check (no single source accounts for more than 40% of the final stories). Link validity check (verify URLs return 200 OK).
*   **Alignment status:** NEEDS UPDATE. There is formatting drift across briefings (e.g., `briefing-2026-03-23.md` uses `###` headings and explicit labels like `**Source:**`, while `briefing-2026-03-25.md` uses markdown links and different heading styles). The prompt needs to be explicitly defined to enforce a consistent format.

---

## TRACK 2 — AI DAILY SOURCE PIPELINE

### 7. Story Intake
*   **Purpose:** Loading stories to Google Sheet immediately after briefing generation.
*   **Exact current prompt:** NOT DOCUMENTED — implicit instruction. Based on system behavior, it populates the Intake fields: Story ID, Generation Batch ID, Date Added, Topic / Headline, Briefing Link, Pipeline Stage = Intake.
*   **Inputs:** Completed Markdown briefing.
*   **Outputs:** New rows in the Google Sheet (Pipeline tab).
*   **Hidden rules:** Every story must be added to the Google Sheet immediately.
*   **Alignment status:** ALIGNED.

### 8. Selection Engine / Scoring
*   **Purpose:** To score and rank stories based on viral potential, emotional impact, and audience relevance.
*   **Exact current prompt:**
> **Rule 1 — Use the full 1–5 range intentionally.** Do NOT cluster scores at 3–4. Be aggressive. A mediocre story gets a 1 or 2, not a 3. Reserve 4 for stories with genuine above-average performance on that dimension, and 5 for stories that are exceptional and rare.
> **Rule 2 — At least one story must score 5 in Viral Potential or Emotional Impact.** The Top 1 story in any batch must earn a 5 in at least one of these two dimensions. If no story in the pool genuinely deserves a 5, do not inflate the score — instead, flag the batch by adding the following warning to the Notes column for the Top 1 story: *"Low-quality news cycle — weak viral potential."*
> **Rule 3 — Reject any story with no score of 4 or higher.** If a story does not have at least one score of 4+ across Viral Potential, Emotional Impact, and Audience Relevance, it must be assigned Rank = Reject. No exceptions.
> **Rule 4 — Ranking strictly follows Total Score.** Top 1 = highest total, Top 2 = second highest, Top 3 = third highest. In the event of a tie on Total Score, the tie is broken first by Viral Potential (higher wins), then by Emotional Impact (higher wins). If still tied after both tie-breakers, the story with the higher Audience Relevance wins.
> **Rule 5 — Selection Reason must be score-referenced.** Every Selection Reason must explicitly cite the scores (e.g., "Scores 14/15, V:5, E:5, R:4") and must justify why this story beats or loses to competing stories by referencing specific score differences or tie-break outcomes.
> **Rule 6 — Weak pool warning.** If all stories in a batch score below 10 Total Score, add the following warning to the Notes column for every row in the batch: *"Low-quality news cycle — weak viral potential."*
> **Rule 7 — Top 3 must represent three different Content Types.** The final Top 3 selection must include exactly one Viral story, one Shock story, and one Insight story. This ensures every batch delivers a balanced content mix for the audience.
> **Rule 8 — Content Type diversity overrides raw score in Top 3 composition.** When two stories of the same Content Type compete for a Top 3 slot, the lower-scoring story is demoted to Backup regardless of its absolute score, and the highest-scoring story of the missing Content Type is promoted to fill the slot. Raw score tie-breaking (Viral Potential first, then Emotional Impact) applies only within the same Content Type when selecting the representative for that type.
> **Rule 9 — Selection Reason must include Content Type justification.** Every Selection Reason must state the story's Content Type, explain why it is the best representative of that type in the batch, and describe how it complements the other two Top 3 stories to complete the required Viral + Shock + Insight mix.
*(Source: OPERATIONAL-WORKFLOW.md)*
*   **Inputs:** Stories in the Google Sheet (Intake stage).
*   **Outputs:** Scored and ranked stories in the Google Sheet (Selection stage).
*   **Hidden rules:** Wait for human approval before script generation.
*   **Alignment status:** ALIGNED.

### 9. Script Generator (Working)
*   **Purpose:** To generate working scripts with visual direction for selected stories.
*   **Exact current prompt:** NOT DOCUMENTED — implicit instruction. Based on system behavior, it generates a script structured with `[Hook]`, `[Context]`, `[Significance]`, and `[Call to Action]` labels, accompanied by detailed Visual Direction notes for each section. Target constraints: 75-150 words per script. Tone: Conversational, confident, slightly urgent. Hook must appear in the first 3 seconds.
*   **Inputs:** Selected story from the briefing.
*   **Outputs:** `working-scripts-YYYY-MM-DD.md` containing the structured script and visual direction.
*   **Hidden rules:** Script generation ONLY starts when Selection Approved = Approved AND Rank is Top 1, Top 2, or Top 3.
*   **Alignment status:** ALIGNED.

### 10. Script Generator (TTS-Ready)
*   **Purpose:** To generate clean TTS text optimized for speech synthesis.
*   **Exact current prompt:** NOT DOCUMENTED — implicit instruction. Based on system behavior, it generates clean, spoken text only. No labels, headings, or brackets. Abbreviations are spelled out phonetically for natural speech synthesis (e.g., "artificial intelligence" instead of "AI" if mispronounced).
*   **Inputs:** Working script.
*   **Outputs:** `tts-ready-YYYY-MM-DD.md` containing the clean spoken text.
*   **Hidden rules:** Target constraints: 75-150 words per script, equating to 30-60 seconds of spoken audio.
*   **Alignment status:** ALIGNED.

### 11. Script Quality Evaluator
*   **Purpose:** To self-evaluate the generated scripts based on specific criteria.
*   **Exact current prompt:** NOT DOCUMENTED — implicit instruction. Based on system behavior, it evaluates the script on Hook Strength (1-5), Clarity (1-5), Flow (1-5), and TTS Pass (Yes/No).
*   **Inputs:** Generated script.
*   **Outputs:** Scores populated in the Google Sheet.
*   **Hidden rules:** Quality threshold: If Hook Strength < 4 OR Clarity < 4 OR Flow < 4 OR TTS Pass = No, automatically regenerate the script (up to 3 attempts).
*   **Alignment status:** ALIGNED.

### 12. Voiceover Generator
*   **Purpose:** To generate voiceover audio using the ElevenLabs API.
*   **Exact current prompt:** NOT DOCUMENTED — implicit instruction. Based on system behavior, it makes an API call to ElevenLabs with Voice: "MQ-4-news" (Custom voice ID: `h5auDwZge17EdlHgtS16`), Model: `eleven_multilingual_v2` (or Flash v2.5).
*   **Inputs:** TTS-ready script text.
*   **Outputs:** MP3 audio file for each script.
*   **Hidden rules:** Only proceed if Script Approved = Yes.
*   **Alignment status:** ALIGNED.

### 13. Visual Asset Generator
*   **Purpose:** To generate keyframe images and animated video clips based on visual direction notes.
*   **Exact current prompt:** NOT DOCUMENTED — implicit instruction. Based on system behavior, it performs a two-step process for each script section (Hook, Context, Significance, CTA): Step 1: Generate a keyframe image using Nano Banana Pro based on the detailed visual direction prompt (aspect ratio 9:16). Step 2: Animate the generated keyframe into a short video clip (~8 seconds) using Veo 3.1.
*   **Inputs:** Visual direction notes from the working script.
*   **Outputs:** 4 keyframe images (PNG) and 4 animated video clips (MP4) per script.
*   **Hidden rules:** None explicitly.
*   **Alignment status:** ALIGNED.

### 14. Video Assembler
*   **Purpose:** To assemble the final video using FFmpeg.
*   **Exact current prompt:** NOT DOCUMENTED — implicit instruction. Based on system behavior, it concatenates the 4 video clips into a single continuous video stream. If the total duration does not match the voiceover length, it applies time-stretching using the `setpts` filter to synchronize with the audio track. The voiceover MP3 is overlaid as the primary audio track.
*   **Inputs:** 4 animated video clips (MP4) and 1 voiceover audio file (MP3) per script.
*   **Outputs:** Final MP4 video file (H.264, AAC, 720x1280 or 1080x1920, 9:16 aspect ratio).
*   **Hidden rules:** None explicitly.
*   **Alignment status:** ALIGNED.

### 15. Production Quality Evaluator
*   **Purpose:** To self-evaluate the assembled video based on specific criteria.
*   **Exact current prompt:** NOT DOCUMENTED — implicit instruction. Based on system behavior, it evaluates the video on Video Quality Score (1-5), Sync Pass (Yes/No), and Visual Consistency (1-5).
*   **Inputs:** Assembled final video.
*   **Outputs:** Scores populated in the Google Sheet.
*   **Hidden rules:** Quality threshold: If Video Quality Score < 4 OR Sync Pass = No OR Visual Consistency < 4, identify the failing component, fix upstream, and retry (up to 2 attempts).
*   **Alignment status:** ALIGNED.

### 16. Google Drive Uploader
*   **Purpose:** To upload generated assets to Google Drive with strict naming conventions and folder routing.
*   **Exact current prompt:** NOT DOCUMENTED — implicit instruction. Based on system behavior, it uploads assets to specific folders: Final Videos to `Final Videos/YYYY/MM-Month/Week-XX/`, Scripts to `Scripts/YYYY/MM-Month/`, Voiceovers to `Voiceovers/YYYY/MM-Month/`, Visual Assets to `Visual Assets/YYYY/MM-Month/`, Briefings to `Briefings/YYYY/MM-Month/`. File naming convention: `YYYY-MM-DD_TOPIC_SHORTTITLE[_SUFFIX][_VERSION].ext`.
*   **Inputs:** Generated assets (scripts, voiceovers, visuals, final videos).
*   **Outputs:** Files uploaded to Google Drive.
*   **Hidden rules:** Auto-creation logic for folders based on date and ISO week.
*   **Alignment status:** ALIGNED.

### 17. Google Sheet Updater
*   **Purpose:** To update the Google Sheet fields at each stage of the pipeline.
*   **Exact current prompt:** NOT DOCUMENTED — implicit instruction. Based on system behavior, it updates specific fields at each stage (Intake, Selection, Script, Production, Ready, Posted, Archived) as defined in the SHEET SCHEMA ALIGNMENT section of OPERATIONAL-WORKFLOW.md.
*   **Inputs:** Pipeline progress and status changes.
*   **Outputs:** Updated fields in the Google Sheet.
*   **Hidden rules:** Always keep Pipeline Stage and Action Required accurate. Do NOT fail silently — always notify the user on Telegram with failure details.
*   **Alignment status:** ALIGNED.

---

## Summary Table

| Component | Documented? | File Location | Aligned? | Action Needed |
| :--- | :--- | :--- | :--- | :--- |
| 1. News Crawler / Research Agent | Implicit | PIPELINE-TECHNICAL-SPEC.md, workflow-design.md | ALIGNED | Document exact prompt |
| 2. Relevance Filter | Implicit | PIPELINE-TECHNICAL-SPEC.md, workflow-design.md | ALIGNED | Document exact prompt |
| 3. Summarizer | Implicit | workflow-design.md | ALIGNED | Document exact prompt |
| 4. Fact Check | Implicit | PIPELINE-TECHNICAL-SPEC.md, workflow-design.md | ALIGNED | Document exact prompt |
| 5. Trend Analyst | Implicit | workflow-design.md | NEEDS UPDATE | Fix formatting drift (bullets vs numbered lists) and document exact prompt |
| 6. Presenter / Formatter | Implicit | workflow-design.md | NEEDS UPDATE | Fix formatting drift across briefings and document exact prompt |
| 7. Story Intake | Implicit | OPERATIONAL-WORKFLOW.md | ALIGNED | Document exact prompt |
| 8. Selection Engine / Scoring | Explicit | OPERATIONAL-WORKFLOW.md | ALIGNED | None |
| 9. Script Generator (Working) | Implicit | PIPELINE-TECHNICAL-SPEC.md, AI-DAILY-SOURCE-PROJECT.md | ALIGNED | Document exact prompt |
| 10. Script Generator (TTS-Ready) | Implicit | PIPELINE-TECHNICAL-SPEC.md, AI-DAILY-SOURCE-PROJECT.md | ALIGNED | Document exact prompt |
| 11. Script Quality Evaluator | Implicit | OPERATIONAL-WORKFLOW.md | ALIGNED | Document exact prompt |
| 12. Voiceover Generator | Implicit | PIPELINE-TECHNICAL-SPEC.md | ALIGNED | Document exact prompt |
| 13. Visual Asset Generator | Implicit | PIPELINE-TECHNICAL-SPEC.md | ALIGNED | Document exact prompt |
| 14. Video Assembler | Implicit | PIPELINE-TECHNICAL-SPEC.md | ALIGNED | Document exact prompt |
| 15. Production Quality Evaluator | Implicit | OPERATIONAL-WORKFLOW.md | ALIGNED | Document exact prompt |
| 16. Google Drive Uploader | Implicit | STORAGE-STRUCTURE.md | ALIGNED | Document exact prompt |
| 17. Google Sheet Updater | Implicit | OPERATIONAL-WORKFLOW.md | ALIGNED | Document exact prompt |
