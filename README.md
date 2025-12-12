# IHC Image Analysis Using HED Deconvolution and CellPose-SAM

This repository provides a **quantitative analysis pipeline for immunohistochemistry (IHC) images** stained with **Hematoxylin and DAB**, aiming to identify and quantify **DAB-positive cells** at the single-cell level.

The workflow combines **color deconvolution**, **deep-learning-based nucleus segmentation**, and **threshold validation across positive and negative slides** to ensure robustness and biological plausibility.

------

## 📌 Project Overview

- Each IHC image contains **two informative channels**:
  - **Hematoxylin (H)**: nuclear staining
  - **DAB**: target molecule (Molecule of Interest, MOI)
- Input images are **RGB TIFF files**
- Core objectives:
  - Objectively determine a **DAB positivity threshold**
  - Assign DAB positivity at the **cell level**
  - Validate the threshold on **negative and multiple positive slides**

------

## 🧠 Analysis Workflow

1. **Image Loading**
   - Read RGB TIFF images
2. **Color Deconvolution**
   - Convert RGB images into **HED color space**
   - Extract **H (Hematoxylin)** and **DAB** channels
3. **H Channel Preprocessing**
   - Normalize the H channel to the range `[0, 1]`
4. **Nucleus Segmentation**
   - Perform nucleus segmentation using the **CellPose-SAM model**
   - Segmentation is driven by the normalized H channel
5. **DAB Positivity Assignment**
   - Apply a threshold on the DAB channel
   - Identify DAB-positive regions
   - Determine which nuclei overlap with these regions
   - Label overlapping nuclei as **DAB-positive cells**
   - Compute the proportion of DAB-positive cells

------

## 📂 Notebooks Description

The repository contains three Jupyter notebooks corresponding to different stages of the analysis:

### `00_Positive_Slide_Choose_A_Threshold.ipynb`

**Purpose: Determine the DAB positivity threshold**

- Select a **clearly positive IHC slide**
- Apply **Otsu’s method** on the DAB channel to automatically determine a threshold
- Use this threshold to:
  - Segment DAB-positive regions
  - Identify nuclei overlapping with these regions
  - Label them as DAB-positive cells
- Quantify:
  - Total number of nuclei
  - Number of DAB-positive cells
  - Percentage of DAB-positive cells

------

### `01_Negative_Slide_Threshold_Examination.ipynb`

**Purpose: Validate the threshold on negative slides**

- Apply the threshold determined in `00` to **multiple negative slides**
- Expected outcome:
  - Almost no DAB-positive regions
  - Almost no DAB-positive cells
- This step verifies that the threshold does not introduce **false positives** in negative controls

------

### `02_Positive_Slide_Threshold_Examination.ipynb`

**Purpose: Validate the threshold on additional positive slides**

- Apply the same threshold to **multiple independent positive slides**
- Evaluate:
  - Whether DAB-positive regions are biologically reasonable
  - Whether the fraction of DAB-positive cells is consistent
- Confirms the **robustness and generalizability** of the selected threshold

------

## 📊 Outputs

- Visualization of:
  - DAB-positive regions
  - Nucleus segmentation masks
  - DAB-positive vs. DAB-negative cells
- Quantitative metrics per slide:
  - Total cell count
  - DAB-positive cell count
  - Percentage of DAB-positive cells

------

## ⚠️ Notes and Considerations

- The Otsu-derived threshold may depend on:
  - Staining quality
  - Scanner brightness and contrast
- Recommended practice:
  - Determine the threshold using a **representative positive slide**
  - Validate using **negative controls and multiple positive slides**
- Segmentation quality may be influenced by:
  - Tissue density
  - Nuclear morphology
  - Image resolution

------

## 📜 License

This project is intended for **research and methodological validation only**.
For clinical or commercial use, ensure appropriate validation and regulatory compliance.