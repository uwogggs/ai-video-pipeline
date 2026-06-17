# 🎬 AI Video Pipeline

[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![FFmpeg](https://img.shields.io/badge/Video-FFmpeg-007808?logo=ffmpeg&logoColor=white)](https://ffmpeg.org/)
[![Google AI](https://img.shields.io/badge/AI-Google%20API-4285F4?logo=google&logoColor=white)](https://ai.google.dev/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

End-to-end automated video production pipeline leveraging AI for content generation, editing, and publishing. From script to final render — minimal manual intervention, maximum output quality.

## Architecture Overview

```
┌──────────────────────────────────────────────────────────┐
│                  AI Video Pipeline                        │
│                                                           │
│  ┌───────────┐    ┌────────────┐    ┌─────────────────┐  │
│  │  Content   │    │   Script   │    │   Voice/TTS     │  │
│  │  Planner   │───▶│ Generator  │───▶│   Synthesis     │  │
│  │  (Google)  │    │  (Google)  │    │   (Google TTS)  │  │
│  └───────────┘    └────────────┘    └────────┬────────┘  │
│                                              │            │
│  ┌───────────────────────────────────────────▼────────┐  │
│  │              Media Assembly Engine                  │  │
│  │                                                     │  │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐           │  │
│  │  │  Image   │ │  Video   │ │  Audio   │           │  │
│  │  │  Fetch   │ │  Clips   │ │  Mix     │           │  │
│  │  └────┬─────┘ └────┬─────┘ └────┬─────┘           │  │
│  │       └─────────────┼───────────┘                   │  │
│  │                     │                               │  │
│  │              ┌──────▼──────┐                        │  │
│  │              │   FFmpeg    │                        │  │
│  │              │   Render    │                        │  │
│  │              └──────┬──────┘                        │  │
│  └─────────────────────┼──────────────────────────────┘  │
│                        │                                  │
│  ┌─────────────────────▼──────────────────────────────┐  │
│  │   Output: MP4 / MP3 / SRT Subtitles                │  │
│  └────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────┘
```

## Features

- **AI Script Generation** — Uses Google Gemini to generate video scripts from topic prompts
- **Text-to-Speech** — Google Cloud TTS for natural voiceovers
- **Automated Editing** — FFmpeg-based assembly of video clips, images, and audio
- **Subtitle Generation** — Auto-generated SRT subtitles synced to audio timing
- **Batch Processing** — Queue multiple videos for overnight rendering
- **Template System** — Reusable video templates for consistent branding
- **Rich CLI** — Beautiful terminal output with progress bars via Rich library
- **Environment Isolation** — `.env`-based configuration for API keys and paths

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Language | Python 3.11 |
| Video Processing | FFmpeg + moviepy |
| AI Content | Google Gemini API |
| Text-to-Speech | Google Cloud TTS |
| HTTP Client | httpx |
| CLI Output | Rich |
| Config | python-dotenv |

## Setup & Installation

### Prerequisites
- Python 3.11+
- [FFmpeg](https://ffmpeg.org/download.html) installed at `C:\tools\ffmpeg`
- Google Cloud API key with Gemini and TTS enabled

### Quick Start

```bash
# Clone the repository
git clone https://github.com/Uwogggs/ai-video-pipeline.git
cd ai-video-pipeline

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env
# Edit .env with your API keys and paths

# Generate a video from a topic
python main.py generate --topic "5 Tips for Home Lab Networking" --style educational
```

### Environment Variables

```env
GOOGLE_API_KEY=your_google_api_key
FFMPEG_PATH=C:\tools\ffmpeg\bin\ffmpeg.exe
OUTPUT_DIR=C:\Users\jonat\AI-Video-Hustle\output
TEMP_DIR=C:\Users\jonat\AI-Video-Hustle\temp
VOICE=en-US-Neural2-D
VIDEO_RESOLUTION=1920x1080
```

## Usage

### Generate a Video

```bash
# Single video from topic
python main.py generate --topic "Windows Server DNS Setup Guide" --duration 5

# Batch from topics file
python main.py batch --file topics.txt --style tutorial --parallel 2

# Custom template
python main.py generate --topic "NAS Backup Strategies" \
  --template tech-tutorial \
  --voice en-US-Neural2-F \
  --resolution 1080p
```

### Rich CLI Output

```
╭─────────────────────────────────────────╮
│  🎬 AI Video Pipeline v1.0              │
╰─────────────────────────────────────────╯

  📝 Generating script...           ✓ done (3.2s)
  🎤 Synthesizing voiceover...      ✓ done (5.1s)
  🖼️  Fetching media assets...       ✓ done (2.8s)
  ✂️  Editing video segments...      ✓ done (12.4s)
  📄 Generating subtitles...         ✓ done (1.1s)
  🎞️  Rendering final output...      ✓ done (28.7s)

  ✅ Output: output/home_lab_networking.mp4 (1080p, 5:23)
```

### Programmatic Usage

```python
from pipeline import VideoPipeline

pipeline = VideoPipeline(
    api_key="your-key",
    ffmpeg_path=r"C:\tools\ffmpeg\bin\ffmpeg.exe"
)

# Generate video
result = pipeline.generate(
    topic="Networking Fundamentals for IT Admins",
    style="educational",
    duration_minutes=5,
    voice="en-US-Neural2-D"
)

print(f"Video saved to: {result.output_path}")
print(f"Duration: {result.duration}")
print(f"Subtitles: {result.subtitle_path}")
```

## Project Structure

```
ai-video-pipeline/
├── main.py                  # CLI entry point
├── pipeline/
│   ├── __init__.py
│   ├── generator.py         # Script generation (Google AI)
│   ├── tts.py               # Text-to-speech synthesis
│   ├── editor.py            # FFmpeg video assembly
│   ├── subtitles.py         # SRT subtitle generation
│   └── templates.py         # Video template system
├── config/
│   ├── settings.py          # Configuration management
│   └── templates/           # Video template definitions
├── assets/
│   ├── music/               # Background music tracks
│   ├── images/              # Stock images and overlays
│   └── fonts/               # Subtitle fonts
├── output/                  # Rendered videos (gitignored)
├── .env.example
├── requirements.txt
└── README.md
```

## License

MIT License — see [LICENSE](LICENSE) for details.
