---
name: carousel-generator
description: Generate professional LinkedIn/Instagram carousel images using AI image generation (Nano Banana Pro). Ensures logo consistency, text legibility, brand color matching. Triggers on "create carousel", "make carousel", "generate carousel", "carousel images", "carousel slides".
argument-hint: [carousel_topic]
---

# Carousel Image Generator

Generate professional multi-slide carousel images for LinkedIn/Instagram using AI image generation with brand consistency guarantees.

---

## 5-Phase Workflow

| Phase | Name | What Happens |
|-------|------|-------------|
| 0 | GATHER | Collect topic, slide count, brand assets, style references |
| 1 | PROMPTS | Generate detailed AI prompts with critical instructions |
| 2 | GENERATE | Create images via Nano Banana Pro API, organize in folders |
| 3 | REVIEW | Quality check, offer regeneration for problematic slides |
| 4 | EVOLVE | Check for quality patterns, update templates |

---

## Visual Style Modes

Before generating prompts, analyze the content and select the appropriate visual style.

### Mode Selection Logic

| Mode | Content Signals | Visual Expression |
|------|-----------------|-------------------|
| **Editorial** | Numbers, quotes, case studies, guarantees, step-by-step | Beige/cream + accent color pill cards + serif |
| **Emergence** | Personal journey, breakthrough, before/after, transformation | Dark void + mint/green glow, minimal |
| **Fever Dream** | Unexpected, absurd, rule-breaking, surreal, pattern interrupt | Glitch, asymmetry, unexpected color |
| **Video Game** | Stats, results, achievements, milestones, progress | HUD elements, progress bars, badges |
| **Raw Real** | Unpolished truth, behind-the-scenes, vulnerability | Minimal, text-forward, screenshot aesthetic |

### Selection Process

1. **Read the content/script** -- What's the emotional journey?
2. **Identify core signals** -- Transformation? Authority? Pattern interrupt? Achievement? Confession?
3. **Match to mode** based on signals
4. **Apply mode-specific visual DNA** in prompts
5. **Verify:** Does the visual feel like the intended experience?

### Mode-Specific Prompt Adjustments

**Editorial (default for most carousels):**
- Background: Beige/cream (#FAF8F5) + subtle grid
- Primary: Your brand accent gradient
- Secondary: Charcoal #333333
- Cards: Pill-shaped, 60px radius, 3-8 degree rotation
- Typography: Elegant serif headlines, sans body
- Feel: Clean, editorial -- premium but human

**Emergence:**
- Background: Deep void hsl(220 3% 2%)
- Accent: Mint hsl(158 84% 65%)
- Elements: Subtle glow, minimal decoration
- Feel: Something appearing from nothing

**Fever Dream:**
- Unexpected color combinations
- Glitch elements, visual noise
- Asymmetry, broken grids
- Feel: "Wait, what?" -- can't immediately categorize

**Video Game:**
- Progress bars, stat displays
- Achievement badges
- HUD elements
- Feel: Dopamine hit of progress

**Raw Real:**
- Minimal design, text-forward
- Screenshot/notes app aesthetic
- No polish, no gradients
- Feel: Pure authenticity

---

## Phase 0: Gather Requirements

Ask the user:

1. **Carousel Topic:** What's the main message? (e.g., "5 AI Tools Every Creator Needs")
2. **Number of Slides:** How many? (recommend 6-8 for LinkedIn)
3. **Content Structure:**
   - Cover slide text?
   - Key points for each slide?
   - CTA message and URL?
4. **Brand Assets:**
   - Logo file path (optional)
   - Primary brand color hex
   - Secondary color hex (optional)
   - Background color preference
5. **Style Reference:** Any reference image paths?

### Automatic Folder Setup

Create organized structure:
```bash
mkdir -p "output/carousels/{carousel-slug}"
```

**RULE:** Always use kebab-case for folder names.

---

## Phase 1: Generate Prompts

For EACH slide, create detailed prompt following this template:

### Critical Instructions (MANDATORY in every prompt)

```
[CRITICAL INSTRUCTION - DO NOT MODIFY LOGO]
Place the {LOGO_NAME} logo at exactly ({X}px, {Y}px), sized at {SIZE}px.
Do not modify, recolor, or reinterpret the logo.

[CRITICAL INSTRUCTION - TEXT MUST BE READABLE]
All text must be horizontal and readable from left to right.
Even if cards are rotated, text INSIDE cards must remain perfectly horizontal.

[STYLE REFERENCE]
{Style description from user's reference images or selected visual mode}
```

### Prompt Structure by Slide Type

**Cover/Intro Slides:**
- Main title with emphasized word in brand color
- Subtitle or hook
- Logo placement
- Creator attribution ("MADE BY {NAME}")
- Category label

**Content Slides:**
- Section title in brand color
- Pill-shaped cards with key information
- Card rotation: max +/-15 degrees (CRITICAL: keeps text readable)
- Drop shadows for depth

**Comparison Slides:**
- Two pill-shaped cards (left: old way, right: new way)
- Contrasting colors (gray vs brand color gradient)

**CTA/Outro Slides:**
- Main heading with brand color emphasis
- Large pill-shaped CTA card
- URL in monospace font
- Forward arrow icon

### Save Prompts

Write to: `output/carousels/{carousel-slug}/{carousel-slug}-prompts.json`

Structure:
```json
{
  "metadata": {
    "created": "ISO timestamp",
    "topic": "carousel topic",
    "brand_colors": {
      "primary": "#hex",
      "secondary": "#hex"
    },
    "logo_path": "path/to/logo.png"
  },
  "slides": [
    {
      "id": 1,
      "name": "slide-slug",
      "prompt": "full detailed prompt"
    }
  ]
}
```

Show prompts to user for approval before generation.

---

## Phase 2: Generate Images

**RULE:** Use Nano Banana Pro API at `https://api.kie.ai`

### API Configuration

- **Model:** `nano-banana-pro`
- **Aspect Ratio:** `1:1` (square for carousel)
- **Resolution:** `2K` (2048x2048px) -- balances quality and cost
- **Format:** `png`
- **Cost:** ~$0.09 per 2K image, ~$0.12 per 4K image

### Generation Process

For each slide:

1. **Create Task:**
```bash
curl -s -X POST "https://api.kie.ai/api/v1/jobs/createTask" \
  -H "Authorization: Bearer $KIE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nano-banana-pro",
    "input": {
      "prompt": "{slide_prompt}",
      "aspect_ratio": "1:1",
      "resolution": "2K",
      "output_format": "png"
    }
  }'
```

2. **Poll for Completion:**

```bash
curl -s "https://api.kie.ai/api/v1/jobs/recordInfo?taskId={TASK_ID}" \
  -H "Authorization: Bearer $KIE_API_KEY"
```

- Check every 3 seconds
- Max wait: 2 minutes per image
- States: waiting -> queuing -> generating -> success/fail

**CRITICAL:** Use `/api/v1/jobs/recordInfo?taskId=` NOT `/api/v1/jobs/getTaskStatus` (that endpoint returns 404).

Response on success:
```json
{
  "code": 200,
  "data": {
    "state": "success",
    "resultJson": "{\"resultUrls\": [\"https://cdn.kie.ai/...\"]}"
  }
}
```

Extract URL: `JSON.parse(data.resultJson).resultUrls[0]`

3. **Download & Save:**
- Save to: `output/carousels/{carousel-slug}/{number:02d}-{slide-slug}.png`

4. **Progress Updates:**
```
Slide X: {name}
   Creating generation task...
   Task created: {taskId}
   Generating....... Done
   Downloading image...
   Saved: {filename}
```

### File Organization

```
output/carousels/
└── {carousel-slug}/
    ├── 01-slide-name.png
    ├── 02-slide-name.png
    ├── ...
    ├── {carousel-slug}-prompts.json
    └── reference-*.jpg (if any)
```

### Naming Conventions

- **Carousel folders:** kebab-case (e.g., `outreach-2022-vs-2026`)
- **Slide files:** `{number:02d}-{slide-slug}.png` (e.g., `01-cover-intro.png`)
- **Prompts file:** `{carousel-slug}-prompts.json`
- **References:** `reference-{n}-{description}.{ext}`

---

## Phase 3: Review & Evolve

After all images generated:

### Quality Checklist

- [ ] Logo appears consistently in all slides
- [ ] Logo is not modified, rotated, or recolored
- [ ] All text is horizontal and readable (no upside-down text)
- [ ] Brand colors match exact hex codes
- [ ] Typography is consistent across slides
- [ ] Card rotation is appropriate (15 degrees or less)
- [ ] Files are named sequentially

### Common Issues & Fixes

**Issue: Upside-down text on rotated cards**
-> Strengthen text legibility instruction: "Text must be horizontal even though card is rotated"

**Issue: Logo modified or repositioned**
-> Add explicit coordinates and "DO NOT MODIFY" emphasis

**Issue: Inconsistent style**
-> Strengthen [STYLE REFERENCE] section with more specific details

### Regeneration

If user identifies problematic slides:
1. Update prompt with stronger instructions
2. Regenerate ONLY those slides (reuse same folder)
3. Repeat quality check

### Final Output Summary

```
Carousel Generated Successfully!

Summary:
- Topic: {Carousel Name}
- Total slides: X
- Resolution: 2K (2048x2048px)
- Format: PNG
- Cost: ~$X.XX

Files: output/carousels/{carousel-slug}/

Ready to post!
```

---

## Phase 4: Evolution Check

At end of every invocation:

1. **Repeated logo issues?** -> Strengthen default logo instructions in template
2. **Repeated text legibility issues?** -> Reduce max rotation limit or add more examples
3. **New slide type pattern emerging?** -> Add template variant
4. **User manually fixed prompts?** -> Learn the pattern, update template
5. **Carousel folder disorganized?** -> Improve folder creation logic

---

## Non-Negotiable Rules

1. **ALWAYS include [CRITICAL INSTRUCTION] sections** in every prompt
2. **NEVER exceed +/-15 degree rotation** for cards (prevents text legibility issues)
3. **ALWAYS create dedicated folders** for each carousel (no dump folder)
4. **ALWAYS use exact hex codes** for brand colors (no approximations)
5. **ALWAYS validate prompts** before generation (check for critical sections)
6. **ALWAYS show preview** after each slide for early feedback
7. **ALWAYS use `/api/v1/jobs/recordInfo?taskId=` for polling** -- NOT `/api/v1/jobs/getTaskStatus` (404)
8. **CRITICAL: KIE API field is `image_input`** (array of URLs) -- NOT `input_image_urls`

---

## Quick Reference

### API Keys
```
KIE_API_KEY: (environment variable)
```

### Cost Per Carousel
```
6 slides at 2K: ~$0.54
8 slides at 2K: ~$0.72
6 slides at 4K: ~$0.72
8 slides at 4K: ~$0.96
```

---

**Version:** 1.0
**API:** KIE.ai Nano Banana Pro
