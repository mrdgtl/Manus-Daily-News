# AI Daily Source Pipeline: Operational Workflow

This document defines the complete workflow rules for the AI Daily Source pipeline. The Google Sheet dashboard serves as the source of truth and main control system for this operational workflow.

## 1. SHEET-FIRST OPERATION

Every story must be added to the Google Sheet (ID: `1Nc_O0Ina5gnz2X7hV0E1ia_NN1eArW3bTP1pcF2bbO4`, Pipeline tab) immediately after briefing generation. All intake and selection fields must be populated before moving forward. The Google Sheet is now the main operational control system. Telegram is only for alerts and delivery notifications (e.g., "Briefing complete — 8 stories loaded to Sheet. Awaiting Top 3 approval.").

## 2. SELECTION GATE

Each story must be scored for Viral Potential (1-5), Emotional Impact (1-5), and Audience Relevance (1-5). The Total Score is calculated as the sum of these three metrics. Based on the scores, assign a Rank: Top 1, Top 2, Top 3, Backup, or Reject. A Selection Reason (free text explaining why this story was ranked this way) must be added.

Set the following fields:
- Selection Approved = Pending
- Pipeline Stage = Selected
- Action Required = Approve Top 3

**STOP** — Wait for human approval before script generation. Notify the user on Telegram: "Selection complete. Top 3 stories scored and ranked in the Sheet. Please review and approve."

## 3. SCRIPT GATE

Only generate scripts for stories where Selection Approved = Approved AND Rank = Top 1, Top 2, or Top 3. After script generation, populate the following fields:
- Script Status = Done
- Script Link (URL to script in Drive)

Self-evaluate the script on the following criteria:
- Hook Strength (1-5)
- Clarity (1-5)
- Flow (1-5)
- TTS Pass (Yes/No)
- Script Approved (Yes/No)

**Quality threshold:** If Hook Strength < 4 OR Clarity < 4 OR Flow < 4 OR TTS Pass = No, automatically regenerate the script (up to 3 attempts). After regeneration attempts, if the script is still below the threshold, set Script Approved = No, Action Required = Review Script, and notify the user.

If all scripts pass, set Pipeline Stage = Script, Action Required = None, and proceed automatically to production. Upload working scripts and TTS scripts to the Google Drive Scripts folder with the proper naming convention.

## 4. PRODUCTION GATE

Only proceed if Script Approved = Yes. Generate voiceover via ElevenLabs API (voice: MQ-4-news, voice ID: h5auDwZge17EdlHgtS16). Update Voiceover Status as it progresses (Not Started → In Progress → Done/Failed). Generate visual assets (keyframes via Nano Banana Pro, animation via Veo 3.1). Update Visuals Status as it progresses. Assemble the final video via FFmpeg. Update Assembly Status.

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
- Video Approved = No (awaiting human review)
- Action Required = Approve Video
- Pipeline Stage = Ready

**STOP** — Wait for human approval. Notify the user on Telegram: "3 videos produced and ready for review. Please approve in the Sheet." After the user sets Video Approved = Yes, set Action Required = Post, Pipeline Stage = Ready.

## 6. FAILURE HANDLING

If any step fails, set Failure Type to the appropriate value: Script, Voice, Visual, Assembly, or Unknown. Set Retry Needed = Yes. Add a clear description in the Notes column explaining what failed and why. Do NOT fail silently — always notify the user on Telegram with the failure details. Do NOT proceed to the next stage until the failure is resolved.

## 7. PIPELINE STAGE DISCIPLINE

Always keep Pipeline Stage accurate and up to date:
- **Intake** → story added to sheet
- **Selected** → scored, ranked, awaiting approval
- **Script** → scripts generated and approved
- **Production** → voiceover, visuals, assembly in progress
- **Ready** → final video complete, awaiting posting approval
- **Posted** → video posted to social media
- **Archived** → moved to Archive tab

## 8. ACTION REQUIRED DISCIPLINE

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

After the user approves the video, it is marked for posting. Throughout the entire process, the Google Sheet is continuously updated to reflect the current status, and Telegram is used exclusively for alerts and notifications. Any failures are logged in the Sheet and immediately reported via Telegram.

### Human Approval Gates

| Gate | When It Fires | What User Does | Where |
| --- | --- | --- | --- |
| Selection Gate | After stories are scored and ranked | Review scores, approve/reject selections | Google Sheet |
| Script Gate (conditional) | Only if a script fails auto-quality checks | Review and approve/reject script | Google Sheet |
| Final Approval Gate | After videos are produced | Review videos, approve for posting | Google Sheet |

### Fields Updated Automatically at Each Stage

| Stage | Fields Auto-Updated |
| --- | --- |
| Intake | Story ID, Generation Batch ID, Briefing Date, Headline, Source, Source URL, Category, Pipeline Stage = Intake |
| Selection | Viral Potential, Emotional Impact, Audience Relevance, Total Score, Rank, Selection Reason, Selection Approved = Pending, Pipeline Stage = Selected, Action Required = Approve Top 3 |
| Script | Script Status, Script Link, Hook Strength, Clarity, Flow, TTS Pass, Script Approved, Pipeline Stage = Script |
| Production | Voiceover Status, Visuals Status, Assembly Status, Video Quality Score, Sync Pass, Visual Consistency, Final Video Link, Pipeline Stage = Production |
| Ready | Video Approved = No, Action Required = Approve Video, Pipeline Stage = Ready |
| Posted | Posted Status, Post Date, Action Required = None, Pipeline Stage = Posted |
