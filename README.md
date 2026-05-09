# LenguaMaster 🌐
### Visual Translation & Edge AI for International Travelers

**Course:** ITAI 1378 — Computer Vision and AI  
**Author:** Min Ma Kha  
**Date:** 2026-05-04

---

## Overview

LenguaMaster is a multi-modal, **offline-capable** visual translation system for international travelers. It integrates six AI sub-systems into a single end-to-end pipeline that detects text in images, recognizes it (printed or handwritten), auto-detects the source language, and translates it to English — all without requiring an internet connection.

```
Input Image / Video
        │
        ▼
[Step 2] YOLOv8-nano          →  Text bounding boxes
        │
        ▼
[Step 4] MobileNetV3 Router   →  Printed or Handwritten?
        │
        ├── Printed    ──▶ [Step 3] CRNN-CTC (ONNX)
        └── Handwritten ──▶ [Step 4] CRNN-Attention (ONNX)
        │
        ▼  Recognised text
[Step 6] langdetect           →  Auto-detect source language
        │
        ▼
[Step 6] M2M100 (facebook/m2m100_418M)  →  English translation
        │
        ▼
Side-by-side display with OCR boxes + English output
```

---

## Demo Video

📹 **[Watch Demo on Google Drive](https://drive.google.com/file/d/19JEP6secFOwJSgoo9IfDPb-AfnaFGIi_/view?usp=sharing)**


---

## Repository Structure

```
LenguaMaster/
├── notebooks/
│   └── Step1_Environment_and_EDA.ipynb   # Main notebook (Steps 1–6)
├── src/
│   └── pipeline.py                        # Standalone pipeline module
├── docs/
│   ├── AI_usage_log.md                    # AI assistance log (10+ entries)
│   └── presentation.pdf                   # Final presentation slides
├── results/
│   ├── step2_detection_dashboard.png
│   ├── step3_ocr_dashboard.png
│   ├── step4_htr_dashboard.png
│   └── step6_translation_output.png
└── README.md
```

---

## Pipeline Components

| Step | Component | Model | Export | Latency |
|------|-----------|-------|--------|---------|
| 2 | Text Detection | YOLOv8-nano | ONNX opset 17 + INT-8 | 59.6 ms |
| 3 | Printed OCR | CRNN-CTC | ONNX opset 18 | 5.0 ms |
| 4 | Handwriting HTR | CRNN-Attention | ONNX | ~10 ms |
| 4 | Router | MobileNetV3-Small | PyTorch | <10 ms |
| 6 | Language Detection | langdetect | Python library | <1 ms |
| 6 | Translation | M2M100 (418M) | HuggingFace | ~30 ms |

**Total target latency: < 200ms end-to-end on mobile CPU**

---

## Datasets

| Dataset | Task | Samples |
|---------|------|---------|
| ICDAR 2015 | Text Detection | 1,500 images |
| Kaggle HTR | Handwriting Recognition | 190,920 words |
| COCO-Text v2 | Scene Text | 63,686 images |
| TRDG Synthetic | OCR Augmentation | 9,000 images |
| M2M100 (Meta) | Translation | 100 languages, MIT license |

---

## Supported Languages (Auto-Detected)

🇪🇸 Spanish · 🇫🇷 French · 🇩🇪 German · 🇧🇷 Portuguese · 🇨🇳 Chinese · 🇯🇵 Japanese

All detected automatically from image text — no manual language selection needed.

---

## Key Results

| Metric | Value |
|--------|-------|
| Printed OCR — Greedy CER | 7.57% |
| Printed OCR — Beam CER | **4.56%** (95% CI: [3.6, 5.6]) |
| Text Detection F1 | 0.628 (IoU ≥ 0.50) |
| Text Detection Recall | 0.734 |
| CRNN ONNX Latency | 5.0 ms/image (CPU) |

---

## Setup & Running

### Requirements
```bash
pip install torch torchvision ultralytics transformers datasets
pip install opencv-python-headless albumentations editdistance
pip install onnx onnxruntime easyocr langdetect sentencepiece
pip install python-bidi arabic-reshaper
```

### Run in Google Colab
1. Open `notebooks/Step1_Environment_and_EDA.ipynb` in Google Colab
2. Mount Google Drive and upload your datasets
3. Run all cells in order (Steps 1–6)
4. Test using Step 6 test cells (image, video, raw text)

---

## Fixes Applied

| Issue | Fix |
|-------|-----|
| TRDG CLI generates 0 images | Replaced with Python API (GeneratorFromDict) |
| IAM dataset requires registration | Integrated Kaggle HTR dataset (190K samples) |
| Training estimated 27 hours | Subsampled to 10K · 10 epochs · batch 128 → ~15 min |
| No output for 5+ hours | Added per-batch progress + ETA every epoch |
| Wrong scheduler import in Step 5.1 | Moved get_linear_schedule_with_warmup to transformers |
| Helsinki-NLP failed for JP/ZH/PT | Replaced with facebook/m2m100_418M (MIT, 100 languages) |

---

## References

- Shi et al. (2017) CRNN · Graves et al. (2006) CTC · Jocher et al. (2023) YOLOv8
- Fan et al. (2020) M2M100 · Kang et al. (2022) CRNN-Attention HTR
- Howard et al. (2019) MobileNetV3 · Papineni et al. (2002) BLEU

---

## License

This project is for educational purposes as part of ITAI 1378.
