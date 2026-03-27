# Prompt System Changelog

## v1.3 — 2026-03-27

**v2 Production Review — Visual and Motion Upgrades (7 changes)**

- Updated `visual-generator.md` to v1.1:
  - Removed constant/random micro-animation behavior; motion is now intentional, subtle, and story-driven
  - Enforced strict 9:16 vertical orientation (1080x1920) with hard validation — non-vertical assets are marked as failed (no rotation or force-fitting)
  - Added mandatory motion easing (ease-in/ease-out curves); linear motion is prohibited; documented FFmpeg easing expressions using sinusoidal/cosine curves
  - Added standardized motion system: one effect per segment (slow zoom in, slow zoom out, subtle pan, fade in/out, slide in/out) with recommended mapping per script section (Hook → zoom in, Context → pan, Significance → zoom out, CTA → fade in)
  - Structured visual direction prompts as format-agnostic keyframes for future AI video generation compatibility (Veo, Runway, Pika)
  - Added mandatory branded end card: 3 seconds, 1080x1920, dark background, `[LOGO]` placeholder, tagline "Daily AI news, simplified.", appended as static image during assembly
- Updated `production-evaluator.md` to v1.1:
  - Added Orientation Check (Pass/Fail) as an automatic fail gate — non-vertical visuals cannot pass QC
  - Added End Card Check (Pass/Fail) — missing end card caps Video Quality Score at 3/5
  - Added motion quality evaluation: excessive/unnatural motion, lack of easing, and stacked effects all reduce scores
  - Updated perfect score (5/5) requirements: correct orientation, intentional eased motion, clean transitions, branded voice, end card present, and professional polish
- Updated `OPERATIONAL-WORKFLOW.md`:
  - Added orientation validation rules to Production Gate
  - Replaced old visual motion rules with v1.1 standardized motion system reference
  - Added branded end card assembly instructions to Production Gate
  - Updated Prompt File Authority table to reflect v1.1 for visual-generator and production-evaluator

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
