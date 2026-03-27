# AI Daily Source: Scalable Storage Structure (Final)

This document outlines the refined Google Drive storage architecture designed to support the long-term scalability of the AI Daily Source content pipeline. The structure prioritizes predictable organization, consistent file naming, and seamless integration with the Generation Batch ID system.

## 1. Folder Hierarchy

To balance organization with accessibility, the structure employs a `Year/Month` hierarchy for intermediate assets, and a deeper `Year/Month/Week` hierarchy for final deliverables.

### Final Videos (Year → Month → Week)
Final outputs are organized down to the week to make social media scheduling and historical retrieval effortless.

```text
Final Videos/
└── 2026/
    └── 03-March/
        ├── Week-13/
        │   ├── 2026-03-25_POLICY_WhiteHouse-AI-Framework_v1.mp4
        │   └── 2026-03-25_PRODUCT_OpenAI-Kills-Sora_v1.mp4
        └── Week-14/
            └── ...
```

### Intermediate Assets (Year → Month)
Briefings, Scripts, Voiceovers, and Visual Assets are organized by month to prevent excessive nesting while keeping directories manageable.

```text
Scripts/
└── 2026/
    └── 03-March/
        ├── 2026-03-25_POLICY_WhiteHouse-AI-Framework_working_v1.md
        └── 2026-03-25_POLICY_WhiteHouse-AI-Framework_tts_v1.md

Voiceovers/
└── 2026/
    └── 03-March/
        └── 2026-03-25_POLICY_WhiteHouse-AI-Framework_v1.mp3

Visual Assets/
└── 2026/
    └── 03-March/
        ├── 2026-03-25_POLICY_WhiteHouse-AI-Framework_HOOK_v1.png
        └── 2026-03-25_POLICY_WhiteHouse-AI-Framework_CONTEXT_v1.png

Briefings/
└── 2026/
    └── 03-March/
        └── 2026-03-25_POLICY_WhiteHouse-AI-Framework_briefing_v1.md
```

## 2. File Naming Convention

A strict, predictable file naming convention ensures that any asset can be immediately identified without opening it.

**Format:** `YYYY-MM-DD_TOPIC_SHORTTITLE[_SUFFIX][_VERSION].ext`

### Component Rules

| Component | Description | Rules / Examples |
| :--- | :--- | :--- |
| **YYYY-MM-DD** | Date the story entered the pipeline | `2026-03-25` |
| **TOPIC** | Standardized category (fixed list) | `PRODUCT`, `RESEARCH`, `POLICY`, `BUSINESS`, `SAFETY`, `TOOLS`, `OPEN_SOURCE` |
| **SHORTTITLE** | Concise summary | Max 4 words, no filler words, PascalCase, hyphenated. Example: `OpenAI-Kills-Sora` (not `OpenAI-Decides-To-Kill-Off-Sora-Platform`) |
| **_SUFFIX** | Asset type or script section | Scripts: `_working`, `_tts`<br>Visuals: `_HOOK`, `_CONTEXT`, `_SIGNIFICANCE`, `_CTA`<br>Briefings: `_briefing` |
| **_VERSION** | Versioning for regenerated assets | `_v1`, `_v2`, `_v3` |
| **.ext** | File extension | `.mp4`, `.md`, `.mp3`, `.png` |

### Complete Naming Examples

- **Briefing:** `2026-03-25_PRODUCT_OpenAI-Kills-Sora_briefing_v1.md`
- **Working Script:** `2026-03-25_PRODUCT_OpenAI-Kills-Sora_working_v2.md`
- **TTS Script:** `2026-03-25_PRODUCT_OpenAI-Kills-Sora_tts_v2.md`
- **Voiceover:** `2026-03-25_PRODUCT_OpenAI-Kills-Sora_v2.mp3`
- **Visual Asset (Hook):** `2026-03-25_PRODUCT_OpenAI-Kills-Sora_HOOK_v1.png`
- **Visual Asset (CTA):** `2026-03-25_PRODUCT_OpenAI-Kills-Sora_CTA_v1.png`
- **Final Video:** `2026-03-25_PRODUCT_OpenAI-Kills-Sora_v1.mp4`

## 3. Generation Batch ID Mapping

The **Generation Batch ID** (tracked in the Google Sheet) serves as the connective tissue between the operational dashboard and the file system.

**Batch ID Format:** `BATCH-YYYYMMDD-XX` (e.g., `BATCH-20260325-01`)

When a batch is generated, all resulting files inherit the date from the Batch ID. This guarantees that if an operator needs to find the assets for `BATCH-20260325-01`, they know exactly where to look:
- **Year Folder:** `2026`
- **Month Folder:** `03-March`
- **File Prefix:** `2026-03-25_*`

*Note: The Google Sheet will continue to track the direct link to the Final Video in the "Final Video Link" column for immediate access.*

## 4. Auto-Creation Logic in the Pipeline

To prevent manual overhead and ensure structural integrity, folder creation will be automated within the production pipeline script.

**The Logic Flow:**
1. **Batch Initialization:** When a new batch is started (e.g., `BATCH-20260325-01`), the pipeline extracts the Year (`2026`), Month (`03`), and calculates the ISO Week number (`13`).
2. **Directory Check:** The pipeline queries Google Drive to check if the required `Year/Month` and `Year/Month/Week` folders exist within the root asset folders.
3. **Auto-Creation:** If a folder does not exist, the pipeline creates it using the Google Workspace API and caches the new Folder ID.
4. **Asset Upload:** As scripts, audio, and videos are generated, they are uploaded directly into these dynamically verified/created folders using the strict naming convention.

This ensures the folder structure scales infinitely without requiring human operators to manually create "April" or "Week-14" folders ahead of time.
