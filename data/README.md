\# Dataset Documentation



\## Overview

This directory contains all data for the LenguaMaster Visual Translation project.



\## Structure

\* `raw/`: Original, unprocessed images and text samples.

\* `processed/`: Preprocessed images and augmentations ready for training.

\* `sample/`: Small sample dataset for quick testing.



\## Dataset Details



\### Sources \& Tasks

\* \*\*ICDAR 2015\*\*: Text Detection (1,500 images) - Research use

\* \*\*Kaggle HTR\*\*: Handwriting Recognition (190,920 words) - Open

\* \*\*COCO-Text v2\*\*: Scene Text (63,686 images) - CC BY 4.0

\* \*\*TRDG Synthetic\*\*: OCR Augmentation (9,000 images) - MIT

\* \*\*M2M100 (Meta)\*\*: Translation (100 languages) - MIT



\### Data Processing Pipeline

1\. SHA-256 Verification

2\. Format Conversion

3\. Stratified Split 

4\. Augmentation (3-Tier)

5\. ONNX / M2M100 Export



\*Note: Full datasets are not hosted in this repository due to size constraints. Please refer to the respective dataset providers to download the raw data.\*

