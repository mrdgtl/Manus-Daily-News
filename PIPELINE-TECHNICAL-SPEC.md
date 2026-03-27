# AI Daily Source Project - Technical Pipeline Specification

This document outlines the comprehensive, end-to-end technical pipeline for the AI Daily Source project. It details the sequential stages from initial news collection to the final video output, specifying inputs, processes, outputs, tools, storage, triggers, and potential failure points for each stage.

## Stage 1: News Collection

**Input:** 
- None (Triggered event)

**Process:**
- Manus's native web research capability executes parallel searches across 10+ predefined source categories.
- Categories include:
  - Company blogs (OpenAI, Google DeepMind, Anthropic, Meta AI, Microsoft, Apple)
  - Tech news sites (TechCrunch, The Verge, Wired, Ars Technica, VentureBeat)
  - Research outlets (arXiv, Google Research Blog)
  - Policy sources (White House, EU AI Act updates, Congressional records)
  - Safety/ethics (AI safety organizations, EFF)
  - Business/funding (Crunchbase, Bloomberg)
- The system collects 20-30 raw stories published within the past 2-3 days.

**Output:** 
- Raw story list containing titles, sources, URLs, and publication dates.

**Tools/Models Used:** 
- Manus Web Research Capability

**Storage:** 
- In memory (passed directly to Stage 2)

**Trigger for Next Stage:** 
- Completion of the parallel search and aggregation of the raw story list.
- Initial Trigger: Scheduled cron job fires at 5:00 AM ET on Tuesday, Thursday, and Saturday, or via manual trigger ("Start AI news briefing").

**Failure Points:**
- Sources may be down, paywalled, or have changed their HTML structure.
- The search algorithm may miss relevant stories from less prominent or newly established sources.

---

## Stage 2: Filtering & Verification

**Input:** 
- Raw story list from Stage 1.

**Process:**
- Manus LLM reasoning evaluates each story against specific criteria: impact scope, novelty, source credibility, and category diversity.
- **Cross-verification:** Each story must be independently confirmed by at least two distinct sources.
- **Source diversity check:** The system ensures no single media outlet represents more than 40% of the selected stories.
- The system curates and selects the top 8-12 stories for the final briefing.

**Output:** 
- Curated story list with verified facts and multiple source references.

**Tools/Models Used:** 
- Manus LLM

**Storage:** 
- In memory (passed directly to Stage 3)

**Trigger for Next Stage:** 
- Completion of the filtering, verification, and selection process.

**Failure Points:**
- The LLM may misjudge the relevance or impact of a story.
- Cross-verification may fail for breaking news or exclusive stories with limited coverage.
- Potential bias toward heavily covered mainstream stories over niche but highly important developments.

---

## Stage 3: Briefing Generation

**Input:** 
- Curated story list from Stage 2.

**Process:**
- Manus LLM generates the full briefing document in Markdown format.
- The document is structured into three main sections:
  1. **Quick Headlines:** One-line summaries of the selected stories.
  2. **Detailed Summaries:** Each story includes the source, link, category, and a 2-3 paragraph comprehensive summary.
  3. **Trend Analysis:** Cross-story patterns, broader implications, and industry movements.

**Output:** 
- Markdown briefing file (e.g., `briefing-YYYY-MM-DD.md`).

**Tools/Models Used:** 
- Manus LLM (Writing)
- GitHub API via `gh` CLI with classic Personal Access Token (PAT)

**Storage:** 
- Pushed to the GitHub repository `mrdgtl/Manus-Daily-News` under the `briefings/` directory.
- Delivered to the user via Telegram.

**Trigger for Next Stage:** 
- Successful generation of the Markdown file.

**Failure Points:**
- The LLM may hallucinate details or misinterpret technical nuances.
- The trend analysis may be superficial or state the obvious.
- The GitHub push may fail if the authentication token expires or encounters network issues.

---

## Stage 4: Script Selection & Generation

**Input:** 
- Completed Markdown briefing from Stage 3.

**Process:**
- Manus LLM selects the 3-5 most compelling stories optimized for short-form video content based on: strong emotional hooks, surprising statistics, broad audience appeal, visual potential, and controversy/conflict.
- For each selected story, the LLM generates two distinct script versions:
  1. **Working Script:** Structured with `[Hook]`, `[Context]`, `[Significance]`, and `[Call to Action]` labels, accompanied by detailed Visual Direction notes for each section.
  2. **TTS-Ready Script:** Clean, spoken text only. No labels, headings, or brackets. Abbreviations are spelled out phonetically for natural speech synthesis.
- **Target Constraints:** 75-150 words per script, equating to 30-60 seconds of spoken audio.

**Output:** 
- `working-scripts-YYYY-MM-DD.md`
- `tts-ready-YYYY-MM-DD.md`

**Tools/Models Used:** 
- Manus LLM

**Storage:** 
- Pushed to the GitHub repository under the `scripts/` directory.
- Populated into the content pipeline CSV/Google Sheet.

**Trigger for Next Stage:** 
- Successful generation and storage of the TTS-ready scripts.

**Failure Points:**
- The script tone may not align with the intended fast-paced TikTok/Reels style.
- Visual direction notes may be too vague for consistent image/video generation.
- Word count may drift outside the target range, affecting final video duration.

---

## Stage 5: Voiceover Generation

**Input:** 
- TTS-ready script text for each selected story.

**Process:**
- The system makes an API call to the ElevenLabs text-to-speech endpoint.
- **Voice:** "MQ-4-news" (Custom voice ID: `h5auDwZge17EdlHgtS16`).
- **Model:** `eleven_multilingual_v2` (or Flash v2.5 for cost efficiency).
- **Endpoint:** `POST https://api.elevenlabs.io/v1/text-to-speech/{voice_id}`
- **Headers:** `xi-api-key` containing the API key.

**Output:** 
- MP3 audio file for each script (approximately 30-45 seconds each).

**Tools/Models Used:** 
- ElevenLabs API

**Storage:** 
- Temporarily stored in the sandbox environment (passed to Stage 7).
- Final version backed up to Google Drive.

**Trigger for Next Stage:** 
- Successful download of the generated MP3 files.

**Failure Points:**
- API rate limits or network timeouts.
- Character quota exhaustion on the ElevenLabs subscription plan.
- Inconsistent voice quality or unnatural pacing.
- Pronunciation issues with complex technical terms or acronyms.

---

## Stage 6: Visual Generation

**Input:** 
- Visual direction notes extracted from the working scripts.

**Process:**
- For each script section (Hook, Context, Significance, CTA), the system performs a two-step generation process:
  - **Step 1:** Generate a keyframe image using Nano Banana Pro based on the detailed visual direction prompt. The aspect ratio is strictly set to 9:16 (portrait).
  - **Step 2:** Animate the generated keyframe into a short video clip (~8 seconds) using Veo 3.1. The prompt describes the intended motion, camera effects, and energy, using the keyframe as the reference/first frame.

**Output:** 
- 4 keyframe images (PNG) per script.
- 4 animated video clips (MP4) per script.

**Tools/Models Used:** 
- Nano Banana Pro (Image Generation)
- Veo 3.1 (Video Animation)

**Storage:** 
- Temporarily stored in the sandbox environment (passed to Stage 7).

**Trigger for Next Stage:** 
- Successful generation of all 4 MP4 clips for a given script.

**Failure Points:**
- Image quality and adherence to the prompt may vary.
- The animation may exhibit artifacts or fail to match the intended motion dynamics.
- Text baked into the generated visuals may be illegible or nonsensical.
- Style inconsistency across the 4 clips for a single story.
- Generation latency (can take several minutes per clip).

---

## Stage 7: Video Assembly

**Input:** 
- 4 animated video clips (MP4) and 1 voiceover audio file (MP3) per script.

**Process:**
- FFmpeg is utilized to concatenate the 4 video clips into a single continuous video stream.
- **Synchronization:** If the total duration of the concatenated video clips does not match the voiceover length, FFmpeg applies time-stretching to the video using the `setpts` filter to synchronize with the audio track.
- The voiceover MP3 is overlaid as the primary audio track.
- The final composition is encoded.

**Output:** 
- Final MP4 video file.
- **Specifications:** H.264 video codec, AAC audio codec, 720x1280 or 1080x1920 resolution, 9:16 vertical aspect ratio, 30-60 seconds total duration.

**Tools/Models Used:** 
- FFmpeg

**Storage:** 
- Delivered to the user via Telegram.
- Planned: Uploaded to Google Drive (organized in monthly folders).

**Trigger for Next Stage:** 
- Successful encoding of the final MP4 file.

**Failure Points:**
- Audio/video synchronization issues.
- Time-stretching may look unnatural or jittery if the duration mismatch is significant.
- Resolution or quality degradation during the final encoding pass.
- The resulting file size may exceed platform limits for automated posting.

---

## Stage 8: Delivery & Tracking

**Input:** 
- Final assembled MP4 video files.

**Process:**
- The final videos are delivered directly to the user via Telegram for review and manual posting.
- The pipeline status is updated in the tracking Google Sheet (updating the "Voice Status" and "Video Status" columns).
- **Planned Automations:**
  - Automated upload to Google Drive monthly folders (pending service account setup).
  - Make.com automation workflow for auto-posting the final videos to TikTok and Instagram Reels.

**Output:** 
- Videos accessible to the user.
- Updated tracking documentation.

**Tools/Models Used:** 
- Telegram API (Current)
- Google Sheets API
- Google Drive API (Planned)
- Make.com (Planned)

**Storage:** 
- Telegram chat history.
- Google Sheets.

**Trigger for Next Stage:** 
- End of pipeline.

**Failure Points:**
- Manual posting remains a significant bottleneck.
- Tracking sheets may fall out of sync if the API update fails.

---

## End-to-End Flow Diagram

```text
[5am ET Cron Trigger] or [Manual "Start AI news briefing"]
        │
        ▼
┌─────────────────────┐
│ STAGE 1: Collection │ ── Manus web research (10+ source categories, parallel)
│ 20-30 raw stories   │
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│ STAGE 2: Filter &   │ ── Manus LLM reasoning + cross-verification
│ Verify (8-12 items) │
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│ STAGE 3: Briefing   │ ── Manus LLM writes Markdown briefing
│ Generation          │ ── Push to GitHub + deliver via Telegram
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│ STAGE 4: Script     │ ── Manus LLM selects top 3-5, writes working + TTS scripts
│ Generation          │ ── Push to GitHub + populate Google Sheet
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│ STAGE 5: Voiceover  │ ── ElevenLabs API (MQ-4-news voice)
│ Generation          │ ── MP3 per script
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│ STAGE 6: Visual     │ ── Nano Banana Pro (keyframes) + Veo 3.1 (animation)
│ Generation          │ ── 4 clips per script
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│ STAGE 7: Assembly   │ ── FFmpeg (concatenate + sync audio + encode)
│                     │ ── Final MP4 (9:16 vertical)
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│ STAGE 8: Delivery   │ ── Telegram + Google Drive (planned) + Sheet status update
│ & Tracking          │
└─────────────────────┘
```

---

## Current Limitations & Known Gaps

- **Manual Intervention Required:** Video generation currently requires manual confirmation for each clip; the process is not yet fully automated.
- **Storage Integration Pending:** Google Drive integration is planned but currently pending the completion of service account setup.
- **No Automated Publishing:** There is currently no automated posting mechanism to platforms like TikTok or Instagram (Make.com integration planned).
- **Text Overlay Limitations:** Text overlays are currently baked directly into the visual generation prompts rather than being added as a clean, separate compositing step during assembly.
- **Lack of Feedback Loop:** The system does not currently ingest performance metrics (views, engagement) to learn which video styles or topics perform best.
- **Throughput Constraints:** Each video requires approximately 10 minutes of generation time, which limits the overall throughput of the pipeline.
