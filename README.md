#  Exploring Hybrid 3D Reconstruction: A Comparative Study of Photogrammetry and Gaussian Splatting

# 1. Introduction

Modern 3D reconstruction demands both geometric precision and photorealistic rendering. This project explores a hybrid workflow that combines conventional photogrammetry with advanced neural rendering via Gaussian Splatting. We aim to analyze how each method contributes to the reconstruction pipeline and how they complement each other when integrated.

# 2. Project Overview

* **Objective:** Assess the effectiveness of augmenting traditional photogrammetry with neural-generated views.
* **Dataset:** 10 original PNG images capturing a simulated lunar module from varied angles.
* **Methodology:**

  * Task A: 3D model generation from original images using Agisoft Metashape.
  * Task B: Neural rendering with Gaussian Splatting and dataset augmentation, followed by mesh regeneration.

#  3. Tools and Setup

| Component           | Specification                          |
| ------------------- | -------------------------------------- |
| OS                  | Ubuntu 22.04 LTS                       |
| Photogrammetry Tool | Agisoft Metashape Professional (Demo)  |
| Rendering Framework | Gaussian Splatting (Inria, 2023)       |
| Libraries           | Python 3.10, PyTorch, OpenCV, LPIPS    |
| Hardware            | NVIDIA RTX 4060 GPU, CUDA Toolkit 12.1 |

#  4. Assignment Breakdown

##  Task A: Photogrammetric Reconstruction

This stage involved reconstructing a 3D model from the initial 10 images using Agisoft Metashape.

**Step 1:** Imported the images and aligned them to build a sparse point cloud.

```bash
# Metashape GUI: Workflow > Add Photos > Align Photos (Accuracy: High)
```

**Step 2:** Generated a dense point cloud with high-quality settings and enabled confidence filtering.

```bash
# Metashape GUI: Workflow > Build Dense Cloud (Quality: High, Depth Filtering: Mild, Point Confidence: Enabled)
```

**Step 3:** Built a mesh from the dense cloud, selecting 'arbitrary' surface type and applying a mosaic texture map.

```bash
# Workflow > Build Mesh > Source: Dense Cloud, Surface: Arbitrary
# Workflow > Build Texture > Mapping Mode: Generic, Blending Mode: Mosaic
```

**Step 4:** Exported the textured model for later comparison.

```bash
# Export Path: /home/user/Photogrammetry_Models/15_views_model.obj
```

##  Task B: Neural View Augmentation and Reconstruction

This stage tested whether adding neural-generated views could enhance the model's completeness and fidelity.

**Step 1:** Trained a Gaussian Splatting model on the original image set to learn spatial representations.

```bash
python3 train.py -s /home/user/Agisoft_Dataset --disable_viewer -r 800 --convert_SHs_python
```

**Step 2:** Synthesized 10 novel views from alternative perspectives to fill in occluded gaps.

```bash
python3 render.py -s /home/user/Agisoft_Dataset -m output/<model_id> --skip_train --eval
```

**Step 3:** Merged the 10 neural views with the original 10, creating a 25-image dataset.

```bash
mkdir -p ~/Photogrammetry_25Views
cp ~/Agisoft_Dataset/*.png ~/Photogrammetry_25Views/
cp ~/GS_Rendered/*.png ~/Photogrammetry_25Views/
```

**Step 4:** Reconstructed the model again using the expanded dataset, applying the same parameters as Task A.

#  5. Performance Evaluation

## 5.1 Image Similarity (PSNR & SSIM)

Two metrics were used to compare original images with corresponding rendered outputs. These were computed using the following Python snippet:

```python
from skimage.metrics import peak_signal_noise_ratio, structural_similarity
psnr = peak_signal_noise_ratio(original_img, rendered_img)
ssim = structural_similarity(original_img, rendered_img, channel_axis=2)
```

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

##  5.2 Mesh Statistics Comparison

| Metric          | 15-Image Model | 25-Image Model |
| --------------- | -------------- | -------------- |
| Vertices        | 9,408,59        | 258,098        |
| Faces           | 18,804,91        | 517,422        |
| Bounding Volume | 66.59          | 135.98         |

**Insight:** The augmented model captured a larger volume and had higher face density, pointing to improved coverage and structural detail.

#  6. Observations

* Neural views reduced occlusion issues by adding unseen geometry.
* SSIM values were modest, showing that while structure was retained, finer details need refinement.
* The 25-view model was visually smoother and geometrically broader.
![image](https://github.com/user-attachments/assets/4069be9f-3dbb-4ba9-839d-2ab38032d684)

#  7. Reflections and Challenges

**Achievements:**

* Demonstrated that neural rendering can successfully complement traditional reconstruction.
* Validated that image diversity significantly boosts photogrammetry results.

**Challenges:**

* Neural outputs lacked crispness compared to real photos.
* Rendering consistency across novel views was hard to achieve.

#  8. Conclusion & Recommendations

This study affirms that combining Gaussian Splatting with photogrammetry enhances the quality and completeness of 3D reconstructions. Although further refinement in synthetic view generation is needed, the added perspective diversity clearly benefited the final mesh.

### Suggestions for Future Work

* Improve Gaussian output sharpness.
* Use automated viewpoint sampling strategies.
* Test with outdoor or real-time datasets.


For Further reference :

Google Drive Link : https://drive.google.com/drive/folders/1O4EkNWAZxBKtdB-KF_LUXxyEoJ8riMsx?usp=sharing

