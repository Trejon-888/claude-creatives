---
name: document-carousel
description: "Create educational carousel documents (HTML to PDF) and optionally post as image carousels across platforms. Triggers on 'carousel document', 'document carousel', 'guide carousel', 'LinkedIn PDF', 'create carousel', 'make a guide', 'educational carousel'."
argument-hint: [topic or brief]
---

# Document Carousel Creator

**Purpose:** Create professional, educational carousel documents as HTML files and auto-generate PDFs for social media posting. Produces clean, Google Docs-style guides -- not image-based carousels.

---

## Distinction from carousel-generator

| Skill | Output | Style | Tool |
|-------|--------|-------|------|
| `carousel-generator` | AI-generated images (PNG) | Visual, brand-heavy | KIE API |
| `document-carousel` | HTML to PDF document | Text-heavy, educational | Chrome headless |

Use `document-carousel` when the goal is an **educational guide, explainer, or reference document** that reads like a well-formatted Google Doc.

---

## 7-Phase Workflow

| Phase | Name | What Happens |
|-------|------|-------------|
| 0 | CONTEXT | Understand topic, gather research, identify audience |
| 1 | OUTLINE | Structure the document into 6-10 pages with clear sections |
| 2 | WRITE | Create HTML document with all content and styling |
| 3 | GENERATE | Render PDF via Chrome headless and open for review |
| 4 | EXTRACT | Convert PDF pages to PNG images, create slideshow video |
| 5 | PACKAGE | Create platform-specific content, show for user approval |
| 6 | PUBLISH | Upload media, post via Late API (optional) |

---

## Phase 0: Context

1. Ask user for the topic or accept the provided brief
2. Identify the brand: name, logo path, accent color, website URL
3. Research the topic if external knowledge is needed (web search)
4. Identify the audience and what transformation the document delivers

---

## Phase 1: Outline

Structure the document as 6-10 pages:

| Page | Purpose | Pattern |
|------|---------|---------|
| 1 | **Cover** | Title, subtitle, byline, logo |
| 2 | **The Problem** | Pain point the reader has |
| 3 | **The Solution** | What this is and how it works |
| 4-6 | **Deep Dive** | Core capabilities, technology, process |
| 7 | **Results / Proof** | Numbers, testimonials, timelines |
| 8 | **Why Different** | Comparison to alternatives |
| 9 | **CTA** | Next steps, link, closing |

**RULE:** Every page must have a clear section label (Roman numerals: I. II. III.) and a heading that stands alone as a statement.

Present outline to user for approval before writing.

---

## Phase 2: Write HTML

### Document Template

Use the proven HTML template structure:

```
<page>
  <content> section label -> heading -> body </content>
</page>
```

**RULE: NO repeating headers or footers.** Social media carousels are viewed on mobile at small sizes. Every pixel matters. Headers (logo + title) and footers (domain + page numbers) eat ~13% of vertical space per page and make text unreadable on mobile. Instead:
- Logo appears ONLY on the cover page (integrated into the cover design)
- Logo + domain appear ONLY on the final CTA page
- No page numbers (users swipe, they don't need numbers)
- No repeating headers on content pages (section labels are self-labeling)

**RULE: `@page { margin: 0 !important; }` in CSS.** Prevents Chrome from adding its own date/title/URL artifacts when generating PDF. Also add in `@media print` block.

### Design System

**RULE:** The document must look like it came from Google Docs. Clean, restrained, professional.

**Typography:**
- Font: Inter (Google Fonts import)
- H1: 56px / 700 weight (cover only)
- H2: 36px / 700 weight (page headings)
- H3: 20px / 600 weight (subheadings)
- Body: 17px / 400 weight / 1.7 line-height
- Section labels: 14px / 600 / uppercase / accent color

**Colors (CSS variables -- customize to your brand):**
```css
--text-primary: #111827;
--text-secondary: #4B5563;
--text-tertiary: #9CA3AF;
--accent-color: #4DB89A;    /* Replace with your brand color */
--border: #E5E7EB;
--bg-subtle: #F9FAFB;
```

**Page dimensions:**
- Width: 816px (US Letter)
- Min-height: 1056px
- Padding: 48px 64px 44px (no header/footer = more content space)
- `page-break-after: always` on each page

**Components available:**
- Callout box (left accent border + subtle bg) -- for quotes and key takeaways
- Feature rows (icon + text, separated by hairline borders)
- Stats grid (3-col with dividers, clean numbers)
- Comparison table (two-column, header + rows)
- Pill tags (small rounded labels for categories/channels)
- Flow diagram (horizontal steps with arrows)
- Numbered lists (plain `1. 2. 3.` style)
- Bullet lists (small gray dots)
- Horizontal rules (`<hr>`)

**Icons:**
- **RULE:** Never use emojis. Use inline SVG icons from Feather icon set (stroke-based, 18x18px, accent color stroke).
- Common icons: phone, message-square, users, shield, mail, zap, check-circle, arrow-right

**What NOT to do:**
- No emoji anywhere in the document
- No heavy font weights (max 700)
- No colored backgrounds on cards (use borders and subtle bg only)
- No rounded corners above 8px
- No dark-mode sections (keep everything white/light)
- No marketing-speak in headings (write like a guide author, not a copywriter)

### Cover Page Logo Template

Logo appears ONLY on the cover page, integrated into the cover body:

```html
<div class="cover-logo">
  <img src="path/to/your-logo.png" alt="Your Brand">
  <span class="cover-logo-text">YOUR BRAND</span>
</div>
```

And on the final CTA page, centered at the bottom:

```html
<div style="text-align: center;">
  <div class="cover-logo" style="justify-content: center;">
    <img src="path/to/your-logo.png" alt="Your Brand">
    <span class="cover-logo-text">YOUR BRAND</span>
  </div>
  <p style="font-size: 13px; color: var(--text-tertiary);">
    Your tagline &mdash; yourdomain.com
  </p>
</div>
```

### File Location

Save to: `output/documents/{topic-slug}/`
- HTML: `{topic-slug}.html`
- PDF: `{topic-slug}.pdf`

**RULE:** Always use the existing `output/documents/` directory. Never create a new carousel folder elsewhere.

---

## Phase 3: Generate PDF

Run Chrome headless to convert HTML to PDF:

**Windows:**
```bash
"/c/Program Files/Google/Chrome/Application/chrome.exe" \
  --headless --disable-gpu \
  --print-to-pdf="{output-path}.pdf" \
  --no-margins --print-to-pdf-no-header \
  "{input-path}.html"
```

**Mac:**
```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --headless --disable-gpu \
  --print-to-pdf="{output-path}.pdf" \
  --no-margins --print-to-pdf-no-header \
  "{input-path}.html"
```

**Linux:**
```bash
google-chrome \
  --headless --disable-gpu \
  --print-to-pdf="{output-path}.pdf" \
  --no-margins --print-to-pdf-no-header \
  "{input-path}.html"
```

**RULE:** Always use `--no-margins` and `--print-to-pdf-no-header` AND ensure `@page { margin: 0 !important; }` is in the HTML CSS. All three are needed to fully suppress Chrome's automatic date/title/URL stamps.

After generation:
1. Open the PDF for user review
2. Report file size and page count
3. Ask if any adjustments are needed

---

## Phase 4: Extract Page Images + Slideshow

### Convert PDF to PNG images

```python
import fitz, os
doc = fitz.open("path/to/document.pdf")
for i, page in enumerate(doc):
    pix = page.get_pixmap(matrix=fitz.Matrix(3, 3))  # 3x zoom for high-res
    pix.save(f"pages/page-{i+1}.png")
```

Save to: `output/documents/{topic-slug}/pages/`

### Create slideshow video (optional, for YouTube/video platforms)

```bash
ffmpeg -y -framerate 1/7 -i "pages/page-%d.png" \
  -vf "scale=1080:1080:force_original_aspect_ratio=decrease,pad=1080:1080:(ow-iw)/2:(oh-ih)/2:white" \
  -c:v libx264 -pix_fmt yuv420p -r 30 -crf 18 -preset medium \
  "{topic-slug}-slideshow.mp4"
```

- 7 seconds per page = good pacing for reading
- 1080x1080 square format works across platforms
- White padding preserves the document aesthetic

---

## Phase 5: Content Package (MANDATORY BEFORE POSTING)

**RULE: NEVER post without showing the full content package to the user and receiving explicit approval.** No exceptions.

### Create platform-tailored captions

Each platform MUST have unique wording. Same core message, different phrasing. Never copy-paste.

---

### Instagram Carousel

**Caption -- KEEP IT MINIMAL (1-3 sentences):**

```
[One punchy sentence -- the hook. Must land before the "...more" truncation at ~125 chars.]
[Optional second sentence with the key insight or transformation.]
[One CTA: "Save this" / "Send to someone who needs it" / "Link in bio"]

#hashtag1 #hashtag2 #hashtag3
```

**Rules:**
- 3-5 hashtags max (Instagram's own recommendation). Quality > quantity
- Save CTAs perform best -- carousels generate 1.4x more saves than single images
- DM shares are weighted 3-5x higher than likes by the algorithm -- "Send this to someone" is high-value
- First comment: engagement prompt ("Which page hit hardest?") or move hashtags here
- Tone: conversational + authoritative. "I" not "we". Direct, human, not corporate
- Do NOT repeat what's on the slides -- the caption adds context the slides don't show

**What NOT to do:**
- No long paragraphs or mini-blog essays
- No stacking multiple CTAs (pick ONE action)
- No generic hashtags (#business #success #motivation)
- No link dumps in the caption

---

### TikTok Photo Carousel

**Caption -- SHORT AND PUNCHY (1-2 sentences):**

```
[Bold claim or swipe prompt that stops the scroll.]
[Key takeaway in one sentence.] Link in bio.

#hashtag1 #hashtag2 #hashtag3
```

**Rules:**
- Max 5 hashtags (TikTok enforces this limit)
- Framework: 1 trending + 1 niche + 1 branded + 2 content-specific
- TikTok carousels get 81% higher engagement than videos and 3x more saves
- Tone: casual, like texting a friend. Authentic > polished
- "POV:" and "Who else..." framing performs well

**What NOT to do:**
- No corporate language
- No emoji walls
- No "follow for more" as the only content

---

### Threads

**Caption -- CONVERSATIONAL (under 500 chars):**

```
[Short punchy hook -- question or bold statement.]
[1-2 sentences of context for the carousel.]
[Natural conversation starter as CTA -- NOT engagement bait.]

[One topic tag]
```

**Rules:**
- Exactly ONE topic tag per post (Threads design)
- Images outperform text by 60% on Threads; carousels drive 247% more interactions
- Engagement bait is actively penalized ("Like if you agree" gets downranked)
- Tone: spontaneous, unfiltered, personal. Write as "I"
- First 30 minutes of engagement are critical -- reply to comments fast

**What NOT to do:**
- No hashtag spam (you only get one topic tag anyway)
- No copy-paste from Instagram
- No "engagement bait" CTAs

---

### YouTube Community Post (Slideshow Video)

**Description:**

```
[Hook sentence -- restate the problem or key stat from the carousel.]

[2-3 sentences expanding on the value. Reference specific points.]

[CTA -- lead magnet link or "watch the full video" link]

#Hashtag1 #Hashtag2 #Hashtag3
```

**Title:** 3 options for user to pick. Max 70 chars. Front-load the keyword
**Tags:** 10-15 researched keywords across primary, secondary, long-tail
**First comment:** Engagement question related to the carousel topic

---

### Content Variation Quick Reference

| Aspect | Instagram | TikTok | Threads | YouTube |
|--------|-----------|--------|---------|---------|
| **Length** | 1-3 sentences | 1-2 sentences | Under 500 chars | 4-6 sentences + links |
| **Hashtags** | 3-5 | 3-5 (max 5) | 1 topic tag | 3-5 + tags array |
| **Tone** | Conversational + authoritative | Casual, like texting | Spontaneous, personal | Professional, detailed |
| **Best CTA** | Save / Send to someone | Comment / Save | Question / Discussion | Subscribe / Comment |
| **First comment** | Engagement prompt | Not required | Reply to spark thread | Engagement question |
| **Links** | Link in bio only | Link in bio only | Inline links OK | Full links in description |

---

### Present Package to User

Show all platforms in this format:

```
--- INSTAGRAM ---
Caption:
[full caption with hashtags]
First comment: [text]

--- TIKTOK ---
Caption:
[full caption with hashtags]

--- THREADS ---
Caption:
[full caption with topic tag]

--- YOUTUBE ---
Title options:
  1. [option 1]
  2. [option 2]
  3. [option 3]
Description: [full description]
Tags: [tag1, tag2, ...]
First comment: [text]
```

**Ask for:**
1. Any edits to captions
2. Title selection (YouTube)
3. Explicit approval to post -- "go ahead" / "publish" / "yes"

**NEVER post without explicit user approval.**

---

## Phase 6: Publish (Optional)

### Upload media to Late storage

```bash
# Upload each page image
for i in 1 2 3 4 5 6 7 8; do
  PRESIGN=$(curl -s -X POST "https://getlate.dev/api/v1/media/presign" \
    -H "Authorization: Bearer $LATE_API_KEY" \
    -H "Content-Type: application/json" \
    -d "{\"filename\": \"carousel-page-${i}.png\", \"contentType\": \"image/png\"}")
  UPLOAD_URL=$(echo "$PRESIGN" | python -c "import sys,json; print(json.load(sys.stdin)['uploadUrl'])")
  PUBLIC_URL=$(echo "$PRESIGN" | python -c "import sys,json; print(json.load(sys.stdin)['publicUrl'])")
  curl -s -X PUT "$UPLOAD_URL" -H "Content-Type: image/png" --data-binary "@pages/page-${i}.png"
done
```

**RULE:** Domain is `getlate.dev` -- NOT `api.getlate.dev`.

### Post via single multi-platform REST request

**RULE:** Use ONE API call with `platforms[]` array and `customContent` per platform. NEVER fire separate calls per platform.

```bash
curl -s -X POST "https://getlate.dev/api/v1/posts" \
  -H "Authorization: Bearer $LATE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "[FALLBACK_CONTENT]",
    "mediaItems": [
      {"type": "image", "url": "[PAGE_1_URL]"},
      {"type": "image", "url": "[PAGE_2_URL]"}
    ],
    "platforms": [
      {
        "platform": "instagram",
        "accountId": "[IG_ACCOUNT_ID]",
        "customContent": "[IG_CAPTION]",
        "platformSpecificData": {
          "shareToFeed": true,
          "firstComment": "[IG_FIRST_COMMENT]"
        }
      },
      {
        "platform": "tiktok",
        "accountId": "[TT_ACCOUNT_ID]",
        "customContent": "[TT_CAPTION]",
        "platformSpecificData": {
          "privacy_level": "PUBLIC_TO_EVERYONE",
          "allow_comment": true,
          "content_preview_confirmed": true,
          "express_consent_given": true
        }
      },
      {
        "platform": "threads",
        "accountId": "[THREADS_ACCOUNT_ID]",
        "customContent": "[THREADS_CAPTION]"
      }
    ],
    "publishNow": true
  }'
```

For YouTube (video post -- separate call since it uses video not images):

```bash
curl -s -X POST "https://getlate.dev/api/v1/posts" \
  -H "Authorization: Bearer $LATE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "[YT_DESCRIPTION]",
    "mediaItems": [{"url": "[SLIDESHOW_VIDEO_URL]"}],
    "platforms": [{
      "platform": "youtube",
      "accountId": "[YT_ACCOUNT_ID]",
      "platformSpecificData": {
        "title": "[SELECTED_TITLE]",
        "visibility": "public",
        "tags": ["tag1", "tag2"],
        "firstComment": "[YT_FIRST_COMMENT]",
        "categoryId": "27"
      }
    }],
    "publishNow": true
  }'
```

### Account IDs

To find your account IDs, list your connected accounts:

```bash
curl -s "https://getlate.dev/api/v1/accounts" \
  -H "Authorization: Bearer $LATE_API_KEY"
```

This returns all connected social accounts with their IDs and platforms.

### Verify posts

After posting, check the response for success/failure per platform. If any failed:
1. Check if it actually posted despite the error
2. Only retry if confirmed NOT published
3. Maximum 3 retries per platform

Report to user: Post IDs, platform status, any failures.

---

## Evolution Check

1. Did this invocation reveal a repeated pattern? Note it for future improvement
2. Was this skill invoked alongside another skill? Note the pairing
3. Did the user request a design element not in the template? Add to components list
4. Is the design system diverging from what user approved? Flag for update

---

## Non-Negotiable Rules

1. **NEVER post without showing content package and getting explicit approval**
2. **NO repeating headers or footers** -- logo on cover and CTA page only
3. **`@page { margin: 0 !important; }` in CSS** -- prevents Chrome artifacts
4. **NO emojis in the document** -- use inline SVG icons only
5. **Google Docs aesthetic** -- clean, restrained, professional
6. **Platform-tailored captions** -- never copy-paste the same text across platforms
7. **Single multi-platform API call** -- never fire separate calls per platform
8. **Domain is `getlate.dev`** -- NOT `api.getlate.dev`

---

**Version:** 1.0
**Tools:** Chrome headless (PDF), PyMuPDF (page extraction), FFmpeg (slideshow), Late API (publishing)
