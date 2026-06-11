# 🎬 Prompt2Tube

> **Type a topic. Get a YouTube-ready video.**

Prompt2Tube is a fully automated AI video generation pipeline that takes any topic and produces a complete YouTube-ready video — with narration, images, video clips, and final assembly.

---

## 🚀 What It Does

```
Topic → Research → Story → Script → Audio → Images → Video → Final MP4
```

You type a topic. The pipeline:
1. Researches it using Gemini
2. Generates 5 story concepts and lets you pick one
3. Writes a full production script
4. Plans and generates narration audio (Cartesia TTS)
5. Generates cinematic images (KIE.ai Flux 2)
6. Engineers video prompts and generates clips (KIE.ai Veo 3)
7. Assembles everything into a final MP4 using FFmpeg

---

## 🧠 Tech Stack

| Step | Tool | Model |
|------|------|-------|
| Research & Planning | Google Gemini | gemini-2.5-flash-lite |
| Audio (TTS) | Cartesia Sonic | sonic-2 |
| Image Generation | KIE.ai | Flux 2 Pro |
| Video Generation | KIE.ai | Veo 3 (image-to-video) |
| Final Assembly | FFmpeg | — |
| UI | Gradio | — |

---

## 🏗️ Architecture

```
app.py          ← Gradio UI (sidebar, 10 steps)
pipeline.py     ← Core logic (calls Gemini + scripts)
config.py       ← API keys, paths, prompt file mapping

prompts/
├── 1_research.md
├── 2_story_generator.md
├── 3_image_prompt_generator.md
├── 4_video_prompt_engineering.md
└── 5_voice_prompt_engineer.md

script/
├── generate_asset.py       ← KIE.ai Flux 2 image gen
├── generate_video.py       ← KIE.ai Veo 3 video gen
├── generate_audio.py       ← Cartesia TTS audio gen
├── create_composite.py     ← Layer character on background
├── create_split_screen.py  ← Split screen composites
└── assemble_video.py       ← FFmpeg final assembly

projects/
└── your_topic/
    ├── assets/composites/  ← Generated images
    ├── audio/              ← Narration WAV files
    ├── videos/             ← Generated video clips
    └── final/              ← Final assembled MP4
```

---

## ⚙️ Setup

### 1. Clone the repo
```bash
git clone https://github.com/jill-05/Prompt2Tube.git
cd Prompt2Tube
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Install FFmpeg
```bash
# macOS
brew install ffmpeg

# Ubuntu/Debian
sudo apt install ffmpeg

# Windows
winget install ffmpeg
```

### 4. Set up API keys
```bash
cp .env.example .env
```

Edit `.env` and fill in your keys:
```env
GEMINI_API_KEY=your_gemini_key
KIE_API_KEY=your_kie_key
CARTESIA_API_KEY=your_cartesia_key
```

### 5. Run the app
```bash
python app.py
```

Open `http://localhost:8080` in your browser.

---

## 🔑 API Keys

| Service | Get Key | Free Tier |
|---------|---------|-----------|
| Google Gemini | [aistudio.google.com](https://aistudio.google.com) | ✅ Free |
| KIE.ai | [kie.ai](https://kie.ai) | 💰 $5 for 1000 credits |
| Cartesia TTS | [cartesia.ai](https://cartesia.ai) | ✅ Free credits on signup |

### Cost Per Image and Video
```
Images → Flux 2 Pro    5 credits/image × 10 scenes ≈ $0.25
Video  → Veo 3 Lite   60 credits/clip  × 10 scenes ≈ $3.00
Audio  → Cartesia      free credits
──────────────────────────────────────────────────────────
Total per video ≈ $3.25
```

---

## 📋 Pipeline Steps

| Step | What It Does |
|------|-------------|
| 1 · Research | Deep research profile on your topic |
| 2 · Story Concepts | 5 story options to choose from |
| 3 · Script | Full scene-by-scene production script |
| 4 · Asset Planning | JSON manifest with Flux image prompts |
| 5 · Audio Planning | Narration script timed per scene |
| 6 · Audio Generation | Cartesia TTS — one WAV per scene |
| 7 · Image Generation | Flux 2 — one composite image per scene |
| 8 · Video Prompts | Veo 3 motion prompts per scene |
| 9 · Video Generation | Veo 3 — animated clip per scene |
| 10 · Render | FFmpeg — merge audio + video → final MP4 |

> **Audio is generated before video** so video duration matches audio length exactly.

---

## 🔄 Audio/Video Sync

```
Cartesia generates narration → measure actual duration
Video generated at exact audio duration (max 8s per clip)
Silent scenes   → keep Veo 3 ambient audio as-is
Narrated scenes → replace Veo 3 audio with Cartesia narration
FFmpeg concatenates all clips → final.mp4
```

---

## 🤝 Contributing

Contributions are welcome!

- Report issues or bugs
- Suggest new features or improvements
- Share your creations and results
- Improve documentation
- Submit pull requests

Thank you for helping make this project better!
