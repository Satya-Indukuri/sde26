
```md
# faq.md

# FAQ (Frequently Asked Questions)

## 1) Is this system approved for medical diagnosis?
No. This is an academic prototype for a senior design project and is not intended for clinical diagnosis or medical decision-making.

## 2) What inputs will the system support?
Intended support includes:
- DICOM CT series folders (`.dcm` slices)
- NIfTI volumes (`.nii` / `.nii.gz`) commonly used in research datasets

## 3) Where do we get sample CT data for development/testing?
We plan to use public research datasets commonly used for lung CT tasks, such as:
- LIDC-IDRI / LUNA16 (nodule detection research)
- Medical Segmentation Decathlon (lung tumor segmentation task)

These datasets are widely used in academic settings for benchmarking.

## 4) Why might a “normal” scan still produce detections?
False positives are common in early detection systems. Detection thresholds and lung isolation are intended to reduce false positives, but they may not eliminate them.

## 5) What does “confidence” mean in the detection results?
Confidence is a model score indicating how likely a region is to resemble a nodule. It does **not** confirm cancer.

## 6) What does the “risk score” mean?
A risk score is intended to represent relative malignancy likelihood based on model training data. It is decision support only and not a diagnosis.

## 7) Why are risk labels in public datasets sometimes unreliable?
Some datasets provide radiologist annotations/ratings that can act as proxies for malignancy rather than pathology-confirmed outcomes. We will clearly document label limitations.

## 8) Will the project have a web UI?
A web UI is **planned** for later milestones. MVP is expected to be command-line and/or API-based first.

## 9) Do I need a GPU?
A GPU is recommended for faster 3D inference, but CPU-only runs may be supported for simple demos (with longer runtimes).

## 10) What outputs should I expect?
MVP: a JSON/text report listing detected nodules and a basic summary.  
Planned: overlays, measurements table (diameter/volume), and a formatted report export.
