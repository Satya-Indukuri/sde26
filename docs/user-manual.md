
```md
# user-manual.md

# User Manual (Detailed)

This manual describes how users will interact with our Lung Cancer Detection & Risk Scoring system once implemented.

> **Project status:** The system is still in development.  
> This manual documents the **intended design**, workflow, inputs/outputs, and planned configuration so users have a reference guide early.

---

## Table of Contents
1. [System Overview](#system-overview)
2. [User Roles](#user-roles)
3. [Inputs](#inputs)
4. [Outputs](#outputs)
5. [End-to-End Workflow](#end-to-end-workflow)
6. [Configuration (Planned)](#configuration-planned)
7. [Error Handling (Planned)](#error-handling-planned)
8. [Interpreting the Output](#interpreting-the-output)
9. [Privacy & Data Handling](#privacy--data-handling)
10. [Known Limitations](#known-limitations)

---

## System Overview

The system is designed to replicate key steps of a radiologist-style workflow for lung CT screening:

1. **Preprocessing**
   - Load CT data (DICOM series → 3D volume, or NIfTI volume)
   - Normalize voxel spacing (resampling)
   - Apply intensity normalization / HU windowing for lung tissue visibility
   - Isolate lung regions (reduce false positives and speed inference)

2. **Nodule Detection (MVP goal)**
   - Identify candidate pulmonary nodules (locations/boxes + confidence)

3. **Segmentation (Planned)**
   - Produce precise nodule boundary masks (3D U-Net / nnU-Net style)

4. **Risk Scoring / Categorization (Planned)**
   - Estimate malignancy risk score OR map to Lung-RADS-like categories
   - Intended as decision support, not diagnosis

5. **Reporting**
   - Generate a patient-level summary report (JSON/text)
   - (Planned) overlays, measurements, and formatted export

---

## User Roles

- **Demo user / evaluator**
  - Runs inference on sample scans
  - Reviews the generated report and outputs

- **Technical user / developer**
  - Adjusts thresholds
  - Updates model checkpoints
  - Runs tests and performance checks

---

## Inputs

### Supported input types (intended)

#### 1) DICOM CT series folder
- A folder containing multiple CT slices (`.dcm`)
- Expected: a single CT series per folder

#### 2) NIfTI volume (`.nii` / `.nii.gz`)
- Common in research datasets
- Easier for consistent pipeline development

### Input requirements (intended)
- Chest CT scans (thorax)
- Reasonable slice ordering (for DICOM)
- De-identified data preferred

### Common input issues
- multiple series mixed in one folder
- missing/corrupt slices
- non-chest CT (wrong anatomy)

See the [FAQ](faq.md) for dataset guidance.

---

## Outputs

### MVP outputs (intended)
- `report.json` including:
  - de-identified scan/study id
  - list of detections: location + confidence
  - patient-level summary (top finding)

### Planned outputs (later milestone)
- `overlays/` with preview images (axial slices with boxes/masks)
- `measurements.csv` with per-nodule diameter/volume
- formatted report export (e.g., `report.pdf`)

---

## End-to-End Workflow

### Step 1 — Provide an input scan
User provides either:
- a DICOM series folder, or
- a NIfTI file

### Step 2 — Preprocessing
The system will:
- resample voxel spacing to a consistent target
- apply intensity normalization / HU windowing
- isolate lung regions

### Step 3 — Detection (MVP)
The detector identifies candidate nodules and outputs:
- candidate center coordinates and/or bounding boxes
- confidence score per candidate

### Step 4 — (Planned) Segmentation + measurements
For each detection, the system will:
- segment the nodule boundary
- compute:
  - volume (mm³)
  - diameter (mm)

### Step 5 — (Planned) Risk scoring / category
For each nodule:
- output a risk score (0–1 or 0–100%) and/or
- map to a Lung-RADS-like category based on size and characteristics

### Step 6 — Reporting
Generate a patient-level summary:
- number of nodules detected
- top finding (highest risk/confidence)
- per-nodule details (size/risk) (planned)

---

## Configuration (Planned)

The final system is expected to allow simple configuration for tuning sensitivity and runtime:

Possible settings:
- `DEVICE`: `cpu` or `cuda`
- `CONF_THRESHOLD`: detection threshold
- `MAX_DETECTIONS`: cap number of candidates returned
- `MODEL_PATH_DETECTOR`: detector weights path
- `MODEL_PATH_SEGMENTER`: segmentation weights path (planned)
- `OUTPUT_DIR`: where results are saved

Example (illustrative):
```bash
python run_pipeline.py --input sample_scan.nii.gz --conf 0.5 --device cpu --max_detections 10

Error Handling (Planned)

The system is intended to fail safely and provide clear error messages for:

invalid file type / unsupported input

missing/corrupted DICOM slices

mixed DICOM series

non-chest CT scans

missing model files


Interpreting the Output

Detection confidence reflects how “nodule-like” a region appears to the detector.

A higher confidence does not mean confirmed cancer.

Any risk score is a decision-support signal based on training data and may not generalize perfectly.



Privacy & Data Handling

Prefer de-identified inputs for demos and testing

Avoid logging patient identifiers

Store outputs locally unless cloud storage is required for deployment milestones

Treat CT data as sensitive even when de-identified


Known Limitations

Prototype only; not clinically validated

False positives/false negatives are expected in early versions

Public datasets may differ from real clinical scans (dataset bias)

Some public “risk labels” can be proxies (radiologist ratings) rather than pathology-confirmed outcomes

For common questions, see: FAQ

