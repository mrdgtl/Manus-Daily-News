# Prompt System Changelog

## v1.4 — 2026-03-27

**v4 Production Upgrade — Video Clips and Pan Removal (8 changes)**

- Updated `visual-generator.md` to v1.2:
  - Switched primary visual mode from static images to AI-generated video clips (3–8 seconds per segment: Hook, Context, Significance, CTA)
  - Added fallback mode: static images with FFmpeg zoom when video generation is unavailable, with mandatory logging
  - Removed all pan effects entirely — horizontal pan, vertical pan, subtle pan, slide in/out are no longer allowed motion effects
  - Standardized on smooth zoom in/zoom out only (with sinusoidal ease-in/ease-out); fade in/out retained for CTA/end card
  - Updated motion mapping: Hook = zoom in, Context = zoom out, Significance = zoom in, CTA = fade in (Context changed from pan to zoom out)
  - Added video clip assembly pipeline (concatenation with crossfade, voiceover overlay, end card append)
  - Kept branded end card requirement (3 seconds, [LOGO], tagline)
- Updated `production-evaluator.md` to v1.2:
  - Removed all references to pan effects from acceptable motion
  - Updated motion quality checks: only zoom in/out and fade are acceptable; any pan, slide, or jerky motion lowers scores
  - Added evaluation criteria for AI-generated video clip smoothness — stuttery or jerky clips lower Visual Consistency (capped at 4/5)
  - Pan or slide motion detected in any segment now caps Visual Consistency at 3/5
  - Updated perfect score requirements to explicitly prohibit pan effects and require smooth video clips
- Updated `OPERATIONAL-WORKFLOW.md`:
  - Updated Prompt File Authority table to reflect v1.2 for visual-generator and production-evaluator
  - Replaced static image generation with AI-generated video clips as primary visual mode
  - Updated visual motion rules to prohibit all pan effects, standardize on zoom only
  - Updated video assembly instructions for both primary (video clips) and fallback (static images with zoom) modes

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
