# User Guide (Quick Start)

This quick start explains how the system is intended to be used for demos and evaluation.

> Current status: Our initial implementation is expected to run as a **command-line pipeline** (MVP).  
> A web UI is **Planned** for later milestones.

---

## What the system does (high level)

Given a chest CT scan, the system will:
1. Load and preprocess the scan (normalize spacing, window intensities)
2. Isolate lung regions (reduce false positives)
3. Detect suspicious nodule candidates (locations/boxes)
4. Produce a simple report (JSON/text) containing detections and summary information  
   - (Planned) segmentation masks and measurements  
   - (Planned) malignancy risk / Lung-RADS-style category

---

## What you need (for MVP)

- A computer with Windows/Mac/Linux
- Python 3.10+ (recommended) OR Docker Desktop (optional later)
- A supported sample input:
  - **DICOM CT series folder** (multiple `.dcm` slices), or
  - **NIfTI** file (`.nii` / `.nii.gz`) if using pre-converted research datasets

> For early development/testing, we will use publicly available research datasets such as LIDC-IDRI/LUNA16/MSD. (See FAQ.)

---

## How to run (planned CLI workflow)

### Step 1 — Install dependencies (planned)
```bash
pip install -r requirements.txt
