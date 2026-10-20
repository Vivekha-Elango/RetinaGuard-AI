# RetinaGuard AI — Diabetic Retinopathy Severity Assessment

## Overview

RetinaGuard AI is a quality-aware, uncertainty-aware deep learning pipeline for diabetic retinopathy (DR) severity assessment. It does not just predict DR grade — it also checks image quality and flags uncertain predictions for human review.

---

## Pipeline

Retinal Image → Quality Check → DR Severity Prediction → Uncertainty Check → Grad-CAM → Doctor Review

### Steps

1. **Image Quality Assessment** — OpenCV-based blur and brightness check. Poor-quality images are referred for recapture before any prediction is made.
2. **DR Severity Classification** — EfficientNet-B0 fine-tuned on APTOS 2019. Predicts five grades (0–4).
3. **Uncertainty Estimation** — Monte Carlo Dropout (30 passes). High uncertainty triggers human referral.
4. **Explainability** — Grad-CAM heatmap highlights retinal regions that influenced the prediction.

---

## DR Grades

| Grade | Label | Description |
|---|---|---|
| 0 | No DR | No diabetic retinopathy detected |
| 1 | Mild | Mild diabetic retinopathy |
| 2 | Moderate | Moderate diabetic retinopathy |
| 3 | Severe | Severe diabetic retinopathy |
| 4 | Proliferative | Proliferative diabetic retinopathy |

---

## Results (APTOS 2019)

**Metrics must be verified against the final evaluation output before publication.**

| Metric | Value |
|---|---|
| Accuracy | To be confirmed |
| Macro F1 | To be confirmed |
| No DR F1 | To be confirmed |
| Quality Referred | To be confirmed |

---

## Tech Stack

- Python
- PyTorch
- EfficientNet-B0 (`timm`)
- OpenCV
- Monte Carlo Dropout
- Grad-CAM
- Streamlit demo (included in notebook; verify before use)

---

## Dataset

**APTOS 2019 Blindness Detection**

- Five diabetic retinopathy severity grades
- Source: [APTOS 2019 dataset on Kaggle](https://www.kaggle.com/datasets/mariaherrerot/aptos2019)

The dataset is not included in this repository. Follow the dataset's access and usage terms.

---

## Project Structure

```text
RetinaGuard-AI/
├── .gitignore
├── README.md
├── RetinaGuard_train.ipynb
├── models/                 # Add trained weights separately if available
├── data/                   # Dataset, not committed to GitHub
└── outputs/                # Evaluation outputs, if available
```

A testing notebook can be added when it is available.

---

## How to Run

### Training

1. Open `RetinaGuard_train.ipynb` in Google Colab.
2. Enable a GPU if available under **Runtime → Change runtime type**.
3. Configure your Kaggle credentials securely. Never upload `kaggle.json` or API keys to GitHub.
4. Run the notebook cells in order, checking that each step completes successfully.
5. Confirm the location where the trained model weights are saved.

### Testing

1. Obtain the trained `efficientnet_retina.pth` model weights.
2. Open the testing notebook, when available, in Google Colab.
3. Update the model path to match the location of the weights.
4. Run the evaluation or inference cells and review the output.

---

## Referral Logic

The following thresholds describe the intended pipeline configuration. Verify them against the final notebook before relying on them.

| Condition | Intended action |
|---|---|
| Blur score below 10 | Refer for image-quality review |
| Brightness below 20 or above 230 | Refer for image-quality review |
| Uncertainty above 0.01 | Refer for human review |
| Predicted Grade 2 or higher | Recommend ophthalmologist review |
| Grade 0 or 1 with low uncertainty | Review according to the screening protocol |

---

## Disclaimer

RetinaGuard AI is a research prototype for screening support only. It is not a clinically validated diagnostic device and must not replace professional medical assessment. Always consult a qualified ophthalmologist for diagnosis and treatment decisions.

---

## Authors

Developed as part of the RetinaGuard AI project.
