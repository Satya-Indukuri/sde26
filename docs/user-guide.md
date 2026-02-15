# user-guide.md

# User Guide (Quick Start)

This quick start is written for demo users (students/instructors/evaluators) who want to understand how to use our **planned** Lung Cancer Detection & Risk Scoring system.

> **Project status:** We have not built the full system yet.  
> This document describes the **intended MVP workflow** so users will have clear “how-to” directions once the pipeline is implemented.

---

## What the system will do

Given a chest CT scan, the system is intended to:
1. Load the scan (DICOM → 3D volume, or NIfTI volume)
2. Preprocess it (resample spacing, intensity windowing/normalization)
3. Isolate lung regions (reduce false positives)
4. Detect suspicious pulmonary nodules (candidate locations/boxes)
5. Produce a basic report (JSON/text) that summarizes findings

**Planned additions (later milestone):**
- nodule segmentation (3D U-Net / nnU-Net style)
- measurements (diameter, volume)
- malignancy risk score or Lung-RADS-style categories
- overlays and a downloadable formatted report

---

## Who this guide is for

- **Demo user / evaluator** who wants to run the system on a sample scan and view results
- **Instructor/TA** reviewing project functionality and deliverables
- **Team members** who need consistent “how to run it” instructions

---

## What you need (intended requirements)

- A Windows/Mac/Linux computer
- One of the following (depending on final implementation):
  - **Python 3.10+** (recommended), or
  - **Docker Desktop** (optional deployment method later)
- A supported scan input (see below)

---

## Supported inputs (intended)

### Option A — DICOM CT series folder
A folder containing multiple `.dcm` slice files that make up one chest CT series.

### Option B — NIfTI CT volume (research datasets)
A `.nii` / `.nii.gz` file representing a 3D volume (often used in public datasets).

> If you are unsure what you have, see the [FAQ](faq.md).

---

## How to run (planned CLI workflow)

> Note: Commands below are **planned** placeholders. The final repo will include the exact script name and flags.

### Step 1 — Install dependencies (planned)

```bash
pip install -r requirements.txt
```
Step 2 — Run the pipeline (planned)

NIfTI input example
python run_pipeline.py --input sample_scan.nii.gz --output outputs/
DICOM folder input example
python run_pipeline.py --dicom_dir path/to/dicom_folder --output outputs/

Expected outputs (planned)

After a successful run, the pipeline is intended to produce:

outputs/report.json — machine-readable report

(Planned) outputs/overlays/ — images showing detections/masks

(Planned) outputs/measurements.csv — nodule measurements

(Planned) outputs/report.pdf — formatted patient summary
Example report.json format (illustrative)

{
  "study_id": "demo_001",
  "num_detections": 2,
  "detections": [
    {"id": "n1", "center_xyz_mm": [12.2, -43.1, 88.0], "confidence": 0.87},
    {"id": "n2", "center_xyz_mm": [30.0, -10.5, 66.4], "confidence": 0.61}
  ],
  "summary": {
    "top_finding": "n1",
    "note": "Prototype output, not clinical."
  }
}

How to interpret results (simple explanation)

Detections: candidate nodule-like regions that may require review.

Confidence: a model score for how nodule-like the region appears.

Important: a detection is not proof of cancer. This system is a research/academic prototype.

Safety + limitations

Academic prototype only — not for clinical diagnosis

Early versions may produce false positives/false negatives

Performance depends on scan quality and dataset differences

Need help?

Common questions: FAQ

For deeper details and planned configuration options: see the User Manual
