# Setup Guide

A step-by-step guide to get Claude Canvas running on your machine.

---

## Step 1: Install VS Code + Claude Code Extension

### Install VS Code

1. Download VS Code from [code.visualstudio.com](https://code.visualstudio.com/)
2. Run the installer for your operating system
3. Open VS Code after installation

### Install Claude Code Extension

1. Open VS Code
2. Go to the Extensions panel (Ctrl+Shift+X on Windows, Cmd+Shift+X on Mac)
3. Search for **"Claude Code"** by Anthropic
4. Click **Install**
5. After installation, you'll see the Claude Code icon in your sidebar

### Create an Anthropic Account

If you don't already have one:
1. Go to [console.anthropic.com](https://console.anthropic.com/)
2. Sign up for an account
3. Add a payment method (Claude Code requires API credits)
4. The extension will prompt you to sign in when you first use it

---

## Step 2: Clone This Repo

Open a terminal and run:

```bash
git clone https://github.com/Trejon-888/claude-canvas.git
cd claude-canvas
```

Then open the folder in VS Code:

```bash
code .
```

Or use File > Open Folder in VS Code.

---

## Step 3: Get Your KIE.ai API Key

KIE.ai provides the AI image generation that powers the Thumbnail Creator and Carousel Generator skills.

### Create Account

1. Go to [kie.ai](https://kie.ai)
2. Click **Sign Up** and create an account
3. Verify your email

### Get API Key

1. Log in to your KIE.ai dashboard
2. Navigate to **API Keys** or **Settings > API**
3. Click **Create New API Key**
4. Copy the key -- you'll need it in Step 5

### Pricing

KIE.ai charges per image generated:
- **2K resolution:** ~$0.09 per image
- **4K resolution:** ~$0.12 per image

A typical thumbnail session costs $0.12-0.36. A 6-slide carousel costs ~$0.54-0.72.

---

## Step 4: Get Your Late API Key

Late handles media storage (uploading images for AI processing) and social media posting across platforms.

### Create Account

1. Go to [getlate.dev](https://getlate.dev)
2. Sign up for an account
3. Connect your social media accounts (Instagram, TikTok, LinkedIn, YouTube, Threads, etc.)

### Get API Key

1. Log in to your Late dashboard
2. Go to **Settings > API Keys**
3. Click **Create API Key**
4. Copy the key -- you'll need it in Step 5

### Connect Social Accounts (Optional)

If you want to use the Document Carousel's publishing feature:
1. In Late's dashboard, go to **Accounts**
2. Click **Connect** for each platform you want to post to
3. Follow the OAuth flow for each platform

---

## Step 5: Configure Your System

Open Claude Code in VS Code (click the Claude icon in the sidebar) and type:

> "I want to set up my system"

Claude will guide you through:

1. **Setting your API keys** as environment variables
2. **Creating your creator profile** (name, face reference photo, brand color)
3. **Testing the KIE API connection**
4. **Testing the Late API connection**

### Manual API Key Setup (Alternative)

If you prefer to set keys manually:

**Windows (PowerShell profile):**
```powershell
# Add to your PowerShell profile ($PROFILE)
$env:KIE_API_KEY = "your-kie-api-key-here"
$env:LATE_API_KEY = "your-late-api-key-here"
```

**Mac/Linux (bash/zsh profile):**
```bash
# Add to ~/.bashrc or ~/.zshrc
export KIE_API_KEY="your-kie-api-key-here"
export LATE_API_KEY="your-late-api-key-here"
```

**Using a .env file:**
Create a `.env` file in the project root:
```
KIE_API_KEY=your-kie-api-key-here
LATE_API_KEY=your-late-api-key-here
```

> **Important:** Never commit your `.env` file to git. Add it to `.gitignore`.

### Set Up Your Face Reference (for Thumbnails)

1. Find a clear, well-lit photo of your face (front-facing, good resolution)
2. Create the assets folder:
   ```bash
   mkdir -p .claude/skills/thumbnail-creator/assets/your-name/
   ```
3. Copy your photo there and rename it to `face-reference.png`
4. Update the creator profile table in `.claude/skills/thumbnail-creator/SKILL.md`

---

## Step 6: Test Each Skill

### Test 1: Thumbnail Creator

Open Claude Code and say:

> "Create a thumbnail for a video about 'How to Automate Social Media with AI'"

Claude should:
1. Ask you clarifying questions about the style and mood
2. Present 3 thumbnail concepts
3. Let you pick one
4. Generate the thumbnail using KIE.ai
5. Save it to `images/thumbnails/`

### Test 2: Carousel Generator

> "Create a 6-slide carousel about '5 AI Tools Every Content Creator Needs'"

Claude should:
1. Ask about your brand colors and logo
2. Generate prompts for each slide
3. Show prompts for approval
4. Generate images using KIE.ai
5. Save them to `images/carousel/`

### Test 3: Document Carousel

> "Create a carousel document about 'The Complete Guide to AI Content Creation'"

Claude should:
1. Ask about the topic and audience
2. Present an outline for approval
3. Write the full HTML document
4. Generate a PDF using Chrome headless
5. Open the PDF for your review
6. Optionally extract page images and create a content package

---

## Troubleshooting

### "KIE API key not found"

Make sure your environment variable is set and your terminal/VS Code has been restarted after setting it.

```bash
# Check if the key is set
echo $KIE_API_KEY
```

### "Chrome not found" (Document Carousel)

The Document Carousel skill needs Google Chrome for PDF generation. Install it from [google.com/chrome](https://www.google.com/chrome/).

The skill looks for Chrome at these paths:
- **Windows:** `C:\Program Files\Google\Chrome\Application\chrome.exe`
- **Mac:** `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`
- **Linux:** `google-chrome` (in PATH)

### "pymupdf not installed" (Page Extraction)

Install the Python package for PDF page extraction:

```bash
pip install pymupdf
```

### "ffmpeg not found" (Slideshow Video)

Install FFmpeg for creating slideshow videos from carousel pages:
- **Windows:** Download from [ffmpeg.org](https://ffmpeg.org/download.html) or use `winget install ffmpeg`
- **Mac:** `brew install ffmpeg`
- **Linux:** `sudo apt install ffmpeg` or equivalent

### Images look wrong or text is unreadable

This is a common AI image generation issue. Try:
1. Ask Claude to regenerate with stronger text legibility instructions
2. Reduce card rotation angles
3. Simplify the prompt (fewer elements = more consistent output)

### Late API errors

- Make sure the domain is `getlate.dev` (NOT `api.getlate.dev`)
- Check that your API key has the right permissions
- Verify your social accounts are connected in the Late dashboard

---

## Optional: Install Additional Tools

| Tool | What It's For | Install Command |
|------|--------------|-----------------|
| Python 3.10+ | PDF page extraction | [python.org](https://www.python.org/downloads/) |
| pymupdf | PDF processing library | `pip install pymupdf` |
| FFmpeg | Slideshow video creation | [ffmpeg.org](https://ffmpeg.org/download.html) |
| Google Chrome | PDF generation | [google.com/chrome](https://www.google.com/chrome/) |

---

## What's Next?

Once everything is set up, you can:

1. **Create thumbnails** for your YouTube videos with consistent branding
2. **Generate carousel images** for LinkedIn and Instagram posts
3. **Build educational guides** as document carousels and post them across platforms

Just open Claude Code and describe what you want to create. The skills will guide you through the process.

---

Need help? Join the [Skool community](https://www.skool.com/infinitx) or reach out on [LinkedIn](https://www.linkedin.com/in/enrique-marq-756191319/).
