# AI Daily Source: Content Pipeline Sheet Guide

This document explains how to use the Google Sheets content pipeline for the AI Daily Source project. The sheet serves as the single source of truth for every video in the pipeline, from raw briefing to published post.

---

## How to Import the Template

1. Open Google Sheets and create a new spreadsheet.
2. Go to **File > Import** and upload the `content-pipeline-template.csv` file.
3. Name the first tab with the current month (e.g., `2026-03`).
4. Each subsequent month, duplicate the header row into a new tab (e.g., `2026-04`). This keeps the spreadsheet organized and prevents any single tab from becoming unwieldy.

---

## Column Definitions

| Column | Description |
| :--- | :--- |
| **Date** | The date of the briefing the story came from (e.g., `2026-03-26`). |
| **Headline** | The short, descriptive title of the news story. |
| **Source** | The name of the original publication (e.g., Wired, TechCrunch, Reuters). |
| **Source URL** | A direct link to the original article for reference and fact-checking. |
| **Category** | The topic category. Use one of: `Model Release`, `Research`, `Policy`, `Safety`, `Business`. |
| **TTS Script** | The clean, ready-to-read voiceover text. This column contains only the spoken words, with no headings, labels, or brackets. It should be copy-pasteable directly into an ElevenLabs API call. |
| **Visual Direction** | Specific notes on what visuals should accompany each section of the script. This includes suggested images, video clips, text overlays, and transitions. Detailed enough for someone to produce the video from these notes alone. |
| **Voice Status** | Tracks the voiceover generation stage. Values: `Not Started`, `Generated`, `Approved`. |
| **Video Status** | Tracks the video production stage. Values: `Not Started`, `In Production`, `Ready`, `Posted`. |
| **Platform** | The target platform for the video. Values: `TikTok`, `Instagram`, `Both`. |
| **Post Date** | The date the video was or will be published. Left blank until scheduled or posted. |
| **Notes** | Any additional context, feedback, or instructions related to this specific video. |

---

## The Workflow

The content pipeline follows a linear progression from briefing to published video. Each step updates the corresponding status columns in the sheet.

### Step 1: Briefing and Script Population

A daily AI news briefing is generated (stored in `briefings/`). The most compelling stories are selected, and for each one, a TTS-ready script and visual direction notes are written. These populate a new row in the sheet. At this stage, **Voice Status** is set to `Not Started` and **Video Status** is set to `Not Started`.

### Step 2: TTS Generation

The text in the **TTS Script** column is sent to the ElevenLabs API (either manually or via automation). Once the audio file is generated and sounds correct, **Voice Status** is updated to `Generated`. After a final listen and approval, it moves to `Approved`.

### Step 3: Video Production

Using the **Visual Direction** notes, visuals are sourced (stock footage from Pexels, AI-generated images from Nano Banana Pro, or custom clips from Veo 3.1) and assembled with the voiceover audio. **Video Status** moves to `In Production`, then to `Ready` once the final cut is complete.

### Step 4: Posting

The finished video is uploaded to TikTok, Instagram, or both (as indicated in the **Platform** column). The **Post Date** is filled in, and **Video Status** is updated to `Posted`.

---

## Connecting to Make.com Automation

The spreadsheet format is designed for seamless integration with Make.com (or similar automation platforms). The key automation triggers and actions are as follows.

### Trigger: New Row Added

When a new row is added to the sheet (i.e., a new script is ready), Make.com can detect this using the **Google Sheets > Watch New Rows** module. This triggers the downstream pipeline.

### Action 1: TTS Generation

Make.com reads the **TTS Script** column from the new row and sends it to the ElevenLabs API via an HTTP module. The returned audio file URL is stored (e.g., in the **Notes** column or a linked storage system), and **Voice Status** is automatically updated to `Generated`.

### Action 2: Visual Sourcing

In parallel with TTS generation, Make.com can parse the **Visual Direction** column and make API calls to Pexels (for stock footage) or trigger Veo 3.1 / Nano Banana Pro (for AI-generated visuals).

### Action 3: Assembly and Notification

Once audio and visuals are ready, Make.com can trigger a video assembly step (e.g., via a cloud function running FFmpeg, or a service like JSON2Video) and notify you for review. **Video Status** is updated to `In Production`.

### Future: Auto-Posting

Once the pipeline is mature, Make.com can also handle posting to TikTok and Instagram via their respective APIs, completing the fully automated loop.

---

## Monthly Sheet Rotation

To keep the spreadsheet manageable and organized, create a new tab at the start of each month.

1. Right-click the current month's tab and select **Duplicate**.
2. Rename the new tab to the new month (e.g., `2026-04`).
3. Delete all data rows from the new tab, keeping only the header row.
4. All new entries for the month go into the new tab.

This approach keeps historical data accessible (just switch tabs) while ensuring the active workspace stays clean. Make.com scenarios should be updated to point to the new tab at the start of each month, or configured to always read from a tab named with a dynamic month formula.
