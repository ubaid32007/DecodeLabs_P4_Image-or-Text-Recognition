# Project 4 — Image or Text Recognition (Basic)
**DecodeLabs Artificial Intelligence Training Kit — Batch 2026**

This project implements **both** optional execution paths described in the
brief, each satisfying the four Gatekeeper Rule validations.

```
project4/
├── ocr_path/
│   └── ocr_pipeline.py              # Path 1: OCR
├── object_detection_path/
│   ├── object_detection.py          # Path 2: Object Detection
│   ├── MobileNetSSD_deploy.prototxt # pre-trained model architecture
│   └── MobileNetSSD_deploy.caffemodel # pre-trained model weights
├── sample_images/
│   ├── document_sample.png          # synthetic noisy/skewed "scan" for OCR
│   ├── person_sample.jpg            # real photo for detection (person)
│   └── cat_sample.jpg               # real photo for detection (cat)
├── outputs/                         # generated annotated results
└── README.md
```

---

## Path 1 — Optical Character Recognition (`pytesseract`)

**Library:** `pytesseract`, a Python wrapper around Google's Tesseract
engine (CNN + bi-directional LSTM sequence reader).

### Pipeline (`ocr_pipeline.py`)

| Step | Function | What it does |
|---|---|---|
| 1 | `to_grayscale()` | Collapses the 3-channel RGB matrix into a 1-channel intensity matrix |
| 2 | `denoise()` | Gaussian blur to remove sensor noise before thresholding |
| 3 | `deskew()` | Finds the minimum-area bounding box of ink pixels and rotates the image to a horizontal baseline |
| 4 | `binarize()` | Otsu's method automatically computes the optimal black/white cutoff (`if intensity >= cutoff: white else black`) |
| 5 | `run_ocr()` | Runs Tesseract with a configurable Page Segmentation Mode (PSM) and returns per-word text + confidence |
| 6 | `apply_confidence_gate()` | Drops every word below 80% confidence |
| 7 | `annotate()` | Draws green boxes + confidence labels for accepted words, dim red boxes for rejected ones |

### Run it

```bash
python3 ocr_path/ocr_pipeline.py sample_images/document_sample.png --psm 6 --conf 80 --out outputs/ocr_output.png
```

### Result

```
RECOGNIZED TEXT (confidence >= 80%)
DECODELABS TRAINING KIT Artificial Intelligence Project 4 Image and
Text Recognition Basic Batch 2026 Greater Lucknow India Confidence
threshold eighty percent

Accepted: 21   Rejected (< 80%): 0
Average raw confidence across all candidates: 95.3%
```

The test image was synthetically generated with realistic imperfections
(uneven lighting, pixel noise, a 2° skew) to prove the pre-processing
chain — not just the OCR call — is doing real work.

---

## Path 2 — Object Detection (`cv2.dnn` + MobileNet-SSD)

**Library:** OpenCV's DNN module running a pre-trained **MobileNet-SSD**
Caffe model (trained on the PASCAL VOC dataset, 20 object classes).
This is **Transfer Learning**: the model's convolutional backbone
already understands universal visual features from ImageNet — no local
training is performed.

### Pipeline (`object_detection.py`)

| Step | Function | What it does |
|---|---|---|
| 1 | `load_network()` | Loads the pre-trained architecture (`.prototxt`) + weights (`.caffemodel`) |
| 2 | `build_blob()` | `cv2.dnn.blobFromImage`: resizes to 300×300, subtracts the mean (127.5), scales pixel values — the required "4D Blob" |
| 3 | `run_detection()` | Single forward pass (Single Shot Detector — no sliding windows) |
| 4 | `apply_confidence_gate()` | Rescales normalized (0–1) coordinates to real pixel coordinates and drops any detection below 80% confidence |
| 5 | `annotate()` | Draws a labeled, colored bounding box for every accepted detection |

### Run it

```bash
python3 object_detection_path/object_detection.py sample_images/person_sample.jpg --conf 0.80 --out outputs/detection_person.png
python3 object_detection_path/object_detection.py sample_images/cat_sample.jpg   --conf 0.80 --out outputs/detection_cat.png
```

### Results

```
ACCEPTED DETECTIONS (confidence >= 80%)
  person        99.6%   box=(4,15)-(370,508)

ACCEPTED DETECTIONS (confidence >= 80%)
  cat          100.0%   box=(1,1)-(448,299)
```

---

## Gatekeeper Rule — Validation Checklist

| # | Requirement | Path 1 (OCR) | Path 2 (Detection) |
|---|---|---|---|
| 1 | Library Integration | `pytesseract` wrapping Tesseract | `cv2.dnn` + MobileNet-SSD |
| 2 | Pre-Processing Integrity | Grayscale → Blur → Deskew → Otsu Threshold | 4D blob: resize + mean subtraction |
| 3 | Accuracy Benchmarking (≥80%) | `apply_confidence_gate()` | `apply_confidence_gate()` |
| 4 | Visual Confirmation | Annotated image with legible text + per-word confidence | Annotated image with labeled bounding boxes |

Both scripts print a full console report (candidates found, accepted vs.
rejected counts, average confidence) in addition to saving the annotated
image, so every validation point is independently checkable.

## Requirements

```
opencv-python
pytesseract
numpy
```
System dependency: `tesseract-ocr` (the underlying OCR engine binary).

## Notes on the confidence gate

Per the brief: *"High thresholds minimize False Positives but increase
the risk of False Negatives. In Project 4, 80% is the absolute minimum
standard."* Both scripts expose `--conf` so the threshold can be tuned
per use case, but default to 0.80 to match the DecodeLabs standard.
