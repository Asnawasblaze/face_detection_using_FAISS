# Face Recognition System

[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![InsightFace](https://img.shields.io/badge/InsightFace-0.7.3-orange.svg)](https://github.com/deepinsight/insightface)
[![FAISS](https://img.shields.io/badge/FAISS-1.8.0-red.svg)](https://github.com/facebookresearch/faiss)

Real-time face recognition system using **InsightFace (ArcFace)** for face encoding and **FAISS** for efficient similarity search.

## Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Configuration](#configuration)
- [How It Works](#how-it-works)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)

## Features

- **Real-time Face Detection** — SCRFD detector for fast, accurate face detection
- **Face Recognition** — ArcFace 512-dimensional embeddings for robust identification
- **Efficient Search** — FAISS indexing for millisecond-level face matching
- **Face Tracking** — KCF tracker for smooth, continuous tracking across frames
- **Easy Database Management** — Add new faces by simply adding images to folders
- **Cross-platform** — Works on Windows, Linux, and macOS

## Prerequisites

- Python 3.11 or higher
- Webcam (for live recognition)
- 4GB+ RAM recommended

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/face-recognition-faiss.git
cd face-recognition-faiss
```

### 2. Create Virtual Environment

**Windows:**
```bash
python -m venv .venv
.\.venv\Scripts\activate
```

**Linux/macOS:**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

> **Note:** InsightFace models (~341 MB) will auto-download on first run.

## Quick Start

```bash
# 1. Build face embeddings from known faces
python src/recognition/build_embeddings.py

# 2. Build FAISS index
python src/indexing/build_faiss_index.py

# 3. Run live recognition
python src/recognition/recognize_camera.py
```

Press `Q` or `ESC` to exit.

## Usage

### Adding New Faces

1. Create a folder with the person's name in `data/known_faces/`:
   ```
   data/known_faces/
   └── John Doe/
       ├── photo1.jpg
       ├── photo2.jpg
       └── photo3.jpg
   ```

2. Rebuild the database:
   ```bash
   python src/recognition/build_embeddings.py
   python src/indexing/build_faiss_index.py
   ```

3. Run recognition:
   ```bash
   python src/recognition/recognize_camera.py
   ```

### Image Guidelines

| Requirement | Recommendation |
|-------------|----------------|
| Quantity | 3-10 images per person |
| Format | JPG, JPEG, PNG |
| Quality | Clear, well-lit photos |
| Variety | Different angles and expressions |

## Project Structure

```
├── data/
│   ├── known_faces/          # Face images organized by person
│   └── test_images/          # Test images
├── embeddings/
│   ├── face_embeddings.pkl   # Generated face embeddings
│   ├── faiss.index           # FAISS similarity index
│   └── id_map.pkl            # ID to name mapping
├── src/
│   ├── indexing/
│   │   └── build_faiss_index.py
│   └── recognition/
│       ├── build_embeddings.py
│       ├── recognize_camera.py
│       └── test_arcface.py
├── requirements.txt
└── README.md
```

## Configuration

Edit `src/recognition/recognize_camera.py` to customize:

```python
THRESHOLD = 0.45              # Recognition threshold (0.0-1.0)
DETECT_EVERY_N_FRAMES = 30    # Detection frequency
MAX_FACES = 5                 # Max simultaneous faces
VOTE_WINDOW = 5               # Frames for identity confirmation
```

## How It Works

```
Camera → Face Detection (SCRFD) → Alignment → Embedding (ArcFace)
                                                    ↓
Display ← Tracking (KCF) ← Decision ← FAISS Search ←
```

| Step | Description |
|------|-------------|
| Detection | SCRFD locates faces in each frame |
| Alignment | 3D landmarks normalize face orientation |
| Embedding | ArcFace generates 512-D feature vector |
| Search | FAISS finds nearest match in database |
| Decision | Similarity score compared to threshold |
| Tracking | KCF maintains identity across frames |

## Troubleshooting

| Issue | Solution |
|-------|----------|
| No faces detected | Check lighting, camera permissions, and positioning |
| Poor accuracy | Add more images (5-10), ensure clear photos, rebuild index |
| Camera not found | Verify webcam connection and driver installation |
| Slow performance | Reduce resolution or increase `DETECT_EVERY_N_FRAMES` |
| ONNX warnings | Can be safely ignored, system works fine |

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

## Acknowledgments

- [InsightFace](https://github.com/deepinsight/insightface) — Face analysis library
- [FAISS](https://github.com/facebookresearch/faiss) — Similarity search by Meta AI
- [ArcFace Paper](https://arxiv.org/abs/1801.07698) — Additive Angular Margin Loss
- [SCRFD Paper](https://arxiv.org/abs/2105.04714) — Sample and Computation Redistribution