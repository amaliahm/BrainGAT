# 🧠 BrainGAT

Graph Attention Network Approach for Multi-Class Brain Tumor Classification, a research project exploring the use of Graph Attention Networks (GATs) for multi-class brain tumor classification using MRI images. It is based on the paper *“Multi-class Brain Tumor Segmentation using Graph Attention Network”* and leverages the Brain Tumor MRI Dataset from Kaggle.

---

### 📚 Table of Contents

```
- Overview
- Dataset
- Methodology
- Model Architecture
- Training Process
- Results
- How to Run
- References
```

### 🧩 Overview

> Brain tumor classification is a critical task in medical image analysis. Traditional CNNs fail to capture long-range spatial dependencies. This project investigates how Graph Attention Networks (GATs) can represent pixel/region relationships as a graph to enhance segmentation performance.

---

### 📊 Dataset 

**Dataset:** [Brain Tumor MRI Dataset (Kaggle)](https://www.kaggle.com/datasets/masoudnickparvar/brain-tumor-mri-dataset).

It's a combination of those three datasets: figshare, SARTAJ dataset and Br35H.

Contains MRI scans categorized into 4 classes: glioma, meningioma, pituitary tumor, and healthy brain.

Preprocessing steps: resizing, normalization, graph construction from pixel regions or superpixels.

---

### ⚙️ Methodology
* Preprocess MRI scans into graph structures (nodes = regions, edges = spatial proximity).
* Use Graph Attention Networks (GAT) for feature propagation.
* Apply Softmax for multi-class classification output.
* Evaluation.
---

### 🧠 Model Architecture
```
Input: Graph with 80 nodes × 25 features
   ↓
[GAT Layer 1] - Multi-head attention (6 heads)
   • Learns which neighboring regions are important
   • Output: 80 nodes × (128 × 6) = 768 features
   ↓
[Batch Norm + ELU + Dropout]
   ↓
[GAT Layer 2] - Multi-head attention (6 heads)
   • Refines attention patterns
   • Output: 80 nodes × 768 features
   ↓
[GAT Layer 3] - Single-head attention
   • Final attention refinement
   • Output: 80 nodes × 128 features
   ↓
[Global Pooling] - Aggregate nodes to graph level
   • Mean pooling: average all 80 nodes → 128 features
   • Max pooling: max across all 80 nodes → 128 features
   • Concatenate: 256 features
   ↓
[Classification Head] - FC layers
   • FC1: 256 → 128
   • FC2: 128 → 4 (final classes)
   ↓
Output: [Glioma, Meningioma, No Tumor, Pituitary]
```
---

### 💻 Training Process
* For each epoch:

```
Forward Pass:
   Batch of graphs → GAT → Predictions
Loss Calculation:
   Predicted class vs True class → NLL Loss
Backward Pass:
   Compute gradients → Clip gradients → Update weights
Validation:
   Evaluate on validation set → Check accuracy
Early Stopping:
   - If validation accuracy doesn't improve for 15 epochs → Stop
   - Save best model when validation accuracy improves
```
---

### 📈 Results
- **Test Accuracy**: 96.43%
- **Best classes**: No Tumor, Pituitary (easier to distinguish)
- **Challenging**: Glioma vs Meningioma (similar patterns)
---

### 🔬 References

> * Original paper: *“Multi-class Brain Tumor Segmentation using Graph Attention Network”*
> * Dataset: *Brain MRI Images Dataset – Kaggle*
> * Framework: *PyTorch Geometric Documentation*