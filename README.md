<!-- # Portrait Studio Pipeline (Simple, practical)

## What this pipeline does
Given a raw portrait image, it runs:
1. Deblur & denoise (lightweight)
2. Face detection and face restoration (GFPGAN)
3. Background segmentation (rembg / U^2-Net)
4. Background bokeh blur + subtle color grading + sharpening
5. Identity check preserved (we avoid heavy alterations)

## Setup
1. Create a Python 3.8+ virtualenv
2. Install requirements:
   python -m pip install -r requirements.txt

3. (Optional) If you have GPU and want faster inference, install CUDA-compatible `torch` manually:
   See https://pytorch.org/get-started/locally/

## Run
Put your test image(s) into `examples/`. Then:
   python main.py --input examples/input.jpg --output examples/output.jpg

## Files
- main.py: CLI entry point
- pipeline/: core processing modules
- utils.py: helper image utilities

## Notes
- GFPGAN and rembg will attempt to download required pretrained weights automatically on first run.
- For heavy motion blur, full recovery is not always possible; we apply conservative deblur to avoid hallucination. -->


# 📸 StudioLens: Raw to Studio Portrait Pipeline

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Computer Vision](https://img.shields.io/badge/CV-GFPGAN%20|%20rembg-green.svg)]()

**StudioLens** is a modular computer vision pipeline designed to transform raw, uncontrolled human portraits into professional studio-quality images. It addresses common mobile photography issues such as motion blur, low light, cluttered backgrounds, and uneven contrast while maintaining 100% facial identity and natural skin textures.

---

## 🚀 The Pipeline Architecture

The codebase follows a structured, sequential enhancement process:

1.  **Preprocessing:** Global resizing (for speed and stability) and `fastNlMeans` denoising to clean up sensor grain.
2.  **Luminance Correction:** Adaptive Contrast Enhancement (CLAHE) in the LAB color space to fix uneven lighting without distorting natural skin tones.
3.  **AI Facial Restoration:** 
    *   **Detection:** MTCNN identifies facial landmarks.
    *   **Enhancement:** GFPGAN v1.4 restores clarity to eyes, teeth, and skin features.
    *   **Blending:** Controlled alpha-blending (adjustable via CLI) to prevent a "waxy" AI look and preserve natural skin pores.
4.  **Subject Isolation:** Background removal using **U²-Net** (via `rembg`).
5.  **Studio Bokeh Engine:** 
    *   Gaussian blur applied to the background to simulate depth of field.
    *   **Mask Feathering:** Soft-edge blending logic implemented in `segmentation.py` to eliminate "halo" artifacts around hair and shoulders.
6.  **Final Polish:** Sub-pixel sharpening via Unsharp Masking and a final tone refinement for a "punchy" studio finish.

---

## ✨ Key Features
- ✅ **Motion Blur Recovery:** Rescues unusable shots taken in motion or with slight camera shake.
- ✅ **Dynamic Bokeh:** Simulates a high-end $f/1.8$ prime lens look.
- ✅ **Identity Preservation:** Prioritizes photorealism over "beautification" to keep the subject looking like themselves.
- ✅ **Modular Design:** Each stage (segmentation, restoration, deblurring) is a standalone Python module.
- ✅ **CPU Optimized:** Designed to run efficiently on standard hardware without requiring a high-end GPU.

---

## 🛠️ Installation

### 1. Prerequisites
- Python 3.8 or higher
- **GFPGAN Weights:** Download `GFPGANv1.4.pth` from the [Official Release](https://github.com/TencentARC/GFPGAN/releases/download/v1.3.0/GFPGANv1.4.pth) and place it in the `./weights/` folder.

### 2. Setup
```bash
# Clone the repository
git clone https://github.com/YashM8329/Future-of-Gaming-Assignment.git
cd Future-of-Gaming-Assignment

# Create and activate virtual environment
python -m venv venv

# Activate on Windows:
venv\Scripts\activate
# Activate on Mac/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt