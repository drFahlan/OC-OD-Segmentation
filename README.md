# OD & OC Segmentation from Retinal Fundus Photography

Segmentation of the **Optic Disc (OD)** and **Optic Cup (OC)** from fundus images using classical computer vision methods. Built on the [Drishti-GS](https://cvit.iiit.ac.in/projects/mip/drishti-gs/mip-dataset2/Home.php) dataset.

> Course project — EB3206 Biomedical Image Processing

**Authors:** Dwi Rezky Fahlan · Sola Gracia Rania

---

## Overview

Accurate OD and OC segmentation is a prerequisite for glaucoma screening, where the cup-to-disc ratio (CDR) is the primary diagnostic indicator. This project implements a full classical pipeline: localization → segmentation → post-processing, without deep learning.

### Pipeline

```
Fundus Image
    │
    ▼
OD Localization        Sliding window (r=330px) on green channel to find max intensity region
    │
    ▼
OD Segmentation        Otsu thresholding on red channel of localized ROI
    │
    ▼
OD Post-Processing     Morphological opening + closing (disk r=30) → Ellipse Hough Transform
    │
    ▼
OC Segmentation        K-means clustering (K=4) on localized OD region
    │
    ▼
OC Post-Processing     Morphological closing + opening (disk r=30) → Ellipse Hough Transform
```

---

## Results

### Optic Disc (OD)

| Stage | Mean F1 | Min F1 | Max F1 |
|---|---|---|---|
| Segmentation | 0.947 | 0.824 | 0.976 |
| Post-processing (Ellipse) | 0.947 | 0.839 | 0.982 |
| Test set (Segmentation) | 0.955 | 0.942 | 0.974 |
| Test set (Ellipse) | 0.943 | 0.924 | 0.969 |

### Optic Cup (OC)

| Stage | Mean F1 | Min F1 | Max F1 |
|---|---|---|---|
| Segmentation | 0.746 | 0.633 | 0.871 |
| Post-processing (Ellipse) | 0.829 | 0.636 | 0.940 |
| Test set (Segmentation) | 0.735 | 0.549 | 0.864 |
| Test set (Ellipse) | 0.808 | 0.518 | 0.908 |

OD localization achieves **100% coverage** across all samples. OC post-processing improves mean F1 by ~8 points over raw segmentation.

---

## Notebook

The full pipeline is documented in [`notebooks/od_oc_segmentation.ipynb`](notebooks/od_oc_segmentation.ipynb), covering:

- EDA on OD/OC mask statistics and spatial distribution
- Channel analysis of fundus images
- OD localization, segmentation, and post-processing
- OC segmentation experiments (Otsu, K-means K=3, K=4)
- Per-image F1 evaluation

---

## Dependencies

```
numpy
Pillow
opencv-python
scikit-image
matplotlib
scipy
```

---

## Dataset

[Drishti-GS](https://cvit.iiit.ac.in/projects/mip/drishti-gs/mip-dataset2/Home.php) — 101 fundus images with expert-annotated OD and OC masks.

---

## Limitations

- OD localization iterates over the full image (height × width), making it computationally inefficient
- OC segmentation accuracy is limited by the classical approach; a deep learning method would likely improve results significantly
- Pipeline is tuned for Drishti-GS image characteristics and may not generalize out of the box
