\# Models Directory



\## Structure

\* `pretrained/`: Pre-trained models downloaded from public sources (e.g., M2M100).

\* `trained/`: Our trained/fine-tuned and exported ONNX models.



\## Pipeline Components



| Step | Component | Model | Export Format | Latency (Target) |

|------|-----------|-------|---------------|------------------|

| 2 | Text Detection | YOLOv8-nano | ONNX opset 17 + INT-8 | 59.6 ms (FP32) |

| 3 | Printed OCR | CRNN-CTC | ONNX opset 18 | 5.0 ms |

| 4 | Handwriting HTR | CRNN-Attention | ONNX | \~10 ms |

| 4 | Router | MobileNetV3-Small | PyTorch | < 10 ms |

| 6 | Translation | M2M100 (418M) | HuggingFace | \~30 ms |



\## Model Weights (Too Large for Git)

Exported `.onnx` files and the 418MB M2M100 model are not stored in this repository. 

\* The `facebook/m2m100\_418M` model will auto-download via the HuggingFace `transformers` library on first run.

\* YOLOv8 and CRNN models must be trained and exported using the provided notebook pipeline.

