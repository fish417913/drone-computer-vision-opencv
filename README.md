# Drone Computer Vision with OpenCV

Two advanced OpenCV projects exploring spatial and temporal analysis of drone imagery.

These projects use classical computer-vision methods to solve two common aerial-imagery problems:

1. **Feature-based registration of overlapping drone images**
2. **Ego-motion estimation and moving-object analysis in drone video**

The work emphasizes not only implementation, but also evaluation of model assumptions, failure modes, and the distinction between apparent image motion and real scene change.

---

## Project 1 — Drone Image Registration

**Notebook:**  
[`01_drone_image_registration.ipynb`](notebooks/01_drone_image_registration.ipynb)

### Objective

Register two overlapping drone photographs taken from slightly different viewpoints.

### Techniques

- SIFT keypoint detection
- 128-dimensional SIFT descriptors
- FLANN nearest-neighbor matching
- Lowe's ratio test
- RANSAC outlier rejection
- Homography estimation
- Perspective warping
- Reprojection-error analysis
- Spatial match-distribution analysis
- Overlap-aware residual comparison

### Key Results

- ~19,500 SIFT keypoints detected per image
- 369 matches survived Lowe's ratio test
- 232 RANSAC inliers
- 62.87% inlier ratio
- Mean reprojection error: 0.549 px
- Median reprojection error: 0.354 px

### Main Finding

The estimated homography aligned the approximately planar roadway extremely well, while forested regions exhibited substantial residual misalignment.

This demonstrated an important limitation of a single global homography: elevated 3D structures such as tree canopy introduce parallax and cannot always be represented accurately by one planar projective transformation.

---

## Project 2 — Drone Ego-Motion and Moving-Object Analysis

**Notebook:**  
[`02_drone_ego_motion_and_object_detection.ipynb`](notebooks/02_drone_ego_motion_and_object_detection.ipynb)

### Objective

Estimate residual drone-camera motion and isolate independently moving roadway traffic using optical flow.

### Techniques

- Shi-Tomasi feature detection
- Pyramidal Lucas-Kanade optical flow
- RANSAC partial-affine estimation
- Full-video ego-motion analysis
- Camera-motion compensation
- Farnebäck dense optical flow
- Adaptive residual-motion thresholding
- Morphological opening and closing
- Contour extraction
- Motion-region bounding boxes

### Key Results

Representative frame pair:

- 500 / 500 features successfully tracked
- 453 / 500 dominant-motion RANSAC inliers
- 90.6% inlier rate
- Mean dominant-motion displacement: 0.161 px
- Mean RANSAC-outlier displacement: 3.655 px

Across all 299 frame transitions:

- Mean horizontal motion: 0.0350 px
- Mean vertical motion: -0.0131 px
- Mean rotation: 0.002922°
- Median RANSAC inlier ratio: 94.8%

Residual dense optical flow:

- Median residual magnitude: 0.0867 px
- 95th percentile: 0.1926 px
- 99th percentile: 1.7347 px
- Maximum residual magnitude: 3.7897 px

Full-video candidate motion regions:

- Median: 16 regions/frame
- Mean: 15.90 regions/frame
- Range: 12–20 regions/frame

### Main Finding

The drone footage was already highly stabilized. Rather than forcing an unnecessary stabilization step, the estimated ego-motion was used to compensate residual camera movement before dense optical-flow analysis.

High residual-motion regions corresponded primarily to moving roadway vehicles, demonstrating that classical OpenCV methods can isolate independently moving traffic without a trained object-detection model.

---

## Technical Takeaways

These projects demonstrate several recurring computer-vision patterns:

- Feature correspondence can be used to estimate geometric relationships between camera views.
- Robust estimation with RANSAC is useful for separating dominant scene geometry from inconsistent observations.
- Low reprojection error does not guarantee globally accurate registration when feature matches are spatially concentrated.
- Apparent image motion must be separated from camera ego-motion before interpreting motion as real object movement.
- Classical computer-vision methods remain useful for interpretable, geometry-driven drone-analysis workflows.

---

## Tools

- Python
- OpenCV
- NumPy
- Matplotlib
- Google Colab

---

## Repository Structure

```text
drone-computer-vision-opencv/
├── README.md
└── notebooks/
    ├── 01_drone_image_registration.ipynb
    └── 02_drone_ego_motion_and_object_detection.ipynb
