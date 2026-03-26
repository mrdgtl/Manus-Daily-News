# AI Daily Source — Project Documentation

> **Tagline:** Daily sources of AI news, summarized and shared.
>
> **Owner:** Manu
>
> **Status:** Planning
>
> **Last Updated:** March 25, 2026

---

## 1. Vision & Purpose

AI Daily Source is a TikTok and Instagram channel that transforms verified AI news into short-form video content. The channel exists to serve a growing audience of professionals, students, and technology enthusiasts who want to stay informed about artificial intelligence but lack the time or patience to read lengthy articles, research papers, or press releases. Each video covers a single news story — what happened, why it matters — delivered through AI-generated voiceover and visuals, with a caption that links directly to the original source.

The channel is built on top of an existing asset: **Manus Daily News**, a working AI news briefing system that already researches, verifies, and produces structured briefings three times per week. AI Daily Source does not create new journalism; it repackages verified reporting into a format optimized for social media consumption. This distinction is important. The editorial integrity of the source material is already established. The challenge is purely one of production and distribution — turning structured text into compelling video at a consistent cadence.

A secondary channel called **Volcaneer** (tagline: "The hottest daily AI news") is planned for the future. It will follow the same pipeline with a different editorial voice and content selection strategy. However, the focus of this document is entirely on AI Daily Source. Volcaneer will be addressed only in the context of Phase 6 (Scale & Clone) and is not a priority until the primary pipeline is proven.

---

## 2. Content Pipeline Overview

The pipeline from news briefing to posted video consists of six discrete stages. Each stage has a clear input, a defined transformation, and a specific output. The pipeline is designed to be modular so that individual stages can be improved, automated, or replaced without disrupting the others.

### Pipeline Summary

| Stage | Name | Input | Transformation | Output |
| :---: | :--- | :--- | :--- | :--- |
| 1 | News Source | Raw AI news landscape | Research, verify, structure | Briefing with 8–12 verified stories |
| 2 | Article Extraction | Full briefing document | Decompose into individual units | Individual content cards (one per story) |
| 3 | Script Writing | Individual content card | Transform into TTS-optimized script | 30–60 second video script |
| 4 | Video & Voice Production | Video script | Generate voiceover, visuals, assemble | Final vertical video (9:16) |
| 5 | Social Distribution | Final video + metadata | Post, caption, schedule | Published content on TikTok and Instagram |
| 6 | Community Management | Audience comments | Respond, engage, moderate | Active community interaction |

### Stage 1: News Source (Already Built)

The Manus Daily News briefing system is fully operational. It produces structured AI news briefings on Tuesday, Thursday, and Saturday. Each briefing contains 8 to 12 verified stories, and each story includes a headline, a summary, the original source URL, and category tags. This stage requires no further development. It is the foundation upon which everything else is built.

### Stage 2: Article Extraction

The briefing is a single document containing multiple stories. In this stage, each story is extracted and packaged as an individual content unit — a "content card" — containing the headline, summary, source link, and category tag. This is a straightforward text processing task. The output of this stage is a queue of content cards, each representing one potential video.

### Stage 3: Script Writing

This is the most creatively demanding stage. Each content card is transformed into a short-form video script optimized for AI text-to-speech delivery. The script is not a summary; it is a performance piece designed to hold attention in a feed where viewers decide to stay or scroll within the first two seconds.

The script specification is as follows:

| Parameter | Specification |
| :--- | :--- |
| **Target Duration** | 30–60 seconds when read aloud |
| **Word Count** | Approximately 75–150 words |
| **Tone** | Conversational, confident, slightly urgent — not robotic, not overly casual |
| **Hook** | Must appear in the first 3 seconds; a provocative question, a surprising fact, or a bold statement |
| **Structure** | Hook → Context (what happened) → Significance (why it matters) → Call to action |
| **Call to Action** | "Follow for more AI news," "Link in bio for the full story," or similar |
| **TTS Optimization** | No abbreviations (write "artificial intelligence," not "AI" if the TTS mispronounces it), no complex punctuation, natural sentence rhythm, short sentences for pacing |

The quality bar for scripts is high. A script that reads well on paper but sounds stilted when spoken aloud by a TTS engine is not acceptable. Scripts must be validated by listening to them, not just reading them.

### Stage 4: Video & Voice Production

The script drives the production of three assets that are assembled into the final video.

**Voiceover audio** is generated using AI text-to-speech. The voice must sound natural, authoritative, and engaging. It should not sound like a GPS navigation system or a customer service bot. The quality of the voiceover is the single most important factor in whether a viewer stays or scrolls.

**Video visuals** accompany the voiceover. These may include stock footage, AI-generated imagery, animated text overlays, screen recordings, or a combination. The visual style should be clean, modern, and informative — not cluttered or gimmicky. The visuals serve the story; they do not compete with it.

**Final assembly** combines the voiceover and visuals into a single vertical video (9:16 aspect ratio) suitable for TikTok and Instagram Reels. This includes any transitions, text overlays, branding elements, and background music.

### Stage 5: Social Distribution

The finished video is posted to both TikTok and Instagram. Each post requires a caption that includes a concise summary of the story, the original source link, and relevant hashtags. Posts should be scheduled at times optimized for audience engagement, which will vary by platform and will need to be refined over time based on analytics.

The initial approach will be semi-automated: videos are produced by the pipeline but posted manually or through a scheduling tool. Full automation — where the pipeline produces and posts without human intervention — is a Phase 4 objective.

### Stage 6: Community Management

Audience engagement does not end at posting. Comments on TikTok and Instagram require responses that are timely, on-brand, and substantive. AI agents can be deployed to handle routine interactions (thanking commenters, answering simple questions, directing people to sources), while more complex or sensitive interactions are flagged for human review.

The community management persona for AI Daily Source should be helpful, knowledgeable, and approachable — like a well-informed friend who happens to follow AI news closely.

---

## 3. What Manus Can Do Today vs. What Needs External Tools

Honesty about capabilities is essential for planning. The following assessment distinguishes between what Manus can handle natively and what will require external tools, APIs, or services.

| Stage | Capability | Manus Native? | Details |
| :--- | :--- | :---: | :--- |
| **Stage 1** | News research and briefing | Yes | Already built and operational. |
| **Stage 2** | Article extraction | Yes | Text processing and structuring — straightforward for Manus. |
| **Stage 3** | Script writing | Yes | LLM-based generation with specific prompt engineering. Manus can produce, iterate, and refine scripts natively. |
| **Stage 4** | AI voiceover generation | Partial | Manus has access to text-to-speech capabilities. Quality and naturalness need to be evaluated against dedicated TTS services like ElevenLabs, PlayHT, or open-source options like Kokoro. |
| **Stage 4** | Video visual generation | Partial | Manus can generate video content using its media generation capabilities (veo3.1, nano banana). Whether the output meets TikTok-quality standards for this specific use case needs to be tested. |
| **Stage 4** | Video assembly and editing | Uncertain | Combining voiceover, visuals, text overlays, and branding into a polished final product may require dedicated video editing tools or programmatic assembly (e.g., FFmpeg, Remotion, or similar). |
| **Stage 5** | Automated posting | No | TikTok and Instagram do not allow posting through general-purpose AI agents. This requires either their official APIs (TikTok Content Posting API, Instagram Graph API) or third-party scheduling tools (Later, Buffer, Metricool). |
| **Stage 5** | Caption and hashtag generation | Yes | Text generation for captions and hashtag selection is well within Manus's capabilities. |
| **Stage 6** | Comment response generation | Yes | Manus can generate appropriate responses. However, delivering those responses to the platforms requires API access or tools like ManyChat or NapoleonCat. |
| **Stage 6** | Comment monitoring | No | Requires platform API access or integration with a social media management tool. |

The key takeaway is that Manus can handle the **content creation** side of the pipeline (Stages 1–3 and parts of Stage 4) with high confidence. The **production**, **distribution**, and **engagement** sides (later parts of Stage 4, Stages 5–6) will require external integrations that need to be evaluated and selected.

---

## 4. Phased Roadmap for AI Daily Source

The project is structured into six phases. Each phase builds on the previous one, and no phase should be started until the preceding phase has been validated. This is not a waterfall plan — it is a series of build-measure-learn cycles.

| Phase | Name | Objective | Key Deliverables | Dependencies |
| :---: | :--- | :--- | :--- | :--- |
| 1 | Script Pipeline | Prove that briefings can be reliably converted into high-quality video scripts | Article extraction workflow, script writing prompts, 10+ sample scripts, read-aloud validation | Manus Daily News briefings |
| 2 | Video Production | Prove that scripts can be turned into watchable videos | Voiceover tool selection, visual style guide, 5+ sample videos, quality validation | Validated scripts from Phase 1 |
| 3 | Manual Posting & Feedback | Prove that the content resonates with a real audience | Published videos, engagement metrics, audience feedback, style refinements | Sample videos from Phase 2 |
| 4 | Automated Distribution | Remove manual bottlenecks from the posting process | Scheduling tool integration, automated caption generation, posting cadence optimization | Validated content from Phase 3 |
| 5 | Community Management | Build and maintain an engaged audience | Comment response personas, AI response system, moderation workflow | Active channels from Phase 4 |
| 6 | Scale & Clone | Replicate the pipeline for Volcaneer | Adapted pipeline, new brand voice, separate channel launch | Proven pipeline from Phases 1–5 |

### Phase 1 — Script Pipeline (Start Here)

This is the highest-leverage phase because it is entirely within Manus's native capabilities and requires no external tools or spending. The work involves building the article extraction process that takes a full briefing and outputs individual content cards, then developing the script writing workflow with carefully engineered prompts that produce scripts meeting the specification defined in Section 2. The phase concludes by producing at least 10 sample scripts from real briefings and validating them by generating audio and listening to them read aloud. A script that sounds awkward, rushed, or unnatural when spoken is a failed script, regardless of how well it reads on paper.

### Phase 2 — Video Production

With validated scripts in hand, the focus shifts to production. This phase requires evaluating AI voiceover options across two dimensions: naturalness of the voice and cost per minute of audio. It also requires defining a visual style for the channel — what the videos actually look like. The deliverable is a small batch of complete sample videos that represent the target quality level. These videos do not need to be perfect; they need to be good enough to post without embarrassment.

### Phase 3 — Manual Posting & Feedback

The first videos are posted manually to TikTok and Instagram. The purpose of this phase is not growth; it is learning. Which stories get the most engagement? Do viewers watch to the end? Are they commenting? What do the comments say? This feedback loop is essential for refining every upstream stage — from story selection to script tone to visual style.

### Phase 4 — Automated Distribution

Once the content formula is validated, the manual posting process becomes a bottleneck. This phase introduces automation through scheduling tools or direct API integration. The goal is to reduce the time between "video is ready" and "video is posted" to near zero, while maintaining control over timing and quality.

### Phase 5 — Community Management

An active comment section signals to the algorithm that the content is engaging, which drives further distribution. This phase sets up AI-powered comment responses that are on-brand and substantive. The system should handle routine interactions automatically and escalate anything sensitive or complex.

### Phase 6 — Scale & Clone

With a proven, optimized pipeline for AI Daily Source, the same infrastructure is cloned and adapted for Volcaneer. The content selection criteria, editorial voice, visual style, and posting cadence may all differ, but the underlying pipeline architecture remains the same.

---

## 5. Content Strategy Notes

The existing Manus Daily News system produces 8 to 12 stories per briefing, three times per week. This yields a theoretical maximum of 24 to 36 content cards per week — far more than needed for a daily posting cadence. This surplus is a strategic advantage because it allows for curation rather than obligation. Not every story needs to become a video. The editorial process should select stories based on three criteria: **visual potential** (can this story be illustrated compellingly?), **audience relevance** (will the target audience care about this?), and **narrative clarity** (can this be explained in 60 seconds or less?).

The recommended starting cadence is 1 to 2 videos per day, which translates to 7 to 14 videos per week. Given the supply of 24 to 36 content cards, this means roughly one-third to one-half of all stories will be produced as videos. The rest can be held in reserve, used for other formats (e.g., carousel posts, text posts), or simply discarded.

Consistency matters more than volume on both TikTok and Instagram. Posting 7 videos per week, every week, is significantly more valuable than posting 14 videos one week and none the next. The pipeline should be designed to maintain a steady output even when briefing volume fluctuates.

---

## 6. Success Criteria (Validation Framework)

| Dimension | Definition |
| :--- | :--- |
| **Problem** | People want AI news in a digestible, short-form video format. They do not have time to read articles, but they scroll TikTok and Instagram daily. |
| **Success** | Growing follower count, increasing engagement rates (likes, comments, shares, saves), consistent posting schedule maintained over time, and audience retention metrics that show viewers watching past the first 3 seconds. |
| **Failure** | Persistently low engagement despite consistent posting, poor audio or video quality that cannot be resolved, inability to maintain a consistent posting cadence, or negative audience feedback about content quality or accuracy. |
| **Good Enough to Ship** | Scripts that sound natural when spoken by a TTS engine, basic but clean video production (no jarring cuts, legible text, appropriate pacing), and a consistent posting schedule of at least 5 times per week. Perfection is not the goal; reliability is. |
| **Risk Tier** | Tier 2 (moderate). The content is public-facing and represents a brand, but the compliance risk is low. The primary risks are reputational (posting inaccurate or low-quality content) and operational (failing to maintain consistency). |

---

## 7. Open Questions

The following questions do not have answers yet. They are listed here to ensure they are tracked and addressed as the project progresses through its phases.

| # | Category | Question | When to Resolve |
| :---: | :--- | :--- | :--- |
| 1 | **Voiceover** | What AI voiceover tool produces the most natural results at a sustainable cost? Leading candidates include ElevenLabs, PlayHT, and open-source options like Kokoro. | Phase 2 |
| 2 | **Video Quality** | Can Manus's native video generation capabilities (veo3.1, nano banana) produce content that meets TikTok and Instagram quality standards for this use case? | Phase 2 |
| 3 | **Distribution** | What is the most reliable approach for automated TikTok and Instagram posting? Options include the TikTok Content Posting API, Instagram Graph API, or third-party tools like Later, Buffer, or Metricool. | Phase 4 |
| 4 | **Visual Identity** | Should videos feature a human-like AI avatar (talking head style) or use a faceless approach with text overlays, stock footage, and AI-generated imagery? | Phase 2 |
| 5 | **Aesthetics** | What specific visual style resonates best with the target audience? Options range from minimalist and clean to data-heavy and infographic-driven. | Phase 3 (informed by audience feedback) |
| 6 | **Platform Differences** | How should the pipeline handle platform-specific requirements? TikTok and Instagram Reels have different optimal video lengths, trending audio norms, hashtag strategies, and algorithm behaviors. | Phase 3 |
| 7 | **Video Assembly** | What tooling is needed for final video assembly (combining voiceover, visuals, text overlays, and branding)? Options include FFmpeg, Remotion, Descript, CapCut, or Manus's native capabilities. | Phase 2 |
| 8 | **Community Tooling** | Which tool is best suited for AI-powered comment management on TikTok and Instagram? Leading candidates include ManyChat, NapoleonCat, and custom API integrations. | Phase 5 |

---

## Appendix: Channel Identity

| Attribute | AI Daily Source | Volcaneer (Future) |
| :--- | :--- | :--- |
| **Tagline** | Daily sources of AI news, summarized and shared | The hottest daily AI news |
| **Platforms** | TikTok, Instagram | TikTok, Instagram |
| **Tone** | Informative, conversational, trustworthy | Energetic, opinionated, bold |
| **Content Focus** | Broad AI news coverage | Trending and high-impact stories only |
| **Priority** | Primary — build first | Secondary — clone after AI Daily Source is proven |
