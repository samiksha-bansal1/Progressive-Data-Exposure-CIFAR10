# 📊 Effect of Data Exposure Strategies on Deep Learning Performance (CIFAR-10)

## 🧠 Overview

In most deep learning pipelines, models are trained using the entire dataset from the very beginning. This project challenges that assumption by investigating:

> **How the order and amount of data exposure influence learning dynamics, convergence speed, and generalization performance.**

We systematically evaluate different **data exposure strategies** across multiple architectures using the CIFAR-10 dataset.

---

## 🎯 Objectives

* Study the impact of **data exposure strategies** on:

  * Model convergence speed
  * Final test accuracy
  * Generalization performance
* Compare sensitivity across architectures
* Ensure **fair, reproducible experimental design**

---

## 🧪 Experimental Setup

### 📦 Dataset

* **CIFAR-10**

  * 60,000 images (32×32 RGB)
  * 10 classes
* Split:

  * 40,000 → Training
  * 10,000 → Validation
  * 10,000 → Test
* Stratified splitting ensures class balance

---

### 🧠 Architectures

| Model             | Description                                                                |
| ----------------- | -------------------------------------------------------------------------- |
| **VGG-style CNN** | Deep network without skip connections; sensitive to gradient flow          |
| **ResNet**        | Residual connections improve stability and gradient propagation            |
| **MobileNet**     | Lightweight architecture with fewer parameters and implicit regularization |

---

### 🔁 Training Strategies

#### 1. Full Dataset Training (Baseline)

* Entire dataset used from the start

#### 2. Progressive Dataset Revelation

* Data introduced in stages:

  ```
  10% → 25% → 50% → 75% → 100%
  ```
* Maintains class balance at each stage

#### 3. Curriculum Learning

* Samples ordered from **easy → hard**
* Difficulty-based training progression

---

## ⚙️ Reproducibility

* Fixed random seeds (`torch`, `numpy`, CUDA)
* Deterministic CuDNN settings
* Controlled DataLoader randomness
* Identical hyperparameters across all experiments

---

## 🏗️ Project Structure

```
├── progressive_revelation_cifar10.ipynb   # Main experiment notebook
├── results/                               # Saved outputs and plots
├── README.md                              # Documentation
```

---

## 🚀 How to Run

```bash
git clone https://github.com/your-username/data-exposure-cifar10.git
cd data-exposure-cifar10

pip install torch torchvision matplotlib numpy scikit-learn

jupyter notebook progressive_revelation_cifar10.ipynb
```

---

## 📈 Evaluation Metrics

* Training Accuracy
* Validation Accuracy
* Test Accuracy
* Loss Curves
* Generalization Gap

---

## 📊 Results

### 🔢 Final Test Accuracy

| Model         | Full Dataset (%) | Progressive (%) | Curriculum (%) |
| ------------- | ---------------- | --------------- | -------------- |
| **VGG**       | **86.18**        | 75.90           | 75.12          |
| **ResNet**    | **91.31**        | 89.50           | 90.50          |
| **MobileNet** | **86.25**        | 81.39           | 80.43          |

---

### ⚡ Convergence Speed

(Epoch to reach ~70% validation accuracy)

| Model         | Full  | Progressive | Curriculum |
| ------------- | ----- | ----------- | ---------- |
| **VGG**       | 9     | 23          | 24         |
| **ResNet**    | **3** | 12          | 11         |
| **MobileNet** | 5     | 15          | 15         |

---

### 📉 Generalization Gap (Train − Validation)

| Model         | Full  | Progressive | Curriculum |
| ------------- | ----- | ----------- | ---------- |
| **VGG**       | -3.62 | -4.40       | -5.49      |
| **ResNet**    | 6.74  | 5.97        | **0.27**   |
| **MobileNet** | 6.42  | 2.63        | **-0.97**  |

---

## 🔍 Key Insights

### ✅ 1. Full Dataset Training Performs Best Overall

* Highest accuracy across all architectures
* ResNet achieves the best performance: **91.31%**

---

### ⚠️ 2. Progressive & Curriculum Hurt VGG

* ~10% drop in accuracy
* Very slow convergence

**Reason:**
Lack of skip connections makes VGG highly sensitive to limited early data.

---

### 🔥 3. ResNet is Most Robust

* Maintains strong performance across all strategies
* Curriculum learning:

  * Near-baseline accuracy
  * **Almost zero generalization gap (0.27%)**

---

### ⚖️ 4. MobileNet Shows Moderate Sensitivity

* Performance decreases under staged strategies
* However, generalization improves slightly

---

### 🐢 5. Staged Learning Slows Down Training

* 2×–5× slower convergence across all models

---

## 🧠 Final Conclusion

* **Best Accuracy:** Full Dataset Training
* **Best Generalization:** Curriculum Learning (ResNet)
* **Most Sensitive Model:** VGG
* **Most Stable Model:** ResNet

> The effectiveness of data exposure strategies is **highly dependent on model architecture**

---

## ⚠️ Challenges

* Designing a fair curriculum scoring method
* Maintaining class balance during progressive stages
* Eliminating randomness and bias

---

## 🔮 Future Work

* Scale experiments to larger datasets (ImageNet)
* Adaptive curriculum learning strategies
* Hybrid approaches (progressive + curriculum)
* Extend to transformer-based architectures

---

## 📁 Outputs

* `summary_results.csv` → Final metrics
* `full_training_history.csv` → Epoch-wise logs
* `all_9_loss_accuracy_curves.png` → Training curves

---

## 📚 References

* CIFAR-10 Dataset (Krizhevsky, 2009)
* Deep Residual Learning (He et al., 2016)
* MobileNet (Howard et al., 2017)
* Curriculum Learning (Bengio et al., 2009)


---

## ⭐ If you found this project useful, consider giving it a star!
