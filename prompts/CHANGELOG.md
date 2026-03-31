# Prompt System Changelog

## v1.8 — 2026-03-30

**v8 Production Upgrade — Final Outro Voiceover Refinement**

- Updated `visual-generator.md` to v1.6:
  - Updated standard outro voiceover line to "All sources linked below. Follow us for what matters in AI."
- Updated `track1-briefing/script-writer.md` to v1.3:
  - Updated standard outro voiceover line to "All sources linked below. Follow us for what matters in AI."
- Updated `production-evaluator.md` to v1.6:
  - Updated standard outro voiceover line reference.
- Updated `OPERATIONAL-WORKFLOW.md`:
  - Updated Prompt File Authority table to reflect v1.6 for visual-generator and production-evaluator, and v1.3 for script-writer.
  - Updated Video Assembly Instructions to reflect the new standard outro voiceover line.

## v1.7 — 2026-03-30

**v7 Production Upgrade — Outro Refinement and Elastic Clip Calculation**

- Updated `visual-generator.md` to v1.5:
  - Updated standard outro voiceover line to "Sources in the comments. Follow for what matters in AI."
  - Replaced rigid 5-second clip division with elastic clip calculation rules (3-8s range, cutting on natural beats).
- Updated `track1-briefing/script-writer.md` to v1.2:
  - Updated standard outro voiceover line to "Sources in the comments. Follow for what matters in AI."
- Updated `production-evaluator.md` to v1.5:
  - Updated standard outro voiceover line reference.
- Updated `OPERATIONAL-WORKFLOW.md`:
  - Updated Prompt File Authority table to reflect v1.5 for visual-generator and production-evaluator, and v1.2 for script-writer.
  - Updated Video Assembly Instructions to reflect the new standard outro voiceover line.

## v1.6 — 2026-03-30

**v6 Production Upgrade — Multi-Clip Scenes and Standard Outro Voiceover**

- Updated `visual-generator.md` to v1.4:
  - Replaced "one long clip per segment" with multi-clip scenes (~5s clips).
  - Added calculation rules for determining clip count based on voiceover duration.
  - Added standard outro voiceover line integration playing over the branded end card.
- Updated `track1-briefing/script-writer.md` to v1.1:
  - Added mandatory standard outro voiceover line ("Find sources in the comment section, and follow for what matters in AI.") to the end of every CTA.
  - Specified timing for the outro line to play over the branded outro clip.
- Updated `production-evaluator.md` to v1.4:
  - Added penalty (Visual Consistency capped at 3/5) for stretching/looping single clips instead of using multi-clip scenes.
  - Made missing standard outro voiceover line an automatic fail.
- Updated `OPERATIONAL-WORKFLOW.md`:
  - Updated Prompt File Authority table to reflect v1.4 for visual-generator and production-evaluator, and v1.1 for script-writer.
  - Updated Video Assembly Instructions to reflect multi-clip generation and standard outro voiceover.

## v1.5 — 2026-03-30

**v5 Timeline and Assembly Upgrade — Elastic Timeline and Outro Fixes**

- Updated `visual-generator.md` to v1.3:
  - Removed hard duration constraints (30-40s cap) and introduced a script-driven elastic timeline (45-90s target).
  - Enforced visual clip duration to match or exceed voiceover duration per segment.
  - Replaced hard cuts with fades/crossfades.
  - Ensured outro is appended only after CTA completes naturally with a transition.
- Created `track1-briefing/script-writer.md` v1.0:
  - Introduced 45-60s target duration (max 90s for exceptional news).
  - Added strict hook strategy focusing on value without clickbait.
  - Required clear timing guidance for each section.
- Updated `production-evaluator.md` to v1.3:
  - Added Timeline & Assembly Integrity gate check (automatic fail if narration is cut off, CTA is truncated, outro interrupts abruptly, or video duration < audio duration).
  - Lowered Video Quality Score for hard cuts between segments and abrupt outro transitions.
- Updated `OPERATIONAL-WORKFLOW.md`:
  - Updated Prompt File Authority table to reflect v1.3 for visual-generator and production-evaluator, and added script-writer v1.0.
  - Updated Video Assembly Instructions to reflect the new script-driven elastic timeline and outro insertion rules.

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
