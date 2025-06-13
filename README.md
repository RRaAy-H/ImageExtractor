# ImageExtractor

**ImageExtractor** is a Python-based pipeline for extracting, deduplicating, and captioning frames from videos. It automates video downloading, frame extraction, advanced image deduplication (pixel and semantic), OCR-based filtering, and generates descriptive captions for the resulting images.

## Features

- **Download videos** from URLs (supports platforms via `yt-dlp`).
- **Extract frames** at customizable intervals.
- **Multi-step deduplication:**
  - Pixel-based global deduplication.
  - Semantic (deep learning) deduplication (sequential and global) using CLIP.
  - OCR-based filtering for text-rich frames.
- **Caption generation** for deduplicated images using a customizable prompt.
- **Automatic clean-up** of intermediate files and directories.
- **Resumable**: Skips steps if output already exists.

## Installation

1. **Clone the repository:**
   ```sh
   git clone https://github.com/RRaAy-H/ImageExtractor.git
   cd ImageExtractor
   ```

2. **Install dependencies:**
   ```sh
   pip install -r requirements.txt
   ```
   *Additional dependencies may be required depending on your use-case (e.g., pytorch, easyocr, yt-dlp, openai).*

3. **(Optional) Install system dependencies:**
   - `ffmpeg` for frame extraction.
   - GPU drivers for accelerated inference.

## Usage

Run the pipeline from the command line:

```sh
python main.py --video-url <VIDEO_URL>
```
Or to process a local file:
```sh
python main.py --video-path <PATH_TO_VIDEO>
```

### Common Options

- `-o`, `--output-dir`: Directory for all outputs (default: current).
- `--extract-interval`: Seconds between each extracted frame (default: 8).
- `--pixel-threshold`: Hamming distance for pixel deduplication (default: 3).
- `--sequential-deep-threshold`: Cosine similarity for sequential deep deduplication (default: 0.80).
- `--global-deep-threshold`: Cosine similarity for global deep deduplication (default: 0.85).
- `--device`: Torch device (`cuda`, `mps`, `cpu`).
- `--caption-prompt`: Custom prompt for caption generation.
- `--cookies-browser`: Browser to use for cookies (default: chrome).
- `--secrets-path`: Path to API key secrets, if needed for captioning.
- `--ocr-lang`: OCR language (default: en).
- `--ocr-gpu`: Use GPU for OCR (default: True).
- `--min-words`: Minimum OCR word count to keep an image (default: 5).

### Example

```sh
python main.py --video-url "https://www.youtube.com/watch?v=example" -o ./outputs --extract-interval 5 --ocr-lang en
```

## Pipeline Overview

1. **Download** video (if URL provided).
2. **Extract frames** at specified intervals.
3. **Deduplicate**:
   - Remove pixel-level duplicates.
   - Remove semantically similar images (deep learning).
   - Remove images with insufficient text (OCR).
4. **Generate captions** for filtered images.
5. **Clean up** intermediate files.

## Output

- `<video_title>_Dedup_Images/`: Final deduplicated images.
- `<video_title>_caption.json`: JSON file mapping each image to its caption.
- `<video_title>_dedupe_log.json`: Log of deduplication steps.

## Advanced

- Supports resuming: skips steps if outputs exist.
- Customizable via Python API (`process_video()` in main.py).
- Modular: deduplication and captioning logic in separate modules.

## Requirements

- Python 3.7+
- See `requirements.txt` for Python packages.
- `ffmpeg` (system package).
- GPU recommended for faster deep deduplication and OCR.

## License

MIT License. See [LICENSE](LICENSE) for details.

## Acknowledgements

- [yt-dlp](https://github.com/yt-dlp/yt-dlp) for video downloading.
- OpenAI CLIP for semantic deduplication.
- EasyOCR for text extraction.

---

If you’d like, I can further tailor this README with author info, examples, or badges. Let me know!
