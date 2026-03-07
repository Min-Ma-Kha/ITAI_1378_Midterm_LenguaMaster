# 🌐 LenguaMaster — Visual Translation Mobile App

**Student:** Min Ma Kha  
**Course:** ITAI 1378 - Computer Vision and AI  
**Project Type:** Midterm Project  
**Project Tier:** 2 (Intermediate)

---

## 📋 Table of Contents

- [Tier Selection](#-tier-selection)
- [Problem Statement](#-problem-statement)
- [Solution Overview](#-solution-overview)
- [Technical Approach](#-technical-approach)
- [Dataset Plan](#-dataset-plan)
- [Success Metrics](#-success-metrics)
- [Week-by-Week Plan](#-week-by-week-plan)
- [Resources Needed](#-resources-needed)
- [Risks & Mitigation](#-risks--mitigation)
- [Repository Structure](#-repository-structure)
- [Getting Started](#-getting-started)

---

## 🎯 Tier Selection

**Selected Tier: 2 (Intermediate)**

**Justification:**
- Combines multiple computer vision techniques: text detection (YOLOv8), OCR (CRNN), and handwriting recognition (BiLSTM)
- Requires custom training on multiple datasets (ICDAR, IAM Handwriting, COCO-Text)
- Involves building an end-to-end pipeline integrating OCR + Neural Machine Translation
- Includes real-world challenges like handling handwritten text and poor lighting conditions
- More complex than basic classification (Tier 1), but scoped to be achievable within 6 weeks

---

## ❓ Problem Statement

International travelers face significant language barriers when reading menus, signs, and handwritten notes in foreign countries. Current translation apps fail in poor lighting conditions, struggle with handwritten text, and require constant internet connectivity. This causes ordering errors, missed opportunities, and in critical cases (allergen mis-translation), poses safety risks to users.

---

## 💡 Solution Overview

LenguaMaster is an on-device visual translation system that uses computer vision to detect and translate text in real-time through a mobile camera. The system captures images, detects text using YOLOv8, extracts characters via OCR (CRNN for printed text, BiLSTM for handwriting), and translates them using a lightweight Neural Machine Translation model — all without requiring internet connectivity. The translated text is overlaid on the original image in real-time.

---

## 🛠 Technical Approach

### Computer Vision Techniques
- **Text Detection:** Object detection to locate text regions in images
- **OCR (Optical Character Recognition):** Character extraction from detected text regions
- **HTR (Handwritten Text Recognition):** Specialized recognition for cursive and handwritten text

### Models
| Component | Model | Purpose |
|-----------|-------|---------|
| **Text Detection** | YOLOv8 | Locating text bounding boxes in images |
| **Printed OCR** | CRNN (Convolutional Recurrent Neural Network) | Extracting characters from printed text |
| **Handwritten OCR** | BiLSTM with Attention | Recognizing cursive and handwritten text |
| **Translation** | Transformer (lightweight) | Neural Machine Translation (6 language pairs) |

### Framework & Tools
- **Deep Learning:** PyTorch, TensorFlow Lite (for mobile deployment)
- **Computer Vision:** OpenCV (preprocessing, image enhancement)
- **OCR Libraries:** Tesseract (baseline), EasyOCR (backup)
- **Mobile Deployment:** CoreML (iOS), TensorFlow Lite + NNAPI (Android)
- **Training Infrastructure:** Google Colab (free GPU), Weights & Biases (experiment tracking)

---

## 📊 Dataset Plan

### Datasets

| Dataset | Source | Size | Labels | Purpose |
|---------|--------|------|--------|---------|
| **ICDAR 2015** | [Public Dataset](https://rrc.cvc.uab.es/?ch=4) | 10,000 images | Text bounding boxes + transcriptions | Printed text detection training |
| **COCO-Text** | [Public Dataset](https://bgshih.github.io/cocotext/) | 63,000 images | Text regions + annotations | Text detection augmentation |
| **IAM Handwriting Database** | [Public Dataset](https://fki.tic.heia-fr.ch/databases/iam-handwriting-database) | 500,000 samples | Handwritten word images + transcriptions | Handwriting recognition (HTR) |
| **Synthetic Data** | [TextRecognitionDataGenerator](https://github.com/Belval/TextRecognitionDataGenerator) | 50,000 images | Generated text with augmentation | Rare fonts, distortions, noise |
| **Custom Menu Photos** | Self-collected | 500 images | Restaurant menus (labeled) | Domain-specific fine-tuning |

### Data Preparation
- **Pre-labeled datasets:** ICDAR and IAM come with ground truth annotations
- **Custom annotation:** LabelImg tool for annotating restaurant menu photos
- **Data augmentation:** Rotation (±15°), blur, brightness adjustment, noise injection to simulate real-world conditions (poor lighting, camera shake)

### Dataset Links
- ICDAR 2015: https://rrc.cvc.uab.es/?ch=4&com=downloads
- COCO-Text: https://bgshih.github.io/cocotext/
- IAM Handwriting: https://fki.tic.heia-fr.ch/databases/iam-handwriting-database

---

## 📈 Success Metrics

### Primary Metrics

| Metric | Target | Measurement Method |
|--------|--------|-------------------|
| **OCR Accuracy (Printed Text)** | ≥ 90% | Character Error Rate (CER) on ICDAR test set |
| **HTR Accuracy (Handwritten Text)** | ≥ 75% | Character Error Rate (CER) on IAM test set |

### Secondary Metrics

| Metric | Target | Measurement Method |
|--------|--------|-------------------|
| **Translation Speed** | < 3 seconds | End-to-end latency per image (capture → translation → display) |
| **Model Size** | < 150 MB | Combined size of OCR + NMT models for mobile deployment |
| **Frame Rate (Video Mode)** | ≥ 10 FPS | Real-time translation frames per second |

### Evaluation Benchmarks
- **ICDAR 2015 Benchmark** for text detection and printed OCR
- **IAM Handwriting Database** test split for HTR
- **Custom test set:** 100 restaurant menu photos for end-to-end evaluation

---

## 📅 Week-by-Week Plan

| Week | Dates | Tasks | Milestones | Deliverables |
|------|-------|-------|-----------|--------------|
| **Week 8** | Mar 6 - Mar 13 | • Download datasets (ICDAR, IAM, COCO-Text)<br>• Set up Python environment (PyTorch, OpenCV)<br>• Initialize GitHub repository<br>• Explore data and run baseline models | ✅ Dataset ready<br>✅ Environment configured<br>✅ Baseline OCR running | `01_exploration.ipynb` |
| **Week 9** | Mar 14 - Mar 20 | • Train CRNN model for printed text OCR<br>• Train BiLSTM model for handwriting (HTR)<br>• Fine-tune YOLOv8 for text detection<br>• Log experiments in Weights & Biases | ✅ OCR models trained<br>✅ CER < 15% on validation set | `02_train_ocr.ipynb`<br>`models/crnn_ocr.pth`<br>`models/bilstm_htr.pth` |
| **Week 10** | Mar 21 - Mar 27 | • Integrate NMT model (Hugging Face Transformers)<br>• Build end-to-end pipeline (detection → OCR → translation)<br>• Create inference script for single images<br>• Test on custom menu photos | ✅ Pipeline functional<br>✅ End-to-end demo working | `03_pipeline.ipynb`<br>`src/inference.py` |
| **Week 11** | Mar 28 - Apr 3 | • Evaluate on ICDAR + IAM test sets<br>• Optimize model (quantization, pruning)<br>• Test edge cases (blur, rotation, low light)<br>• Improve accuracy with augmentation | ✅ Metrics meet targets<br>✅ Model optimized for mobile | `04_evaluation.ipynb`<br>`results/metrics.json` |
| **Week 12** | Apr 4 - Apr 10 | • Create demo video (screen recording + voiceover)<br>• Write comprehensive README documentation<br>• Prepare presentation slides<br>• Clean up code and notebooks | ✅ Demo video ready<br>✅ Documentation complete | `demo_video.mp4`<br>`docs/proposal.pdf` |
| **Week 13** | Apr 17 | • Final presentation in class<br>• Submit GitHub repo + slides to Canvas | 🎉 **Presentation Day!** | Final submission |

---

## 🧰 Resources Needed

### Compute
| Resource | Usage | Cost |
|----------|-------|------|
| **Google Colab (Free Tier)** | Model training (GPU: T4 or P100) | $0 |
| **Kaggle Notebooks** | Backup compute, dataset hosting | $0 |
| **Heidi Cluster** (if available) | Large-scale training experiments | $0 |

**Total Compute Cost:** $0 (using free tiers)

### Frameworks & Libraries
```
PyTorch >= 2.0
TensorFlow Lite >= 2.13
OpenCV >= 4.8
Ultralytics (YOLOv8) >= 8.0
Transformers (Hugging Face) >= 4.30
EasyOCR >= 1.7
Weights & Biases (wandb) >= 0.15
LabelImg >= 1.8 (for annotation)
```

### Tools
- **Annotation:** LabelImg (bounding box labeling)
- **Experiment Tracking:** Weights & Biases
- **Version Control:** Git + GitHub
- **Notebook Environment:** Jupyter Lab / Google Colab

### APIs (if needed)
- **Google Cloud Vision API** (fallback for OCR evaluation) — Free tier: 1,000 requests/month

### Estimated Total Cost
**$0** — All resources use free tiers and open-source tools

---

## ⚠️ Risks & Mitigation

| Risk | Probability | Impact | Mitigation Strategy |
|------|-------------|--------|-------------------|
| **Insufficient handwriting training data** | Medium | High | • Use pre-trained HTR models from Hugging Face<br>• Apply heavy data augmentation (rotation, distortion)<br>• Generate synthetic handwriting samples |
| **OCR accuracy below 90% target** | Medium | High | • Train for more epochs with learning rate scheduling<br>• Use ensemble models (CRNN + EasyOCR)<br>• Switch to larger pre-trained models (TrOCR) |
| **Model size exceeds 150 MB limit** | High | Medium | • Apply INT8 quantization using TensorFlow Lite<br>• Use MobileNet-based architectures<br>• Deploy NMT via cloud API instead of on-device |
| **Translation latency > 3 seconds** | Medium | Medium | • Optimize preprocessing pipeline (resize, grayscale early)<br>• Use DistilBERT for faster NMT<br>• Implement batch processing for multiple text regions |
| **Poor performance on handwritten cursive** | High | High | • Focus on printed text first (MVP), handwriting as stretch goal<br>• Use transfer learning from IAM pre-trained models<br>• Collect more domain-specific handwriting samples |
| **Dataset licensing issues** | Low | Medium | • Verify all datasets are publicly available for academic use<br>• ICDAR, IAM, COCO-Text are all research-friendly<br>• Credit datasets properly in documentation |
| **Time constraints (6 weeks)** | Medium | High | • Prioritize core features: text detection + printed OCR + NMT<br>• Handwriting and real-time video are stretch goals<br>• Use pre-trained models where possible to save time |

---

## 📁 Repository Structure

```
ITAI_1378_Midterm_LenguaMaster/
│
├── README.md                  ← You are here! Project overview and documentation
├── requirements.txt           ← Python dependencies (pip install -r requirements.txt)
├── .gitignore                 ← Files to exclude from version control
│
├── notebooks/                 ← Jupyter notebooks for exploration and training
│   ├── 01_exploration.ipynb   ← Dataset exploration and visualization
│   ├── 02_train_ocr.ipynb     ← OCR model training (CRNN + BiLSTM)
│   ├── 03_pipeline.ipynb      ← End-to-end pipeline integration
│   └── 04_evaluation.ipynb    ← Model evaluation and metrics
│
├── src/                       ← Source code (Python scripts)
│   ├── __init__.py
│   ├── data_loader.py         ← Dataset loading and preprocessing
│   ├── models.py              ← Model architectures (CRNN, BiLSTM, YOLO)
│   ├── train.py               ← Training script
│   ├── inference.py           ← Inference pipeline for single images
│   └── utils.py               ← Helper functions (image preprocessing, metrics)
│
├── data/                      ← Dataset storage (not committed to Git)
│   ├── README.md              ← Dataset download instructions and links
│   ├── raw/                   ← Raw downloaded datasets
│   ├── processed/             ← Preprocessed images and annotations
│   └── custom/                ← Custom-collected menu photos
│
├── models/                    ← Trained model checkpoints
│   ├── crnn_ocr.pth           ← Trained CRNN model for printed text
│   ├── bilstm_htr.pth         ← Trained BiLSTM model for handwriting
│   ├── yolov8_detect.pt       ← Fine-tuned YOLOv8 for text detection
│   └── nmt_transformer.pth    ← Neural Machine Translation model
│
├── results/                   ← Evaluation results and outputs
│   ├── metrics.json           ← Accuracy, CER, latency metrics
│   ├── predictions/           ← Sample predictions with overlays
│   └── plots/                 ← Training curves, confusion matrices
│
├── docs/                      ← Documentation and presentation
│   ├── proposal.pdf           ← Midterm proposal slides (copy of submission)
│   └── demo_video.mp4         ← Project demo video (link if file is large)
│
└── tests/                     ← Unit tests (optional)
    └── test_inference.py      ← Test inference pipeline
```

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://https://github.com/Min-Ma-Kha/ITAI_1378_Midterm_LenguaMaster.git
cd ITAI_1378_Midterm_LenguaMaster
```

### 2. Install Dependencies

```bash
# Create virtual environment (optional but recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install required packages
pip install -r requirements.txt
```

### 3. Download Datasets

Follow instructions in `data/README.md` to download:
- ICDAR 2015
- COCO-Text
- IAM Handwriting Database

Or run the provided download script:
```bash
python src/download_datasets.py
```

### 4. Run Exploration Notebook

```bash
jupyter lab notebooks/01_exploration.ipynb
```

### 5. Train Models

```bash
# Train OCR models
python src/train.py --model crnn --epochs 50 --batch_size 32

# Train handwriting model
python src/train.py --model bilstm --epochs 30 --batch_size 16
```

### 6. Run Inference

```bash
python src/inference.py --image path/to/menu.jpg --output results/predictions/
```

---

## 📊 Current Progress

- [x] Repository initialized
- [x] Datasets identified and download links verified
- [x] Project proposal slides created
- [ ] Environment setup complete
- [ ] Baseline OCR model running
- [ ] CRNN model trained
- [ ] BiLSTM HTR model trained
- [ ] End-to-end pipeline functional
- [ ] Metrics meet targets
- [ ] Demo video created

---

## 📝 References

1. **ICDAR 2015 Robust Reading Competition:** https://rrc.cvc.uab.es/?ch=4
2. **COCO-Text Dataset:** Veit et al., "COCO-Text: Dataset and Benchmark for Text Detection and Recognition in Natural Images"
3. **IAM Handwriting Database:** Marti & Bunke, "The IAM-database: an English sentence database for offline handwriting recognition"
4. **CRNN for OCR:** Shi et al., "An End-to-End Trainable Neural Network for Image-based Sequence Recognition"
5. **YOLOv8 Documentation:** https://docs.ultralytics.com/
6. **Hugging Face Transformers:** https://huggingface.co/docs/transformers/

---

## 🤝 Contributing

This is a solo project for ITAI 1378. Contributions are not accepted, but feedback is welcome!

---

## 📧 Contact

**Min Ma Kha**  
**Course:** ITAI 1378 - Computer Vision and AI  
**Semester:** Spring 2026

---

## 📜 License

This project is for educational purposes as part of ITAI 1378. All datasets used are publicly available for academic research.

---

**Last Updated:** March 2026
