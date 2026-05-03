> **REPLIT AGENT — START BUILDING IMMEDIATELY**
> Read this entire file, then start on Task 1 right now. Do not ask questions. Do not wait for input. Build everything in the task list, commit each completed task with prefix `replit: `, and push to main. When all tasks are done, run the project and verify it works.


# Infinite Caption Pipeline — Replit Agent Build Spec

Build from scratch. Commit with "replit: " prefix.

## Purpose
Takes video/audio files → transcribes via Whisper → generates styled captions/subtitles → formats for social (TikTok, Reels, YouTube Shorts). Also generates standalone social captions using Claude.

## Stack
- Python 3.11+, openai-whisper, anthropic, Pillow, moviepy, python-dotenv

## Tasks

### 1. transcriber.py
```python
def transcribe(media_path: str, model="base") -> list[dict]:
    """Returns list of {start, end, text} segments using local Whisper."""
```
Models: tiny (fastest), base (balanced), small, medium. Default: base.
No API key needed — runs locally via `openai-whisper` pip package.

### 2. subtitle_formatter.py
Convert transcript segments to subtitle formats:
- `.srt` — standard SubRip
- `.vtt` — WebVTT for web
- `tiktok` — hardcoded style: bold white text, black stroke, center-bottom, 2-3 words per line, max 2 lines
- `reels` — similar to tiktok but slightly different timing

### 3. caption_burner.py
Burn captions into video using moviepy + Pillow:
```python
def burn_captions(video_path: str, srt_path: str, style="tiktok") -> str:
    """Returns path to output video with burned-in captions."""
```

### 4. social_caption_writer.py
Uses Claude to generate social media captions (not subtitles):
```python
def write_caption(topic: str, platform="instagram|tiktok|twitter", style="hype|informative|funny", include_hashtags=True) -> str:
    """Returns a ready-to-post caption with hashtags."""
```
System: "You are a social media expert. Write a {platform} caption for: {topic}. Style: {style}. Max 150 words. Include {5-10} relevant hashtags."

### 5. batch_processor.py
Process all videos in a folder:
- Input: `inputs/` folder with .mp4/.mov files
- Output: `outputs/` with .srt + captioned video + social caption .txt

### 6. main.py CLI
```
python main.py transcribe --input video.mp4
python main.py subtitle --input video.mp4 --style tiktok
python main.py burn --input video.mp4 --style reels
python main.py caption --topic "NYC street art" --platform instagram
python main.py batch --input inputs/
```

### 7. requirements.txt
```
openai-whisper>=20231117
anthropic>=0.39.0
moviepy>=1.0.3
Pillow>=10.4.0
python-dotenv>=1.0.0
tqdm>=4.66.0
```

### 8. .env.example
```
ANTHROPIC_API_KEY=
```