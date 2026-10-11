# YT-Transcript-Tool

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![OpenAI Whisper](https://img.shields.io/badge/OpenAI-Whisper-green.svg)](https://github.com/openai/whisper)
[![yt-dlp](https://img.shields.io/badge/yt--dlp-audio-red.svg)](https://github.com/yt-dlp/yt-dlp)

Automated pipeline in Jupyter Notebook (`Download + transcribe.ipynb`) to download audio streams from YouTube videos using **`yt-dlp`** + **`FFmpeg`** and generate accurate local transcriptions and subtitles using **OpenAI Whisper**.

---

## Features

- **Best-Audio Extraction**: Downloads the highest-quality audio stream from any YouTube URL via `yt-dlp` and converts it to `.opus` using `FFmpegExtractAudio`.
- **Automatic Filename Sanitization**: Normalizes Unicode characters and removes restricted filesystem characters (`<>:"/\|?*`) to create a clean output folder per video.
- **Local Whisper Transcription**: Invokes OpenAI's `whisper` CLI locally with configurable model size (`tiny`, `base`, `small`, `medium`, `large`) and target language (e.g., `es`, `en`).
- **Multi-Format Output**: Produces plain text (`.txt`), timestamped subtitles (`.srt`, `.vtt`), tab-separated segments (`.tsv`), and full metadata (`.json`) inside a dedicated folder named after the video title.

---

## Repository Structure

```text
YT-Transcript-Tool/
├── Download + transcribe.ipynb   # Main Jupyter Notebook pipeline
└── README.md                     # Documentation and setup guide
```

---

## Prerequisites

1. **Python 3.10+**
2. **FFmpeg** installed and available on your system `PATH`:
   - **Windows (Winget)**: `winget install Gyan.FFmpeg`
   - **macOS (Homebrew)**: `brew install ffmpeg`
   - **Linux (APT)**: `sudo apt install ffmpeg`

---

## Installation

```bash
# Clone the repository
git clone https://github.com/JoaquinFernandez96/YT-Transcript-Tool.git
cd YT-Transcript-Tool

# Create and activate a virtual environment
python -m venv venv
# Windows:
venv\Scripts\activate
# macOS / Linux:
source venv/bin/activate

# Install required packages
pip install yt-dlp openai-whisper jupyter
```

> **Optional (GPU Acceleration)**: To run Whisper on an NVIDIA GPU with CUDA, install PyTorch with CUDA support before running Whisper (see [PyTorch Get Started](https://pytorch.org/get-started/locally/)).

---

## Usage

1. Launch Jupyter Notebook or open [`Download + transcribe.ipynb`](Download%20+%20transcribe.ipynb) in VS Code / JupyterLab:
   ```bash
   jupyter notebook "Download + transcribe.ipynb"
   ```
2. Configure the parameters at the top of the notebook cell:
   ```python
   MODEL = "small"                                       # Options: "tiny", "base", "small", "medium", "large"
   LANGUAGE = "es"                                       # ISO language code (e.g., "es", "en", "pt")
   link = "https://www.youtube.com/watch?v=YOUR_VIDEO"   # Target YouTube URL
   ```
3. Run the cell. The script will:
   - Download and extract `<Video Title>.opus`
   - Create a sanitized directory `./<Sanitized Video Title>/`
   - Generate the Whisper transcription files (`.txt`, `.srt`, `.vtt`, `.tsv`, `.json`) inside that folder.

---

## Whisper Model Reference

| Model | Parameters | Approximate VRAM | Relative Speed | Best Use Case |
| :--- | :--- | :--- | :--- | :--- |
| `tiny` | 39 M | ~1 GB | ~10x | Quick drafts on CPU |
| `base` | 74 M | ~1 GB | ~7x | Fast everyday transcriptions |
| `small` | 244 M | ~2 GB | ~4x | **Default — balanced accuracy & speed (Spanish/English)** |
| `medium` | 769 M | ~5 GB | ~2x | High-accuracy lectures & technical terminology |
| `large` | 1550 M | ~10 GB | 1x | Maximum multilingual accuracy |
