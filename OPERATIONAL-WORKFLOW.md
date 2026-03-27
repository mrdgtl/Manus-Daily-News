# AI Daily Source Pipeline: Operational Workflow

This document defines the complete workflow rules for the AI Daily Source pipeline. The Google Sheet dashboard serves as the source of truth and main control system for this operational workflow.

## Prompt File Authority

The following workflow stages are governed by versioned prompt files stored in /prompts/track2-production/. These files are the canonical source of instructions. If any section of this document conflicts with the prompt files, the prompt files take precedence.

| Stage | Prompt File | Version |
|---|---|---|
| Selection Gate | /prompts/track2-production/selection-engine.md | v1.0 |
| Script Generation | /prompts/track2-production/script-generator.md | v1.0 |
| Script Quality Evaluation | /prompts/track2-production/script-evaluator.md | v1.0 |

## 1. SHEET-FIRST OPERATION

Every story must be added to the Google Sheet (ID: `1Nc_O0Ina5gnz2X7hV0E1ia_NN1eArW3bTP1pcF2bbO4`, Pipeline tab) immediately after briefing generation. All intake and selection fields must be populated before moving forward. The Google Sheet is now the main operational control system. Telegram is only for alerts and delivery notifications (e.g., "Briefing complete — 8 stories loaded to Sheet. Awaiting Top 3 approval.").

## 2. SELECTION GATE

Each story must be scored for Viral Potential (1-5), Emotional Impact (1-5), and Audience Relevance (1-5). The Total Score is calculated as the sum of these three metrics. Based on the scores, assign a Rank: Top 1, Top 2, Top 3, Backup, or Reject. A Selection Reason (free text explaining why this story was ranked this way) must be added.

Set the following fields:
- Selection Approved = Pending
- Pipeline Stage = Selected
- Action Required = Approve Top 3

**STOP** — Wait for human approval before script generation. Notify the user on Telegram: "Selection complete. Top 3 stories scored and ranked in the Sheet. Please review and approve."

## 2A. SCORING RULES

The following rules govern how stories are scored during the Selection stage. These rules are mandatory and must be applied consistently on every batch.

**Rule 1 — Use the full 1–5 range intentionally.** Do NOT cluster scores at 3–4. Be aggressive. A mediocre story gets a 1 or 2, not a 3. Reserve 4 for stories with genuine above-average performance on that dimension, and 5 for stories that are exceptional and rare.

**Rule 2 — At least one story must score 5 in Viral Potential or Emotional Impact.** The Top 1 story in any batch must earn a 5 in at least one of these two dimensions. If no story in the pool genuinely deserves a 5, do not inflate the score — instead, flag the batch by adding the following warning to the Notes column for the Top 1 story: *"Low-quality news cycle — weak viral potential."*

**Rule 3 — Reject any story with no score of 4 or higher.** If a story does not have at least one score of 4+ across Viral Potential, Emotional Impact, and Audience Relevance, it must be assigned Rank = Reject. No exceptions.

**Rule 4 — Ranking strictly follows Total Score.** Top 1 = highest total, Top 2 = second highest, Top 3 = third highest. In the event of a tie on Total Score, the tie is broken first by Viral Potential (higher wins), then by Emotional Impact (higher wins). If still tied after both tie-breakers, the story with the higher Audience Relevance wins.

**Rule 5 — Selection Reason must be score-referenced.** Every Selection Reason must explicitly cite the scores (e.g., "Scores 14/15, V:5, E:5, R:4") and must justify why this story beats or loses to competing stories by referencing specific score differences or tie-break outcomes.

**Rule 6 — Weak pool warning.** If all stories in a batch score below 10 Total Score, add the following warning to the Notes column for every row in the batch: *"Low-quality news cycle — weak viral potential."*

### Scoring Dimension Definitions

| Dimension | What It Measures | Score 1–2 | Score 3 | Score 4 | Score 5 |
| --- | --- | --- | --- | --- | --- |
| Viral Potential | How likely is this to get shares, comments, and reactions on TikTok/Instagram? Consider controversy, surprise factor, and relatability. | Niche or dry — general audience scrolls past | Mildly interesting, some shares expected | Strong hook, likely to trend in AI circles | Perfect stop-scroll moment, mass sharing expected |
| Emotional Impact | Does this story provoke strong emotions — fear, excitement, outrage, wonder, or vindication? | Flat — no emotional resonance | Mild interest or curiosity | Strong single emotion (e.g., outrage or excitement) | Multi-layered emotional response — triggers outrage, fear, and vindication simultaneously |
| Audience Relevance | How relevant is this to people who follow AI news channels? Consider broad appeal vs. niche. | Irrelevant or too niche even for AI followers | Relevant to a sub-segment of the AI audience | Relevant to most AI followers | Directly affects every AI follower and has mainstream crossover appeal |

### Content Type Definitions

Every story must be assigned a Content Type before ranking. The Content Type reflects the primary nature of the story and is used to enforce diversity in the Top 3 selection.

| Content Type | Label | Definition |
| --- | --- | --- |
| Viral | Viral / Outrage | High emotional impact. Legal verdicts, controversy, conflict-driven stories, or events that provoke outrage, fear, or strong public reactions. |
| Shock | Shock / Industry Shift | Major unexpected move by a leading company. Product shutdowns, surprise launches, strategic pivots, or reversals that redefine the competitive landscape. |
| Insight | Insight / Curiosity | Interesting, surprising, or highly relevant developments. Partnerships, technical shifts, emerging trends, or stories that inform and intrigue without necessarily provoking outrage. |

**Rule 7 — Top 3 must represent three different Content Types.** The final Top 3 selection must include exactly one Viral story, one Shock story, and one Insight story. This ensures every batch delivers a balanced content mix for the audience.

**Rule 8 — Content Type diversity overrides raw score in Top 3 composition.** When two stories of the same Content Type compete for a Top 3 slot, the lower-scoring story is demoted to Backup regardless of its absolute score, and the highest-scoring story of the missing Content Type is promoted to fill the slot. Raw score tie-breaking (Viral Potential first, then Emotional Impact) applies only within the same Content Type when selecting the representative for that type.

**Rule 9 — Selection Reason must include Content Type justification.** Every Selection Reason must state the story's Content Type, explain why it is the best representative of that type in the batch, and describe how it complements the other two Top 3 stories to complete the required Viral + Shock + Insight mix.

## 3. SCRIPT GATE

**CRITICAL RULE: Script generation ONLY starts when Selection Approved = Approved AND Rank is Top 1, Top 2, or Top 3.**

After script generation begins, update Script Status (Not Started → In Progress → Complete/Failed). Once complete, populate the following fields:
- Script Link (URL to script in Drive)

Self-evaluate the script on the following criteria:
- Hook Strength (1-5)
- Clarity (1-5)
- Flow (1-5)
- TTS Pass (Yes/No)

**Quality threshold:** If Hook Strength < 4 OR Clarity < 4 OR Flow < 4 OR TTS Pass = No, automatically regenerate the script (up to 3 attempts). After regeneration attempts, if the script is still below the threshold, set Script Approved = No, Action Required = Review Script, and notify the user.

If all scripts pass, set Script Approved = Yes, Pipeline Stage = Script, Action Required = None, and proceed automatically to production. Upload working scripts and TTS scripts to the Google Drive Scripts folder with the proper naming convention.

## 4. PRODUCTION GATE

Only proceed if Script Approved = Yes. Generate voiceover via ElevenLabs API (voice: MQ-4-news, voice ID: h5auDwZge17EdlHgtS16). Update Voiceover Status as it progresses (Not Started → In Progress → Complete/Failed). Generate visual assets (keyframes via Nano Banana Pro, animation via Veo 3.1). Update Visuals Status as it progresses. Assemble the final video via FFmpeg. Update Assembly Status.

After assembly, self-evaluate the video on the following criteria:
- Video Quality Score (1-5)
- Sync Pass (Yes/No)
- Visual Consistency (1-5)

**Quality threshold:** If Video Quality Score < 4 OR Sync Pass = No OR Visual Consistency < 4, identify the failing component, fix upstream, and retry (up to 2 attempts).

Upload all assets to proper Google Drive folders with naming conventions:
- Voiceovers to `Voiceovers/YYYY/MM-Month/`
- Visual assets to `Visual Assets/YYYY/MM-Month/`
- Final videos to `Final Videos/YYYY/MM-Month/Week-XX/`

Populate Final Video Link in the Sheet and set Pipeline Stage = Production.

## 5. FINAL APPROVAL GATE

When the final video is ready and passes quality checks, set the following fields:
- Action Required = Approve Video
- Pipeline Stage = Ready

*(Note: Video Approved remains blank until the user explicitly sets it to Yes or No. Do not use "No" to mean "awaiting review".)*

**STOP** — Wait for human approval. Notify the user on Telegram: "3 videos produced and ready for review. Please approve in the Sheet." After the user sets Video Approved = Yes, set Action Required = Post, Pipeline Stage = Ready.

## 6. POSTING AND ARCHIVING

Once a video is posted, update Posted Status to Complete, set the Post Date, and change Pipeline Stage to Posted.

**Archive Rule:** An item moves from the Pipeline tab to the Archive tab when:
- Pipeline Stage = Posted AND Post Date is filled AND at least 7 days have passed since posting, OR
- Pipeline Stage = Posted AND Performance Score has been recorded, OR
- The item is manually archived by the user.

When archiving: copy the entire row to the Archive tab, then delete it from the Pipeline tab.

## 7. FAILURE HANDLING

If any step fails, set Failure Type to the appropriate value: Script, Voice, Visual, Assembly, or Unknown. Set Retry Needed = Yes. Add a clear description in the Notes column explaining what failed and why. Do NOT fail silently — always notify the user on Telegram with the failure details. Do NOT proceed to the next stage until the failure is resolved.

## 8. PIPELINE STAGE DISCIPLINE

Always keep Pipeline Stage accurate and up to date:
- **Intake** → story added to sheet
- **Selected** → scored, ranked, awaiting approval
- **Script** → scripts generated and approved
- **Production** → voiceover, visuals, assembly in progress
- **Ready** → final video complete, awaiting posting approval
- **Posted** → video posted to social media
- **Archived** → moved to Archive tab

## 9. ACTION REQUIRED DISCIPLINE

Always keep Action Required accurate:
- **None** → no human action needed, pipeline can proceed
- **Approve Top 3** → human must review and approve story selection
- **Review Script** → human must review a script that didn't pass auto-quality checks
- **Approve Video** → human must approve final video before posting
- **Post** → video is approved and ready to be posted

---

## EXECUTION SUMMARY

### How the Operational Workflow Runs

The pipeline begins with the generation of a daily briefing. Immediately after generation, stories are loaded into the Google Sheet (Intake stage). The system then scores and ranks the stories based on viral potential, emotional impact, and audience relevance (Selection stage). The pipeline pauses here, awaiting human approval of the Top 3 stories.

Once approved, the system generates scripts for the selected stories. Scripts undergo an automated quality check. If a script fails, it is regenerated up to three times. If it still fails, human intervention is required. If it passes, the pipeline moves to the Production stage.

During Production, voiceovers and visual assets are generated, and the final video is assembled. The video undergoes another automated quality check. If it fails, the system attempts to fix and retry up to two times. Once the video passes, it is uploaded to Google Drive, and the pipeline pauses again, awaiting human approval of the final video (Ready stage).

After the user approves the video, it is marked for posting. Once posted, performance metrics are tracked until the item is archived. Throughout the entire process, the Google Sheet is continuously updated to reflect the current status, and Telegram is used exclusively for alerts and notifications. Any failures are logged in the Sheet and immediately reported via Telegram.

### Human Approval Gates

| Gate | When It Fires | What User Does | Where |
| --- | --- | --- | --- |
| Selection Gate | After stories are scored and ranked | Review scores, approve/reject selections | Google Sheet |
| Script Gate (conditional) | Only if a script fails auto-quality checks | Review and approve/reject script | Google Sheet |
| Final Approval Gate | After videos are produced | Review videos, approve for posting | Google Sheet |

### Fields Updated Automatically at Each Stage

| Stage | Fields Auto-Updated |
| --- | --- |
| Intake | Story ID, Generation Batch ID, Date Added, Topic / Headline, Briefing Link, Pipeline Stage = Intake |
| Selection | Viral Potential, Emotional Impact, Audience Relevance, Total Score, Rank, Selection Reason, Selection Approved = Pending, Pipeline Stage = Selected, Action Required = Approve Top 3 |
| Script | Script Status, Script Link, Hook Strength, Clarity, Flow, TTS Pass, Script Approved, Pipeline Stage = Script |
| Production | Voiceover Status, Visuals Status, Assembly Status, Video Quality Score, Sync Pass, Visual Consistency, Final Video Link, Pipeline Stage = Production |
| Ready | Action Required = Approve Video, Pipeline Stage = Ready |
| Posted | Posted Status, Post Date, Action Required = None, Pipeline Stage = Posted |

---

## SHEET SCHEMA ALIGNMENT

This section serves as the definitive mapping between the live Google Sheet (Pipeline tab) and the operational workflow.

| Col | Column Name | Valid Values / Format | Populated During Stage |
| --- | --- | --- | --- |
| 1 | Story ID | String (Unique ID) | Intake |
| 2 | Generation Batch ID | String (Unique ID) | Intake |
| 3 | Date Added | Date (YYYY-MM-DD) | Intake |
| 4 | Pipeline Stage | Intake, Selected, Script, Production, Ready, Posted, Archived | All Stages |
| 5 | Action Required | None, Approve Top 3, Review Script, Approve Video, Post | All Stages |
| 6 | Topic / Headline | String | Intake |
| 7 | Briefing Link | URL | Intake |
| 8 | Viral Potential | 1, 2, 3, 4, 5 | Selection |
| 9 | Emotional Impact | 1, 2, 3, 4, 5 | Selection |
| 10 | Audience Relevance | 1, 2, 3, 4, 5 | Selection |
| 11 | Total Score | Number (Sum of cols 8-10) | Selection |
| 12 | Rank | Top 1, Top 2, Top 3, Backup, Reject | Selection |
| 13 | Selection Reason | String (Free text) | Selection |
| 14 | Selection Approved | Pending, Approved, Rejected | Selection (Human updates) |
| 15 | Script Status | Not Started, In Progress, Complete, Failed | Script |
| 16 | Script Link | URL | Script |
| 17 | Hook Strength | 1, 2, 3, 4, 5 | Script |
| 18 | Clarity | 1, 2, 3, 4, 5 | Script |
| 19 | Flow | 1, 2, 3, 4, 5 | Script |
| 20 | TTS Pass | Yes, No | Script |
| 21 | Script Approved | Yes, No | Script (or Human if failed) |
| 22 | Voiceover Status | Not Started, In Progress, Complete, Failed | Production |
| 23 | Visuals Status | Not Started, In Progress, Complete, Failed | Production |
| 24 | Assembly Status | Not Started, In Progress, Complete, Failed | Production |
| 25 | Video Quality Score | 1, 2, 3, 4, 5 | Production |
| 26 | Sync Pass | Yes, No | Production |
| 27 | Visual Consistency | 1, 2, 3, 4, 5 | Production |
| 28 | Final Video Link | URL | Production |
| 29 | Video Approved | Yes, No | Ready (Human updates) |
| 30 | Posted Status | Not Started, In Progress, Complete, Failed | Posted |
| 31 | Post Date | Date (YYYY-MM-DD) | Posted |
| 32 | Failure Type | None, Script, Voice, Visual, Assembly, Unknown | Any Stage (on failure) |
| 33 | Retry Needed | Yes, No | Any Stage (on failure) |
| 34 | Notes | String (Free text) | Any Stage |
| 35 | Views | Number | Post-Publishing |
| 36 | Likes | Number | Post-Publishing |
| 37 | Comments | Number | Post-Publishing |
| 38 | Watch Time % | Percentage | Post-Publishing |
| 39 | Performance Score | 1, 2, 3, 4, 5 | Post-Publishing |
