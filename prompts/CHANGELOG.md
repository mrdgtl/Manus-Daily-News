# Prompt System Changelog

## v1.2 — 2026-03-27

- Added `voiceover-generator.md` (v1.0) to enforce ElevenLabs MQ-4-news voice and strict fallback approval rules
- Added `visual-generator.md` (v1.0) to require light motion treatment (subtle zoom, pan, crossfade) via FFmpeg
- Added `production-evaluator.md` (v1.0) to enforce strict quality control on voice fidelity, visual motion, and sync
- Updated `OPERATIONAL-WORKFLOW.md` to integrate new voiceover, visual motion, and production evaluation rules
- Updated `PROMPT-INTEGRATION.md` to map the new production prompt files to their respective workflow stages

## v1.1 — 2026-03-27

- Updated script-evaluator.md to v1.1
- Added stricter evaluation skepticism criteria (penalizing generic hooks, overstatement, legal exaggeration, stiff phrasing, and ungrounded significance claims)
- Clarified that a Hook score of 5 requires both factual discipline and exceptional stop-scroll power

## v1.0.1 — 2026-03-27

- Integrated prompt files into live workflow as canonical instruction sources
- Created PROMPT-INTEGRATION.md mapping document
- Updated OPERATIONAL-WORKFLOW.md with Prompt File Authority section
- Resolved conflicts: prompt files now override implicit logic

## v1.0 — 2026-03-27

- Initial prompt architecture created
- Added core structure for Track 2 production prompts
- Prepared system for prompt version control
- Added selection engine prompt v1.0
- Added script generator prompt v1.0
- Added script evaluator prompt v1.0
