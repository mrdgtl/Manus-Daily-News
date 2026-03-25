# Project Roadmap
**Project Name:** Manus Daily News — AI News Agentic Flow

## Introduction
This document serves as the implementation plan for the Manus Daily News project. While the Product Requirements Document (PRD) defines the "what" and "why" of the system, this Roadmap outlines the "how" and "when." It breaks down the project's evolution into six actionable phases, providing a clear, ordered plan for development. This is a living document that will be updated as progress is made and priorities shift.

## Status Overview

| Phase | Focus | Status |
| :--- | :--- | :--- |
| **Phase 1** | Automation | In Progress |
| **Phase 2** | Refinement | Not Started |
| **Phase 3** | Pipeline Hardening | Not Started |
| **Phase 4** | Intelligence Layer | Not Started |
| **Phase 5** | Distribution | Not Started |
| **Phase 6** | Community / Sharing | Not Started |

---

## Phase 1 — Automation (Current/Next)
**Objective:** Get the system running autonomously on a daily schedule.

*   **Key Tasks:**
    *   Set up a scheduled trigger for Tuesday, Thursday, and Saturday at 5:00 AM ET within Manus.
    *   Implement configurable schedule days — Manu can change which days are active via a simple request, without requiring any technical changes.
    *   Establish a manual "Start AI news briefing" command for on-demand execution, independent of the scheduled days.
    *   Automate the delivery of the final briefing to Telegram.
    *   Automate committing the generated briefings to the GitHub repository under the `briefings/` folder.
    *   Configure the GitHub push mechanism using the classic Personal Access Token (PAT).
*   **Deliverables:** A working daily automation pipeline and the first week of automated briefings successfully committed to the repository.
*   **Definition of Done:** Briefings are generated and delivered daily without any manual intervention for 5 consecutive days.
*   **Dependencies:** GitHub PAT (Completed), repository setup (Completed), first briefing template (Completed).
*   **Risks:** Manus scheduling reliability and the credit consumption required per daily run.

## Phase 2 — Refinement
**Objective:** Improve briefing quality based on real daily usage and user feedback.

*   **Key Tasks:**
    *   Collect daily feedback from Manu regarding the quality, relevance, and accuracy of the briefings.
    *   Tune the story selection criteria to better define what constitutes a "top-10 caliber" story.
    *   Adjust the source list by adding high-value sources or removing noisy/low-quality ones.
    *   Refine the summary style, length, and tone to ensure maximum readability.
    *   Adjust category tags if the current set proves insufficient or overly broad.
    *   Improve the TrendAnalyst agent's output based on which insights prove most useful.
*   **Deliverables:** Updated prompt templates, a refined and tested source list, and a documented feedback log.
*   **Definition of Done:** Manu is consistently satisfied with the briefing quality for 1 full week straight.
*   **Dependencies:** Phase 1 must be complete (daily briefings are running reliably).
*   **Risks:** Quality is subjective; establishing a clear and actionable feedback loop is critical.

## Phase 3 — Pipeline Hardening
**Objective:** Make the system robust, resilient, and error-tolerant.

*   **Key Tasks:**
    *   Implement a source diversity check to ensure no single source accounts for more than 40% of the day's stories.
    *   Add link validation to verify that all included URLs return a 200 OK status before publishing.
    *   Build duplicate detection to prevent the same story from appearing across consecutive daily briefings.
    *   Add fallback behaviors and graceful degradation for when primary sources are unavailable or scraping fails.
    *   Implement error logging to capture and diagnose pipeline failures.
    *   Add strict enforcement for summary length constraints.
*   **Deliverables:** Quality check modules, an error logging system, and resilience documentation.
*   **Definition of Done:** The system successfully handles simulated or real source failures gracefully and maintains all quality checks for 2 consecutive weeks.
*   **Dependencies:** Phase 2 must be complete (a stable quality baseline has been established).
*   **Risks:** Over-engineering the pipeline; checks must remain pragmatic and not overly complicate the workflow.

## Phase 4 — Intelligence Layer
**Objective:** Add cross-day memory and evolving story tracking to the system.

*   **Key Tasks:**
    *   Build a story tracking mechanism across days to detect recurring topics and narratives.
    *   Implement detection and flagging for "developing stories" that require ongoing coverage.
    *   Add week-over-week trend comparison capabilities.
    *   Create a running index or database of covered stories to aid in deduplication and provide historical context.
    *   Enable the TrendAnalyst agent to reference previous briefings when generating the daily "Big Picture" section.
*   **Deliverables:** A functional story tracking system and enhanced trend analysis that incorporates historical context.
*   **Definition of Done:** Briefings actively reference developing stories and demonstrate week-over-week contextual awareness.
*   **Dependencies:** Phase 3 must be complete (the pipeline is reliable), and a sufficient history of briefings must be accumulated.
*   **Risks:** System complexity; the intelligence layer needs to remain elegant and not become over-built or fragile.

## Phase 5 — Distribution
**Objective:** Expand delivery channels beyond the initial Telegram integration.

*   **Key Tasks:**
    *   Evaluate potential distribution options, including email newsletters, Slack, Discord, or a dedicated web dashboard.
    *   Implement at least one additional delivery channel.
    *   Create a simple web-based briefing viewer or dashboard for easier reading.
    *   Set up a weekly digest compilation that summarizes the most important stories of the week.
*   **Deliverables:** At least one additional operational delivery channel and a functional weekly digest format.
*   **Definition of Done:** Briefings are accessible and reliably delivered through at least 2 distinct channels.
*   **Dependencies:** Phase 2 and beyond must be complete (the briefings must be stable and of high enough quality to warrant broader distribution).
*   **Risks:** Scope creep; it is vital to pick one new channel to implement first before expanding further.

## Phase 6 — Community / Sharing
**Objective:** Enable broader audience access and public sharing, if desired.

*   **Key Tasks:**
    *   Evaluate newsletter platforms (e.g., Substack, Buttondown) for public distribution.
    *   Set up a public-facing briefing page or official newsletter.
    *   Add subscriber management and onboarding flows.
    *   Consider branding, design, and presentation adjustments suitable for an external audience.
*   **Deliverables:** A public newsletter or web page and a functional subscriber management system.
*   **Definition of Done:** An external audience can successfully subscribe to and receive the daily briefings.
*   **Dependencies:** Phase 5 must be complete (distribution infrastructure is in place), and Manu must make the strategic decision to take the project public.
*   **Risks:** Content liability, managing audience expectations, and the ongoing maintenance burden of a public-facing product.
