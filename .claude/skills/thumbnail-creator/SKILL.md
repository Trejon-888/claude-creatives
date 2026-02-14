---
name: thumbnail-creator
description: "Create YouTube thumbnails with AI image generation and face compositing. Triggers on 'create thumbnail', 'make thumbnail', 'thumbnail for', 'generate thumbnail', 'youtube thumbnail'."
---

# Thumbnail Creator

Generate high-converting YouTube thumbnails using AI image generation (KIE Nano Banana Pro) with consistent creator face placement, bold text, and professional design.

---

## 7-Phase Workflow

| Phase | Name | What Happens |
|-------|------|-------------|
| 0 | CONTEXT | Load video topic, select creator profile, gather style inputs |
| 1 | CONCEPT | Design 3 thumbnail concepts (text + layout + mood) |
| 2 | PROMPT | Build detailed AI prompts with critical instructions |
| 3 | GENERATE | Create images via Nano Banana Pro API |
| 4 | REVIEW | Present options, get user selection |
| 5 | FINALIZE | Save to organized folders |
| 6 | EVOLVE | Check for pattern improvements |

---

## Assets & References

### Creator Profiles

| Creator | Face Reference | Brand Color | Style |
|---------|---------------|-------------|-------|
| (Your Name) | `.claude/skills/thumbnail-creator/assets/your-name/face-reference.png` | (Your color hex) | (Your style description) |

**Adding a new creator:** Add a subfolder under `assets/{name}/` with `face-reference.png` and update this table.

### Guest/Featured Person References

When thumbnails feature a guest or partner, use their headshot for face alignment:

| Person | Headshot | Label Example |
|--------|----------|---------------|
| (Guest Name) | `assets/headshots/{name}-headshot.png` | "AI AGENCY OWNER" |

**Adding a guest:** Place headshot in `assets/headshots/{name}-headshot.png` and update this table. Guests get a label overlay (their role/result) to help the viewer self-identify.

### Thumbnail Style DNA

Two proven styles for YouTube thumbnails:

#### Style A: White Background (PREFERRED -- proven higher engagement feel)

**Layout Pattern:**
- **White/light background** -- clean, premium aesthetic
- Creator on the RIGHT side (40% of frame), cropped from waist up, holding a prop (phone, laptop)
- Guests/featured people in a **small circle** (12-15% of frame) in the top-left, NOT as full second person
- **Clean illustrations** on the left/center -- topical icons connected by dotted lines/arrows
- Text integrated with the illustration area, NOT oversized

**Typography:**
- Font: Heavy/Black weight sans-serif (Impact, Montserrat Black)
- Size: Medium-large (NOT oversized -- fits the clean aesthetic)
- Color: Dark black `#000000` primary, one word in your brand accent color
- Shadow: Subtle drop shadow for readability on white bg

**Color Palette:**
- Background: White `#FFFFFF` to very light gray gradient
- Primary text: Black `#000000`
- Accent/highlight: Your brand color
- Circle border: Subtle dark shadow

**Key details:**
- Creator wears a **colored t-shirt** (navy, dark blue, black) -- NOT white (blends into bg)
- Chill, natural small smile -- not hyped or forced
- Illustrations are CLEAN vectors, minimal, on-theme
- Guest circle has a label underneath (role/result targeting)

#### Style B: Dark Background (alternative for announcements/news)

**Layout Pattern:**
- Face on the RIGHT side (40-50% of frame), slightly cropped at shoulders
- Bold text on the LEFT side (2-4 words, stacked vertically)
- One accent graphic/logo element
- Dark gradient background (deep blue-black, charcoal, dark teal)

**Typography:**
- Same heavy font but MASSIVE size (fills available space)
- Color: White `#FFFFFF` primary, one word in your brand accent color
- Strong drop shadow or outline for contrast

**When to use which:**
| Scenario | Style |
|----------|-------|
| Tutorials, demos, walkthroughs | Style A (white bg) |
| Case studies, results | Style A (white bg) |
| Announcements, breaking news | Style B (dark bg) |
| Controversy, hot takes | Style B (dark bg) |

**Emotional Tone (both styles):**
- Professional but approachable (not corporate, not childish)
- Curiosity gap (text implies something amazing the viewer needs to see)
- Clean and premium above all -- never cluttered

**Visual Elements (both styles):**
- Clean icons (tech/AI related) -- clean vectors, not messy
- Brand logos of tools mentioned in video
- NO cluttered backgrounds, NO stock photos, NO generic graphics

### Text Formula (Proven Patterns)

| Pattern | Example | When to Use |
|---------|---------|-------------|
| Result statement | "I MADE $90K" | Case studies, results |
| This changes X | "THIS CHANGES EVERYTHING" | Announcements, new tools |
| How I + verb | "HOW I AUTOMATE" | Tutorial, process reveal |
| X is here | "GPT-5 IS HERE" | News, releases |
| Stop + gerund | "STOP POSTING MANUALLY" | Problem-solution |
| Number + noun | "13 PLATFORMS" | Scale, impressive metrics |
| One word shock | "FREE" / "BROKEN" / "INSANE" | Pattern interrupt |

**RULE:** Thumbnail text is NOT the video title. It's 2-4 punchy words that create curiosity. The title provides context; the thumbnail provides emotion.

### Thumbnail Psychology (Title vs Thumbnail Text)

The title and thumbnail serve **completely different psychological roles:**

| Element | Role | Triggers |
|---------|------|----------|
| **Title** | The WHAT (information promise) | Curiosity about content |
| **Thumbnail text** | The WHY YOU CLICK (emotion/identity) | Self-identification, aspiration, FOMO |
| **Background/labels** | Context and authority | Trust, credibility |
| **Face** | Connection and trust | "This person is like me" |

**The "Think like *them*" Pattern:**
A great thumbnail text is a 2-3 word identity trigger that makes the viewer feel something. It does NOT describe the video -- the title already does that. Instead it creates an emotional response:

| Psychology Lever | Example | Why It Works |
|-----------------|---------|--------------|
| **Aspiration** | "Clients on autopilot" | Viewer pictures the dream outcome for themselves |
| **Identity** | "Think like *them*" | Viewer wants to belong to that group |
| **Pain elimination** | "Never outreach again" | Relief from something they hate doing |
| **Permission** | "Let AI close" | Gives them permission to stop struggling |
| **FOMO** | "His AI does everything" | "What am I missing?" |

**Key rule:** If your thumbnail text could BE the title, it's wrong. The text should trigger a FEELING, not describe information.

**Person labels as targeting:** When featuring a guest/partner, add a label like "AI AGENCY OWNER" or "COACH -- $90K/3mo". This makes the viewer self-identify ("that's me" or "I want to be that"). The label targets the avatar psychologically.

---

## Phase 0: Context Gathering

### Required Inputs

1. **Video topic/title** -- What the video is about
2. **Creator** -- Which profile to use (set up your own in the table above)
3. **Key hook** -- What's the one thing that makes someone click?
4. **Tone** -- Excitement / Authority / Curiosity / Urgency (pick one)
5. **Accent element** -- Any specific logo, icon, or graphic to include?

### Automatic Context

Read these files if they exist for the current video:
- Video script (for key moments and hook)
- Content planning doc (for positioning)
- YouTube content package (for title context, to ensure thumbnail text differs from title)

---

## Phase 1: Concept Design

Generate 3 distinct thumbnail concepts. Each concept includes:

```
CONCEPT {N}: "{Working Name}"
------------------------------
Text:        {2-4 words, stacked layout}
Highlight:   {which word is in accent color}
Face:        {expression direction -- looking at camera / looking at text / looking up}
Background:  {color description}
Accent:      {graphic element description}
Mood:        {one-word emotional tone}
Layout:      {text-left/face-right OR face-left/text-right OR centered}
```

**RULE:** All 3 concepts must use DIFFERENT text and different layouts. No recycling.

Present concepts to user. Let them pick one or combine elements.

---

## Phase 2: Prompt Engineering

### Prompt Template (16:9 YouTube Thumbnail)

```
Professional YouTube thumbnail, 1280x720, 16:9 aspect ratio.

[BACKGROUND]
{Background description -- gradient direction, colors, mood lighting, subtle elements}

[SUBJECT - RIGHT SIDE]
{Creator description from profile -- hair, glasses, clothing, etc.}
Positioned on the right 40% of the frame, cropped from mid-chest up.
Natural lighting on face, slight rim light from behind.
Expression: {confident / excited / intrigued / focused}.

[TEXT - LEFT SIDE]
Bold sans-serif text, extremely heavy weight (Black/Impact style):
Line 1: "{WORD_1}" in {color}, {size} relative to frame
Line 2: "{WORD_2}" in {color}, {size} relative to frame
{Line 3 if needed}: "{WORD_3}" in {color}
Text has strong drop shadow (black, 4px offset, 8px blur).
Text fills the left 55% of the frame vertically.
{Highlight word} is in bright accent color with subtle glow.

[ACCENT ELEMENT]
{Description of any logo, icon, or graphic element}
Positioned at {location}, sized at {approximate percentage of frame}.

[CRITICAL INSTRUCTIONS]
- Text must be perfectly readable -- no warping, no perspective distortion
- Face must look photorealistic, natural skin tones
- Background must not compete with text or face for attention
- Overall impression: high-budget YouTube creator, tech/AI niche
- Clean composition, no visual clutter
- Do NOT add any text besides what is specified above
```

### Generation Mode: Image-to-Image with Face References (REQUIRED)

**ALWAYS use `image_to_image` mode** with face references as input from the very first pass. Never use `text_to_image` -- it produces generic faces that require extra passes to fix. By providing face references upfront, the AI incorporates the real likeness into the composition from the start.

#### Single-Person Thumbnail (Creator Only)

**Pass 1:** `image_to_image` with creator face reference as input.

**Input:** `image_input: ["{CREATOR_FACE_URL}"]`

The prompt describes the full composition (layout, text, background, accent elements) while the face reference ensures accurate likeness. One pass, accurate face.

#### Multi-Person Thumbnail (Creator + Guest)

**Pass 1:** `image_to_image` with BOTH face references as input.

**Input:** `image_input: ["{CREATOR_FACE_URL}", "{GUEST_FACE_URL}"]`

The prompt specifies which person goes where:
```
Professional YouTube thumbnail, 1280x720, 16:9.

The first reference image is the main creator -- place on the RIGHT side of the
frame (40% width), cropped from mid-chest up. {Creator description}.

The second reference image is the featured guest -- place on the LEFT side of
the frame (30% width), slightly smaller. Add a label underneath: "{GUEST_LABEL}".
{Guest description}.

[TEXT, BACKGROUND, ACCENT as usual]
```

**If Pass 1 face accuracy is off:** Run a face-alignment Pass 2 with the same face reference + Pass 1 result as inputs.

#### Face Alignment Correction Pass (only if needed)

If a face doesn't look right after Pass 1, run a targeted fix:

**Input:** `image_input: ["{PASS1_RESULT_URL}", "{FACE_REFERENCE_URL}"]`

```
The second image is my real face. Replace the {left/right} person's face in
the first image with my exact likeness. Be detail-oriented: face structure,
hair texture, skin tone, glasses, all distinguishing features. Keep the entire
thumbnail layout, text, logos, and background exactly as-is -- only fix the face.
```

#### Why Image-to-Image First

| Approach | Passes Needed | Face Accuracy | Recommended |
|----------|--------------|---------------|-------------|
| `text_to_image` then face swap | 2-3 passes | Good after fix | No |
| **`image_to_image` with refs from start** | **1 pass (+ fix if needed)** | **Good-to-great** | **Yes** |

Starting with real face references saves passes, saves cost, and produces more consistent results.

### Upload Assets for KIE API

Before generation, upload face reference images to get publicly accessible URLs. You can use the Late API presigned upload:

```bash
# Upload face reference to get a public URL
curl -s -X POST "https://getlate.dev/api/v1/media/presign" \
  -H "Authorization: Bearer $LATE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"filename": "face-reference.png", "contentType": "image/png"}'

# PUT the file to the upload URL
curl -X PUT "$UPLOAD_URL" \
  -H "Content-Type: image/png" \
  --upload-file ".claude/skills/thumbnail-creator/assets/your-name/face-reference.png"
```

Cache uploaded URLs for reuse within the session. No need to re-upload each time.

---

## Phase 3: Generate Images

### API Configuration

| Setting | Value |
|---------|-------|
| API Base | `https://api.kie.ai/api/v1` |
| API Key | `$KIE_API_KEY` (environment variable) |
| Model | `nano-banana-pro` |
| Aspect Ratio | `16:9` |
| Resolution | `4K` (thumbnails need to be crisp) |
| Format | `png` |
| Cost | ~$0.12 per 4K image |

### Generation Process (Image-to-Image)

#### Pass 1: Generate Thumbnail with Face References

Create task with the design prompt + face reference images as input:

**Single person:**
```bash
curl -s -X POST "https://api.kie.ai/api/v1/jobs/createTask" \
  -H "Authorization: Bearer $KIE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nano-banana-pro",
    "input": {
      "prompt": "{FULL_DESIGN_PROMPT}",
      "image_input": ["{CREATOR_FACE_URL}"],
      "aspect_ratio": "16:9",
      "resolution": "4K",
      "output_format": "png"
    }
  }'
```

**Multi-person (creator + guest):**
```bash
curl -s -X POST "https://api.kie.ai/api/v1/jobs/createTask" \
  -H "Authorization: Bearer $KIE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nano-banana-pro",
    "input": {
      "prompt": "{FULL_DESIGN_PROMPT_WITH_BOTH_PEOPLE}",
      "image_input": ["{CREATOR_FACE_URL}", "{GUEST_FACE_URL}"],
      "aspect_ratio": "16:9",
      "resolution": "4K",
      "output_format": "png"
    }
  }'
```

#### Face Correction Pass (only if needed)

If a face doesn't look right, run a targeted correction:

```bash
curl -s -X POST "https://api.kie.ai/api/v1/jobs/createTask" \
  -H "Authorization: Bearer $KIE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nano-banana-pro",
    "input": {
      "prompt": "{FACE_CORRECTION_PROMPT}",
      "image_input": ["{PASS1_RESULT_URL}", "{FACE_REFERENCE_URL}"],
      "aspect_ratio": "16:9",
      "resolution": "4K",
      "output_format": "png"
    }
  }'
```

**IMPORTANT:** For face correction, Pass 1 result goes FIRST, face reference SECOND.

#### Polling for Completion

```bash
curl -s "https://api.kie.ai/api/v1/jobs/recordInfo?taskId={TASK_ID}" \
  -H "Authorization: Bearer $KIE_API_KEY"
```

Poll every 5 seconds. States: `waiting` -> `queuing` -> `generating` -> `success` / `fail`

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

#### Download Results

Download the final thumbnail from the CDN URL and save locally.

---

## Phase 4: Review & Selection

Present all generated thumbnails to the user:

```
THUMBNAIL OPTIONS
=================

Concept 1: "{Name}"
   File: images/thumbnails/{video-slug}/concept-1.png
   Text: {the text on it}

Concept 2: "{Name}"
   File: images/thumbnails/{video-slug}/concept-2.png
   Text: {the text on it}

Concept 3: "{Name}"
   File: images/thumbnails/{video-slug}/concept-3.png
   Text: {the text on it}
```

Ask user:
- "Which thumbnail concept works best?"
- Options: Concept 1 / Concept 2 / Concept 3 / Regenerate with changes

### Iteration

If user wants changes:
1. Adjust the prompt based on feedback
2. Regenerate only the modified concept
3. Present again
4. Max 3 iteration rounds before suggesting manual refinement

---

## Phase 5: Finalize & Save

### File Organization

```
images/thumbnails/
└── {video-slug}/
    ├── concept-1.png
    ├── concept-2.png
    ├── concept-3.png
    ├── final.png              <- Selected/approved thumbnail (4K source)
    ├── final-yt.jpg           <- YouTube-ready (1280x720, JPEG, <2MB)
    └── prompts.json           <- All prompts for reproducibility
```

### Prompts Archive

Save to `images/thumbnails/{video-slug}/prompts.json`:
```json
{
  "metadata": {
    "created": "ISO timestamp",
    "video_title": "...",
    "creator": "your-name",
    "selected_concept": 2
  },
  "concepts": [
    {
      "id": 1,
      "name": "concept name",
      "prompt": "full prompt text",
      "result_url": "KIE CDN URL"
    }
  ]
}
```

### Upload for YouTube

If the video is already posted, upload the thumbnail:

1. Upload to Late storage (get URL)
2. Set thumbnail in YouTube Studio (Late API cannot update thumbnails on existing posts)
3. Change visibility to PUBLIC once thumbnail is set

---

## Phase 6: Evolution Check

At end of every invocation:

1. **Face likeness off?** -> Note which prompt phrasing got closest, update face description
2. **Text unreadable in output?** -> Strengthen text critical instructions
3. **Background too busy?** -> Add "minimal, clean" emphasis
4. **User manually edited?** -> Learn what they changed, update style DNA
5. **New style pattern emerging?** -> Add to Style DNA section

---

## Non-Negotiable Rules

1. **NEVER generate without showing concepts first** -- Always get approval on text + layout before spending API credits
2. **GENERATE 1 thumbnail at a time** -- User picks concept first, then generate. Don't waste credits on 3 parallel generations
3. **ALWAYS use `image_to_image` with face references from Pass 1** -- Never use `text_to_image`. Face refs go in as input images from the start
4. **ALWAYS save prompts** -- Every generation must be reproducible
5. **ALWAYS use 16:9 aspect ratio** -- YouTube standard (1280x720)
6. **ALWAYS use 4K resolution** -- Thumbnails must be crisp at all sizes
7. **THUMBNAIL TEXT IS NOT THE TITLE** -- 2-4 punchy words max, different from video title
8. **Face on one side, text on the other** -- Never overlap face with text
9. **One highlighted word in accent color** -- Creates visual anchor
10. **ALWAYS organize files** -- Dedicated folder per video
11. **ALWAYS use `/api/v1/jobs/recordInfo?taskId=` for polling** -- NOT `/api/v1/jobs/getTaskStatus` (404)
12. **CRITICAL: KIE API field is `image_input`** (array of URLs) -- NOT `input_image_urls`. Wrong field = images silently ignored

---

## Quick Reference

### API Keys
```
KIE_API_KEY: (environment variable)
LATE_API_KEY: (environment variable, for storage uploads)
```

### File Paths
```
Face assets:  .claude/skills/thumbnail-creator/assets/{creator}/face-reference.png
Output:       images/thumbnails/{video-slug}/
```

### Cost Per Thumbnail Session
```
Pass 1 (image_to_image with face refs):  ~$0.12 (4K)
Face correction pass (if needed):        ~$0.12 each
Total:                                   ~$0.12-0.36
```

---

**Version:** 1.0
**API:** KIE.ai Nano Banana Pro
