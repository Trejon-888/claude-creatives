# Claude Canvas

A Claude Code project with 3 skills for creating visual content: YouTube thumbnails, carousel images, and educational document carousels.

This is **Part 2** of the AI content creation system. Part 1 ([IX AI Agent Social Media Manager](https://github.com/Trejon-888/ix-ai-agent-social-media-manager)) handles content distribution to 13+ platforms.

---

## Setup

### Required API Keys

Before using any skill, you need two API keys:

1. **KIE API Key** — For AI image generation (thumbnails and carousel images)
   - Sign up at [kie.ai](https://kie.ai)
   - Get your API key from the dashboard
   - Store it as an environment variable: `KIE_API_KEY`

2. **Late API Key** — For media storage uploads and social media posting
   - Sign up at [getlate.dev](https://getlate.dev)
   - Create an API key in Settings > API Keys
   - Store it as an environment variable: `LATE_API_KEY`

### Setting API Keys

Set your keys as environment variables in your shell profile:

**Windows (PowerShell):**
```powershell
$env:KIE_API_KEY = "your-kie-api-key-here"
$env:LATE_API_KEY = "your-late-api-key-here"
```

**Mac/Linux (bash/zsh):**
```bash
export KIE_API_KEY="your-kie-api-key-here"
export LATE_API_KEY="your-late-api-key-here"
```

Or add them to a `.env` file in the project root (never commit this file).

### Optional Tools

- **Google Chrome** — Required for Document Carousel PDF generation (uses Chrome headless)
- **Python 3.10+** with `pymupdf` — For extracting PDF pages as PNG images
- **FFmpeg** — For creating slideshow videos from carousel pages

---

## Available Skills

### 1. Thumbnail Creator

**Triggers:** "create thumbnail", "make thumbnail", "thumbnail for", "generate thumbnail", "youtube thumbnail"

**What it does:** Generates YouTube thumbnails using AI image generation (KIE Nano Banana Pro) with face compositing, bold text, and professional design.

**Workflow:** Context gathering -> 3 concept designs -> User picks one -> AI generation with face reference -> Face correction if needed -> Save and organize

**Skill file:** `.claude/skills/thumbnail-creator/SKILL.md`

### 2. Carousel Generator

**Triggers:** "create carousel", "make carousel", "generate carousel", "carousel images", "carousel slides"

**What it does:** Creates professional multi-slide carousel images for LinkedIn/Instagram using AI image generation with brand consistency.

**Workflow:** Gather requirements -> Generate prompts with critical instructions -> AI generation per slide -> Quality check -> Organize files

**Skill file:** `.claude/skills/carousel-generator/SKILL.md`

### 3. Document Carousel

**Triggers:** "carousel document", "document carousel", "guide carousel", "LinkedIn PDF", "educational carousel"

**What it does:** Creates educational carousel documents as HTML, converts to PDF, extracts page images for posting as carousels across social platforms.

**Workflow:** Context -> Outline -> Write HTML -> Generate PDF -> Extract PNG pages -> Create content package -> Publish (optional)

**Skill file:** `.claude/skills/document-carousel/SKILL.md`

---

## File Organization

All generated content goes into `output/`, organized by type and date:

```
output/
├── thumbnails/                      # YouTube thumbnails
│   └── YYYY-MM-DD-video-slug/       # One folder per video
│       ├── concept-1.png            # Option A
│       ├── concept-2.png            # Option B
│       ├── concept-3.png            # Option C
│       ├── final.png                # Selected + face-corrected
│       └── prompts.json             # AI prompts (for regeneration)
│
├── carousels/                       # AI-generated image carousels
│   └── YYYY-MM-DD-topic-slug/       # One folder per carousel
│       ├── slide-01.png
│       ├── slide-02.png
│       └── prompts.json             # Prompts per slide (for regeneration)
│
├── documents/                       # Document carousels (HTML → PDF → PNG)
│   └── YYYY-MM-DD-topic-slug/
│       ├── source.html              # Original HTML
│       ├── document.pdf             # Rendered PDF
│       ├── pages/                   # Extracted page images
│       │   ├── page-01.png
│       │   └── ...
│       └── metadata.json            # Outline, pages, platforms posted
│
└── posts/                           # Mixed-format posts (Gary Vee style, etc.)
    └── YYYY-MM-DD-post-slug/
        ├── hook-image.png           # Image hook (first carousel slide)
        ├── video.mp4                # Video component (if applicable)
        └── metadata.json            # Platform, caption, hashtags, post URL
```

**Naming convention:** Always use `YYYY-MM-DD-descriptive-slug` for project folders.

**References:** Style inspiration images go in `references/`:
```
references/
├── carousel-styles/     # Pinterest saves, Instagram screenshots
└── thumbnail-styles/    # YouTube thumbnail references
```

When the user says "use the style from references/carousel-styles/clean-minimal.png", analyze that image and match the visual style.

**Regeneration:** Every project folder includes `prompts.json` or `metadata.json` with the exact prompts and settings used. When asked to regenerate, read the saved prompts and re-run.

---

## Key APIs

### KIE.ai (Image Generation)

- **Base URL:** `https://api.kie.ai/api/v1`
- **Model:** `nano-banana-pro`
- **Create task:** `POST /jobs/createTask`
- **Poll status:** `GET /jobs/recordInfo?taskId={id}`
- **States:** waiting -> queuing -> generating -> success/fail
- **CRITICAL:** The field for input images is `image_input` (array of URLs), NOT `input_image_urls`

### Late (Media Storage + Social Posting)

- **Base URL:** `https://getlate.dev/api/v1`
- **Upload:** `POST /media/presign` -> PUT to upload URL -> use `publicUrl`
- **Post:** `POST /posts` with `platforms[]` array for multi-platform posting
- **CRITICAL:** Domain is `getlate.dev` NOT `api.getlate.dev`

---

## Brand Customization

To personalize for your brand:

1. **Thumbnail Creator:** Update the creator profile table in the SKILL.md with your name, face reference path, and brand colors
2. **Carousel Generator:** Update default brand colors and logo path in Phase 0
3. **Document Carousel:** Update CSS variables and logo references in Phase 2

---

## Tips

- Always let Claude show you concepts/outlines before generating (saves API credits)
- Face references should be clear, well-lit photos with face visible
- For carousels, 6-8 slides is the sweet spot for LinkedIn engagement
- Keep thumbnail text to 2-4 punchy words (not the video title)
- Document carousels work best for educational/guide content
