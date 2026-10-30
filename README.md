# Automated Diabetic Retinopathy Severity Classification

**ResNet50 · Transfer Learning · Medical Image Classification · Grad-CAM · TensorFlow**

A **CSE427 (Machine Learning)** project developed at **BRAC University** by **Pretom Areefin Pranto** and **Md. Farhad Hossain Shishir**.

This repository contains our **original Google Colab notebook**, saved experimental outputs, and project documentation for automated diabetic retinopathy (DR) severity classification from retinal fundus images. The implementation uses an ImageNet-pretrained ResNet50, two-stage transfer learning, class-weighted training, test-time augmentation, and model interpretation with Grad-CAM.

> **Educational/research project — not a medical device.** The model has not been clinically validated and must not be used for patient diagnosis or treatment decisions.

## Project overview

Diabetic retinopathy is a diabetes-related retinal condition that can affect vision. This project investigates whether a convolutional neural network can categorize its severity using retinal fundus images, while addressing class imbalance and showing which image regions influence predictions.

### Highlights

- **Dataset:** 3,469 processed fundus images from the **APTOS 2019 Blindness Detection** dataset.
- **Task:** Four-class severity classification — **No DR**, **Mild**, **Moderate**, and **Proliferative DR**.
- **Split:** Stratified image-level **80% training / 10% validation / 10% testing** (2,775 / 347 / 347 images; seed 42).
- **Model:** ImageNet-pretrained **ResNet50** with a custom classification head; frozen-backbone training followed by partial fine-tuning.
- **Training:** Augmentation and **class-weighted sparse categorical cross-entropy**, with checkpoint selection using validation quadratic weighted kappa (QWK).
- **Evaluation:** Accuracy, macro-F1, balanced accuracy, QWK, class-wise metrics, and horizontal-flip test-time augmentation (TTA).
- **Interpretability:** **Grad-CAM** visualizations for examining image regions influencing model output.

**Important scope note:** APTOS 2019 originally contains **five** labels. Our notebook removes the original *Severe DR* label (class 3) and remaps *Proliferative DR* (class 4) to class 3. The reported performance applies to **four classes**, not all five APTOS categories.

## Reported held-out test results

These numbers are taken from the **saved outputs in the submitted Colab notebook**. They have **not been independently rerun** in a new environment.

| Metric | Result |
| --- | ---: |
| Accuracy (without TTA) | 78.67% |
| **Accuracy (with TTA)** | **81.84%** |
| Balanced accuracy | 68.97% |
| **Macro-F1** | **68.83%** |
| Weighted F1 | 81.75% |
| **Quadratic weighted kappa** | **0.8975** |
| Ordinal mean absolute error | 0.1988 |
| Within-one-grade accuracy | 98.27% |
| Held-out test images | 347 |

The class imbalance remains important: No DR has stronger per-class performance than the minority Mild DR and Proliferative DR categories. A high overall accuracy should therefore not be read as equivalent performance for every severity class.

## Experimental visualizations

**Class distribution**

![APTOS four-class distribution](assets/class_distribution.png)

**Training and validation metrics**

![ResNet50 training curves](assets/training_curves.png)

**Held-out test confusion matrix**

![Four-class confusion matrix](assets/confusion_matrix.png)

The figures above were extracted from the notebook's saved outputs; other visualizations, including Grad-CAM examples, are available in the original notebook.

## Repository structure

```text
.
├── README.md
├── requirements.txt
├── .gitignore
├── GITHUB_UPLOAD_GUIDE.md
├── notebooks/
│   └── Automated_Diabetic_Retinopathy_Severity_Classification_Using_Retinal_Fundus_Images_By_Pretom_Areefin_Pranto_and_MD_Farhad_Hossain_Shishir.ipynb
├── assets/
│   ├── class_distribution.png
│   ├── training_curves.png
│   └── confusion_matrix.png
└── slides/
    └── README.md                 # Instructions for adding the presentation later
```

**The project paper is intentionally not included in this repository.** The presentation slides have not yet been supplied for inclusion.

## Running the notebook in Google Colab

[Open our original Colab notebook](https://colab.research.google.com/drive/1NGHfT5_-OZMIkW-h-oPCZcpi7QmuNWh5?usp=sharing)

1. Open the Colab link above or upload the `.ipynb` file under [`notebooks/`](notebooks/) to Colab. Select a GPU runtime if available.
2. Obtain the [APTOS 2019 dataset from Kaggle](https://www.kaggle.com/c/aptos2019-blindness-detection/data), subject to the dataset's terms. The notebook expects a **prepared 224 × 224 image archive**, not necessarily the raw Kaggle download. Within `archive.zip`, the expected files include `train.csv` (columns `id_code`, `diagnosis`) and `colored_images/`.
3. Place the prepared `archive.zip` in a Google Drive folder accessible from the notebook.
4. **Update `PROJECT_DIR`** inside the unchanged notebook to point to *your* Drive folder. It is set to the original project author's folder in both the dataset setup cell and the later Grad-CAM/recovery cell. This is a necessary local configuration step — the repository copy itself has not been edited.
5. Run the notebook cells sequentially. It extracts the archive, copies the dataset into local Colab storage, trains the model, evaluates the held-out test set, and saves artifacts to the configured Drive project folder.

**Reproducibility note:** Dataset preparation, GPU availability, package versions and model-training randomness can affect reruns. The model weights and full dataset are not included in the repository. `requirements.txt` lists direct imports; it does not guarantee exact reproduction of the original Colab runtime.

## Technologies

`Python` · `TensorFlow / Keras` · `ResNet50` · `scikit-learn` · `NumPy` · `Pandas` · `OpenCV` · `Pillow` · `Matplotlib` · `Google Colab`

## Project presentation

The slide deck can be added under [`slides/`](slides/) as a `.pptx` or `.pdf` file. Instructions for adding it without changing the existing notebook are in [`slides/README.md`](slides/README.md). The paper is being kept private for now.

## Limitations

- The experimental pipeline evaluates **four classes**, excluding original Severe DR cases.
- Performance is uneven across classes because of imbalanced class frequencies.
- The supplied materials do not establish a patient-independent split or validation on an external clinical dataset.
- Grad-CAM visualizations are interpretability aids, **not** proof of clinically meaningful lesion localization.
- Original Colab code includes a project-specific Google Drive path; update it locally before executing in another environment.

## Contributors

- **Pretom Areefin Pranto** — BRAC University · [GitHub](https://github.com/pretom26)
- **Md. Farhad Hossain Shishir** — BRAC University

This is a **joint academic project**. No license is included by default; discuss open-source licensing and permissions with both contributors before applying one.
