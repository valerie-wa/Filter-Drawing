# Jarvis

Real-time hand tracking + face filtering built with MediaPipe, OpenCV, and Sobel edge detection.

## What it does

- Detects hand landmarks in real time via webcam or video file
- Tracks index fingertip position with Sobel edge refinement
- Maps drawn image onto face using Mediapipe polygons
- Logs all recognized commands to `app/data/logs/commands.txt`

## Stack

- [OpenCV](https://opencv.org/) — camera pipeline + image processing
- [MediaPipe](https://mediapipe.dev/) — hand landmark detection
- NumPy — array ops

## Setup

**macOS** (run once):
```bash
./install.sh
```

**Other platforms:**
```bash
pip install -r requirements.txt
```

## Run

```bash
python -m app.core.video_pipeline
```

Press `ESC` to quit.

## Configuration

All settings are in [`app/utils/config.py`](app/utils/config.py):

| Setting | Default | Description |
|---|---|---|
| `CAMERA_INDEX` | `0` | Webcam index |
| `USE_WEBCAM` | `True` | Set `False` to use a video file |
| `COMMAND_COOLDOWN` | `2` | Seconds between commands |
| `EDGE_REGION_SIZE` | `80` | Sobel crop box size around fingertip |

## Using a video file instead of webcam

1. Drop your `.mp4` into `app/data/recordings/videos/`
2. In `config.py` set `USE_WEBCAM = False` and uncomment `VIDEO_PATH`
3. In `video_pipeline.py` swap the filename on the source line

## Voice commands

| Say | Output |
|---|---|
| "open board" / "open canvas" | `OPEN_BOARD` |
| "clear" / "erase" | `CLEAR` |
| "save" | `SAVE` |
| "switch" / "tool" | `SWITCH_TOOL` |
| "stop" | `STOP` |

Add your own in [`app/core/command_parser.py`](app/core/command_parser.py).

## Project structure

```
app/
  assets/
    toolbar.PNG — toolbar overlay
  core/
    face_detecion.py    — Mediapipe
    filter_video_pipeline.py — video pipeline for face filtering
    video_pipeline.py   — main loop
    hand_detection.py   — MediaPipe + Sobel
    command_parser.py   — text → command
    voice_input.py      — mic → text (threaded)
  processing/
    drawing.py          — ui and 2d drawing
  utils/
    config.py           — all settings
    helpers.py          — FPS, smoothing, logger
  data/
    recordings/
      videos/           — drop test videos in here
      images/           — drop test frames in here
    logs/
      commands.txt      — recognized commands logged here
```
