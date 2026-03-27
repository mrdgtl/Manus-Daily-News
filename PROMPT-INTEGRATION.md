# Prompt Integration Document

This document serves as the canonical reference for how versioned prompt files integrate into the live workflow of the AI Daily Source pipeline.

## 1. Prompt File Mapping

The following table maps the active prompt files to their respective workflow stages:

| Prompt File | Workflow Stage | When Triggered | Replaces |
|---|---|---|---|
| `selection-engine.md` | Selection Gate | After briefing intake, before script generation | Implicit scoring logic in OPERATIONAL-WORKFLOW.md |
| `script-generator.md` | Script Generation | After Selection Approved = Approved for Top 3 | Implicit generation logic in PIPELINE-TECHNICAL-SPEC.md |
| `script-evaluator.md` | Script Quality Evaluation | After script generation, before production | Implicit evaluation logic in OPERATIONAL-WORKFLOW.md |

## 2. Integration Rules

The following rules govern the use of prompt files in the pipeline:

- These prompt files are the **PRIMARY** source of instructions for their respective stages.
- If any implicit logic, documentation, or behavioral pattern conflicts with the prompt file content, the **prompt file wins**.
- Prompt files must **NOT** be modified during execution — they are read-only references.
- Any changes to prompt behavior must go through version control (update the prompt file, bump version, update `CHANGELOG.md`).

## 3. Implicit Components

The following components still run on implicit instructions and are not yet covered by versioned prompt files:

### Track 1
- News Crawler: Implicit — no prompt file yet
- Relevance Filter: Implicit — no prompt file yet
- Summarizer: Implicit — no prompt file yet
- Fact Check: Implicit — no prompt file yet
- Trend Analyst: Implicit — no prompt file yet
- Presenter: Implicit — no prompt file yet
- Delivery: Implicit — no prompt file yet

### Track 2
- Story Intake (loading to Sheet): Implicit — no prompt file yet
- Voiceover Generation: Implicit — no prompt file yet
- Visual Asset Generation: Implicit — no prompt file yet
- Video Assembly: Implicit — no prompt file yet
- Production Quality Evaluation: Implicit — no prompt file yet
- Google Drive Upload: Implicit — no prompt file yet
- Google Sheet Updates: Implicit — no prompt file yet

## 4. Conflicts Resolved During Integration

During the integration of these prompt files, the following conflicts with existing documentation were identified and resolved:

- **Selection Engine vs. OPERATIONAL-WORKFLOW.md (Section 2A):**
  While `OPERATIONAL-WORKFLOW.md` contains verbose scoring rules and tie-breaking procedures, `selection-engine.md` provides a more concise and strict set of rules (e.g., mandatory 1-5 range, rejection for scores < 4, and strict content type diversity). `selection-engine.md` is now the canonical source for all scoring and selection logic.

- **Script Generator vs. PIPELINE-TECHNICAL-SPEC.md:**
  `PIPELINE-TECHNICAL-SPEC.md` (Stage 4) states that the system selects "3-5 most compelling stories" for script generation. However, `script-generator.md` strictly constrains generation to exactly the Top 3 stories (Rank = Top 1, Top 2, or Top 3) and introduces specific timing constraints (e.g., Hook in the first 2 seconds). `script-generator.md` is now the canonical source for script generation behavior.

- **Script Evaluator vs. OPERATIONAL-WORKFLOW.md:**
  The evaluation criteria (Hook Strength, Clarity, Flow, TTS Pass) and the retry logic (up to 3 attempts) in `OPERATIONAL-WORKFLOW.md` align closely with `script-evaluator.md`. However, the prompt file formalizes this as a strict quality control system with explicit output requirements. `script-evaluator.md` is now the canonical source for script quality evaluation.
