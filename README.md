# Exploring Hybrid 3D Reconstruction: A Comparative Study of Photogrammetry and Gaussian Splatting

## 1. Introduction

Modern 3D reconstruction demands both geometric precision and photo-realistic rendering. This study evaluates a hybrid workflow that leverages traditional photogrammetry and state-of-the-art neural rendering via Gaussian Splatting. Our objective is to analyze how each technique performs individually and when combined, particularly in the context of a lunar module scene.

## 2. Project Overview

* **Primary Goal:** Evaluate how augmenting traditional photogrammetry with neural-synthesized views enhances 3D reconstruction quality.
* **Dataset:** 10 original images captured from various angles around a simulated lunar module.
* **Approach:**

  * Stage 1: Generate a mesh using only the original images via Agisoft Metashape.
  * Stage 2: Use Gaussian Splatting to synthesize new viewpoints and re-run photogrammetry with the augmented image set.

## 3. Tools and Infrastructure

| Component             | Details                                |
| --------------------- | -------------------------------------- |
| Operating System      | Ubuntu 22.04 LTS                       |
| Photogrammetry Tool   | Agisoft Metashape Professional         |
| Rendering Framework   | Gaussian Splatting (Inria, 2023)       |
| Languages & Libraries | Python 3.10, PyTorch, OpenCV, LPIPS    |
| Hardware              | NVIDIA RTX 4060 GPU, CUDA Toolkit 12.1 |

## 4. Assignment Breakdown: Task A and Task B

### Task A: Photogrammetric Reconstruction Using Metashape

This task aimed to generate a 3D mesh from the original set of 10 images using Agisoft Metashape.

**Step 1: Image Import and Alignment**
The 10 images were imported into a new Metashape project. Using the 'Align Photos' feature, a sparse point cloud was generated. This alignment established the initial 3D spatial relationships between the images.

**Step 2: Dense Cloud Generation**
With the sparse cloud complete, a dense point cloud was created using high-quality settings. Point confidence metrics were enabled to assess the reconstruction density and accuracy.

**Step 3: Mesh and Texture Construction**
The mesh was generated from the dense cloud using an arbitrary surface type. A texture was then mapped onto the mesh using mosaic blending and a 4096 x 4096 resolution.

**Step 4: Export for Evaluation**
The textured mesh was exported for quality comparison and future reference.

### Task B: Synthetic View Generation and Augmented Reconstruction

The goal of Task B was to generate novel views using Gaussian Splatting and incorporate them into a second photogrammetric workflow.

**Step 1: Training a Gaussian Splatting Model**
The Gaussian Splatting framework was trained on the 10 original images. This model learned a volumetric representation of the scene and was used to render synthetic views from novel angles.

**Step 2: Novel View Selection**
Out of the generated outputs, 10 novel views were selected. These were chosen to fill gaps and improve coverage in areas that were occluded or poorly reconstructed in Task A.

**Step 3: Dataset Augmentation**
The novel views were added to the original dataset, forming a combined set of 25 images. This expanded dataset offered improved viewpoint diversity.

**Step 4: Second Reconstruction in Metashape**
Photogrammetry was re-run in Metashape using the 25 images. The workflow mirrored Task A, but the output mesh showed improvements in geometric accuracy and completeness due to the augmented input.

## 5. Quality Evaluation Metrics

### 5.1 Image Similarity Assessment

Rendered images were compared to original views using two metrics:

* **PSNR**: Indicates signal fidelity; higher values imply less distortion.
* **SSIM**: Measures structural similarity; closer to 1 means better visual match.

| Original    | Rendered  | PSNR | SSIM   |
| ----------- | --------- | ---- | ------ |
| A17\_01.png | pic1.png  | 7.27 | 0.0702 |
| A17\_02.png | pic10.png | 6.23 | 0.0431 |
| A17\_03.png | pic2.png  | 7.43 | 0.1133 |
| A17\_04.png | pic3.png  | 7.40 | 0.1133 |
| A17\_05.png | pic4.png  | 8.72 | 0.1535 |
| A17\_06.png | pic5.png  | 8.14 | 0.1296 |
| A17\_07.png | pic6.png  | 9.21 | 0.2261 |
| A17\_08.png | pic7.png  | 8.50 | 0.2276 |
| A17\_09.png | pic8.png  | 8.26 | 0.1574 |
| A17\_10.png | pic9.png  | 8.38 | 0.1520 |

### 5.2 Mesh Comparison (15 vs 25 Images)

| Metric          | Base Model (15 Views) | Augmented Model (25 Views) |
| --------------- | --------------------- | -------------------------- |
| Vertices        | 264,859               | 258,098                    |
| Faces           | 480,491               | 517,422                    |
| Bounding Volume | 66.59                 | 135.98                     |

The augmented model showed a significant increase in bounding volume, suggesting improved spatial coverage. The number of faces also increased, indicating greater mesh complexity and completeness.

## 6. Observations

* SSIM values remained modest, indicating that while structural similarity improved slightly, fine detail preservation is still a challenge.
* Novel views enhanced surface coverage and geometry definition.
* Mesh results demonstrated broader bounding volume and increased triangle count in the augmented case.

## 7. Reflections & Lessons Learned

This hybrid approach showed tangible benefits:

* Neural views enhance visibility into occluded regions.
* Photogrammetry is highly sensitive to camera placement diversity.

Challenges included handling incomplete reconstructions when neural renders lacked sufficient sharpness or realism.

## 8. Final Verdict and Recommendations

Augmenting traditional 3D reconstruction with synthesized views presents a clear advantage in mesh completeness and coverage. Gaussian Splatting offers promising support in enriching datasets, although improvements in rendering fidelity would further enhance results.

**Future Directions:**

* Explore denser angular sampling to reduce occlusions.
* Refine rendering pipelines for sharper novel views.
* Apply the workflow to more complex real-world environments.

## 9. Project Timeline

* **Image Capture & Preprocessing:** 1 hour
* **Initial Reconstruction:** 1.5 hours
* **Model Training & Novel Views:** 2.5 hours
* **Final Reconstruction & Comparison:** 1 hour
* **Documentation & Analysis:** 1 hour

**Total Time:** \~7 hours

---

*Prepared by: Mudit Khandelwal and Team — May 10, 2025*
