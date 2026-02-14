# Output Directory

All generated content is auto-organized here by type and project.

## Structure

```
output/
├── thumbnails/                     # YouTube thumbnails
│   └── YYYY-MM-DD-video-slug/      # One folder per video
│       ├── concept-1.png           # Option A
│       ├── concept-2.png           # Option B
│       ├── concept-3.png           # Option C
│       ├── final.png               # Selected + face-corrected
│       └── prompts.json            # AI prompts used (for regeneration)
│
├── carousels/                      # AI-generated image carousels
│   └── YYYY-MM-DD-topic-slug/      # One folder per carousel
│       ├── slide-01.png            # Individual slides
│       ├── slide-02.png
│       ├── ...
│       └── prompts.json            # AI prompts per slide (for regeneration)
│
├── documents/                      # Document carousels (HTML → PDF → PNG)
│   └── YYYY-MM-DD-topic-slug/      # One folder per document
│       ├── source.html             # Original HTML
│       ├── document.pdf            # Rendered PDF
│       ├── pages/                  # Extracted page images
│       │   ├── page-01.png
│       │   ├── page-02.png
│       │   └── ...
│       └── metadata.json           # Outline, page count, platforms posted
│
└── posts/                          # Mixed-format posts (Gary Vee style, etc.)
    └── YYYY-MM-DD-post-slug/       # One folder per post
        ├── hook-image.png          # Image hook (first carousel slide)
        ├── video.mp4               # Video component (if applicable)
        ├── slides/                 # Additional slides (if multi-image)
        │   ├── slide-01.png
        │   └── ...
        └── metadata.json           # Platform, caption, hashtags, post URL
```

## Naming Convention

Every project folder follows: `YYYY-MM-DD-descriptive-slug`

Examples:
- `2026-02-13-ai-content-creation-tutorial`
- `2026-02-14-5-ai-tools-for-creators`
- `2026-02-15-claude-code-demo-reel`

## Regeneration

Each folder includes a `prompts.json` or `metadata.json` file with the exact prompts and settings used. To regenerate:

1. Open the project folder
2. Tell Claude: "Regenerate [thumbnail/carousel] using the prompts in [folder]"
3. Claude reads the saved prompts and re-runs the generation

This means you never lose your settings — every output is reproducible.
