# AI Usage Log — LenguaMaster
**Course:** ITAI 1378 — Computer Vision and AI  
**Author:** Min Ma Kha  

---

## Entry 1
**Date:** 2026-05-04 | **Step:** Step 1 — TRDG Synthetic Generation  
**Tool:** Claude (Anthropic)  
**Task:** TRDG CLI silently generated 0 images for all languages.  
**AI Output:** Suggested replacing subprocess CLI with Python API using GeneratorFromDict and GeneratorFromStrings with CJK fallback word list.  
**How Used:** Adopted Python API directly. Added CJK travel-domain word list manually. Verified by confirming image counts > 0.  
**Modification:** Added label-embedding in filename for later retrieval without CSV.

---

## Entry 2
**Date:** 2026-05-04 | **Step:** Step 4.3 — IAM Dataset  
**Tool:** Claude (Anthropic)  
**Task:** IAM Handwriting Database download was failing — requires registration.  
**AI Output:** Suggested Kaggle HTR dataset as drop-in replacement with auto-detecting CSV loader and Drive-mount extraction pattern.  
**How Used:** Downloaded Kaggle HTR (190K samples), used suggested load_kaggle_dataset() function.  
**Modification:** Added ZIP_MAP pattern to match existing ICDAR unzip style.

---

## Entry 3
**Date:** 2026-05-05 | **Step:** Step 4.7 — HTR Training  
**Tool:** Claude (Anthropic)  
**Task:** Training ran for 5+ hours with no output.  
**AI Output:** Identified code only prints every 10 epochs. Provided diagnostic cell to measure batch time and estimate total training time (27 hours).  
**How Used:** Ran diagnostic, confirmed 27-hour estimate, applied reduced config.  
**Modification:** Chose epochs=10, batch_sz=128, max_train_samples=10000 after reviewing timing output.

---

## Entry 4
**Date:** 2026-05-05 | **Step:** Step 4.2 — HTRConfig  
**Tool:** Claude (Anthropic)  
**Task:** Needed full updated HTRConfig and training loop with logging to reduce training time.  
**AI Output:** Provided updated HTRConfig dataclass with per-batch progress (every 20 batches), epoch-level ETA, timing estimate before training, and checkpoint indicator.  
**How Used:** Replaced original sections 4.2 and 4.7 cells entirely.  
**Modification:** Changed max_train_samples from 20K to 10K after reviewing timing output.

---

## Entry 5
**Date:** 2026-05-05 | **Step:** Step 5.1 — Imports  
**Tool:** Claude (Anthropic)  
**Task:** Step 5.1 failing with ImportError for get_linear_schedule_with_warmup.  
**AI Output:** Identified it is a HuggingFace function (in transformers) not PyTorch (torch.optim.lr_scheduler).  
**How Used:** Moved import to from transformers import ... line.  
**Modification:** None — fix was minimal and correct.

---

## Entry 6
**Date:** 2026-05-06 | **Step:** Step 6 — Translation Engine  
**Tool:** Claude (Anthropic)  
**Task:** Helsinki-NLP models failing for Japanese, Chinese, Portuguese — producing empty or incorrect translations.  
**AI Output:** Recommended facebook/m2m100_418M — single 418MB model covering 100 languages, MIT license, simpler get_lang_id() API vs NLLB.  
**How Used:** Replaced all 6 Helsinki-NLP models with single M2M100 model. Updated Cell 2 entirely.  
**Modification:** Kept only the 6 required languages, removed bonus languages from NLLB suggestion.

---

## Entry 7
**Date:** 2026-05-06 | **Step:** Step 6 — bidi Import Error  
**Tool:** Claude (Anthropic)  
**Task:** EasyOCR import failing with ImportError: cannot import name 'get_display' from 'bidi'.  
**AI Output:** Identified python-bidi version conflict. Suggested pinning python-bidi==0.4.2 and adding a monkey-patch fallback if get_display is missing.  
**How Used:** Added monkey-patch block to Cell 1 that gracefully handles both old and new bidi versions.  
**Modification:** Removed duplicate import sys since it was already imported at top of cell.

---

## Entry 8
**Date:** 2026-05-07 | **Step:** Step 6 — Auto-Detection  
**Tool:** Claude (Anthropic)  
**Task:** Translation was always outputting Spanish regardless of input language. Needed full auto-detection.  
**AI Output:** Provided complete rewrite of Cell 2 with langdetect integration, LANGDETECT_MAP, and detect_lang_code() function that maps detected codes to supported language codes.  
**How Used:** Replaced Cell 2 entirely. Removed all hardcoded language references.  
**Modification:** Added fallback to Spanish when detection fails or returns unsupported language.

---

## Entry 9
**Date:** 2026-05-07 | **Step:** Step 6 — Testing  
**Tool:** Claude (Anthropic)  
**Task:** Needed test cells for image, video, and raw text that are integrated with Steps 1–5 rather than standalone.  
**AI Output:** Provided 6 cells (6.1–6.6) structured to append after Step 5, sharing all global state (ocr_reader, m2m_model) with naming convention run_translation_pipeline() to avoid conflicts.  
**How Used:** Added cells 6.1–6.6 at end of notebook after last Step 5 cell.  
**Modification:** Renamed run_pipeline() to run_translation_pipeline() to avoid name conflict with Step 5 LenguaMasterSystem.

---

## Summary

| # | Step | Issue | Resolution |
|---|------|-------|-----------|
| 1 | Step 1 | TRDG CLI broken | Python API replacement |
| 2 | Step 4.3 | IAM unavailable | Kaggle HTR integration |
| 3 | Step 4.7 | Training too slow | Dataset subsampling + config scaling |
| 4 | Step 4.7 | No output/logging | Full training loop rewrite with ETA |
| 5 | Step 5.1 | Wrong import module | transformers vs torch fix |
| 6 | Step 6 | Helsinki-NLP failed JP/ZH/PT | Replaced with M2M100 418M |
| 7 | Step 6 | bidi ImportError | Monkey-patch fallback |
| 8 | Step 6 | Hardcoded Spanish output | Full auto-detection with langdetect |
| 9 | Step 6 | Standalone test cells | Integrated as Steps 6.1–6.6 |
