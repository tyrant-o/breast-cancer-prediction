# 🩺 Breast Cancer Prediction (IDC Classification)

Predicting **Invasive Ductal Carcinoma (IDC)** in histopathology tissue patches using **Deep Learning**
(Final accuracy: **86.05%** on unseen patient test set)

---

## 📌 Overview

Invasive Ductal Carcinoma accounts for nearly **80% of all breast cancer cases**, making early detection essential.
This project builds a **deep learning pipeline** to classify histopathology image patches as:

* **0 — Benign (Healthy Tissue)**
* **1 — IDC-Positive (Cancerous Tissue)**

Using a **ResNet18** model with transfer learning, weighted loss, and cyclical learning rate scheduling, this work demonstrates how AI can support pathologists by reducing manual workload and improving diagnostic consistency.

---

## 🚀 Key Features

* ✔️ **Patient-level dataset splitting** to avoid data leakage
* ✔️ **ResNet18 transfer learning** with a custom classification head
* ✔️ **Weighted Cross-Entropy Loss** for class imbalance
* ✔️ **Cyclical Learning Rate (CLR)** scheduling
* ✔️ **Extensive preprocessing + augmentation**
* ✔️ **286k histopathology patches** processed (50×50 RGB images)
* ✔️ **86.05% test accuracy** on unseen patients

---

## 📂 Dataset

**Source:** Breast Histopathology Images (Kaggle)

### Structure:

```
/patient_id/
    ├── 0/  # Benign patches
    └── 1/  # IDC-positive patches
```

### Stats:

* **Total patches:** 277,524
* **Class distribution:**

  * 198,738 benign
  * 78,786 IDC-positive
* **Patients:** 280
* **Patch size:** 50×50 RGB

> ⚠️ Highly imbalanced dataset — handled using class weighting.

---

## 🛠️ Methodology

### 1️⃣ Data Preprocessing

* Resize to 50×50 px
* Normalize with ImageNet statistics
* Data augmentation:

  * Random horizontal flip
  * Random vertical flip

---

### 2️⃣ Patient-Level Split (No leakage)

```
70% Train  
15% Validation  
15% Test  
```

All patches from a patient stay inside the same split.

---

### 3️⃣ Model Architecture

Using **ResNet18 (pre-trained on ImageNet)** as feature extractor.

🧠 **Custom classification head:**

* Linear → ReLU → BatchNorm → Dropout (0.5)
* Linear → ReLU → BatchNorm → Dropout (0.5)
* Final Linear → 2 output classes

---

### 4️⃣ Handling Imbalance

Used **compute_class_weight('balanced')** to calculate weights for weighted cross-entropy.

---

### 5️⃣ Optimization

* **Optimizer:** SGD
* **Learning Rate Scheduler:** CyclicLR

  * Helps escape local minima
  * Improves convergence

---

## 📊 Model Performance

### ✔️ Fast, stable convergence

Even after **1 epoch**, the model showed strong learning patterns.
Training for 30 epochs (recommended) yields smoother convergence and higher accuracy.

### ✔️ Final Test Set Accuracy: **86.05%**

Evaluated on unseen patient patches.

### ✔️ Loss Curves

Loss decreased consistently across training, validation, and testing.

---

## 🧠 Insights

* **Patient-level splitting** is crucial — otherwise model overfits.
* **Weighted loss + CLR** significantly boosts performance on minority class.
* The model generalizes well despite dataset imbalance.
* Patch-based classification works, but loses spatial context of whole slides.

---

## 🏁 Project Assessment

### Strengths:

* High accuracy
* Strong generalization
* Robust pipeline (augmentation, splitting, weighting, CLR)
* Applicable as a **decision-support system** for pathologists

### Limitations:

* Dataset sourced from a **single institution**
* Patch classification loses tumor shape/spatial reasoning
* Only 280 patient samples

---

## 🔮 Future Work

* 🗂️ **Use larger, multi-institution datasets**
* 🖼️ **Whole Slide Image (WSI) analysis** instead of patch-based
* ⚡ **Try modern architectures** like EfficientNet or Vision Transformers
* 🔍 **Add explainability** using Grad-CAM
* 🎯 **Hyperparameter tuning + longer training (30+ epochs)**

---

## 🧾 Conclusion

This project successfully demonstrates the potential of deep learning to classify IDC in breast tissue patches.
With **ResNet18**, balanced loss, and a smart optimization setup, the model achieved **86.05% accuracy** on unseen patients — showing how AI can speed up screening and support pathologists in real clinical settings.

Further improvements (WSI analysis, larger datasets, explainability) could move this from a proof-of-concept to a deployable clinical tool.
