# A-Hybrid-Multi-Factor-Framework-for-Interpretable-Dysgraphia-Screening-from-Handwriting-Dataset
Dysgraphia diagnosis relies on clinical observation or deep learning. Our Hybrid Multi-Factor Framework combines both via script segmentation, CNN character analysis, document-level geometric analysis, and a decision tree. Overcoming a CNN-only failure rate of 37.1%, this hybrid model achieves 95.0% accuracy and 97.2% precision on handwriting data.

# Overview
Dysgraphia is a learning disorder affecting handwriting legibility and fine motor control, yet it remains significantly underdiagnosed. This project proposes a Hybrid Multi-Factor Diagnostic Framework that goes beyond black-box deep learning by modeling dysgraphia as a motor control disorder rather than a simple visual classification task.
The framework combines:

Deep learning (character-level CNN) for morphological analysis
Geometric feature extraction for motor stability assessment
A clinically-inspired decision logic (Steady Hand Safety Net) to suppress false positives


# System Architecture

```
Input Handwriting Image
        │
        ▼
┌─────────────────────┐
│  Preprocessing      │  → Grayscale, Auto-Invert, Gaussian Blur
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  Adaptive Segmenter │  → Binarization → Row Clustering → Aspect-Ratio Splitting
└──────┬──────────────┘
       │
  ┌────┴────┐
  ▼         ▼
Micro     Macro
Analytical Analytical
Module    Module
  │         │
  │  CNN    │  Jitter + Alignment + Spacing
  │         │  Analyzers
  └────┬────┘
       ▼
┌──────────────────────────┐
│  Steady Hand Safety Net  │  → Adjusts thresholds based on motor stability
└──────────────────────────┘
       ▼
Final Diagnosis: Normal / Dysgraphia + Confidence Score
```


# Key Components

### 1. Adaptive Segmentation Pipeline
- Handles both printed and cursive scripts
- Semantic row clustering (30px vertical tolerance)
- Aspect-ratio-based character splitting (AR > 2.2 triggers split)

### 2. Micro-Analytical Module (Attention-Guided CNN)
- Custom CNN with 3 convolutional blocks (16 → 32 → 64 filters)
- Spatial Attention Module (CBAM-inspired) to focus on ink strokes
- Trained on 96×96 character images
- **Character-level accuracy: 99.87%**

### 3. Macro-Analytical Module (Geometric Features)
| Analyzer | What it measures |
|---|---|
| **Jitter Analyzer** | Stroke irregularity via contour-to-convex-hull perimeter ratio |
| **Alignment Analyzer** | Baseline drift using 75th percentile row baseline |
| **Spatial Analyzer** | Spacing consistency via Coefficient of Variation |

### 4. Steady Hand Safety Net
A non-linear decision tree that dynamically adjusts diagnostic sensitivity:

- **Branch A (Steady, Jitter < 30%):** Flags dysgraphia only on catastrophic spatial errors (Spacing > 80% OR Alignment > 85%)
- **Branch B (Shaky, Jitter ≥ 30%):** Applies standard clinical thresholds across all features


# Dataset

- 📊 [Normal and Dysgraphia Character Level Dataset](https://www.kaggle.com/datasets/vibhushanabaskar/normal-dysgraphia-character)
- 📊 [Normal and Dysgraphia Sentence Level Dataset](https://www.kaggle.com/datasets/vibhushanabaskar/normal-dysgraphia-sentence)



## 🔬 Explainability

The framework provides a human-readable explanation for every diagnosis:

- **True Positive example:** Moderate form error but catastrophic spacing (>80%) → Dysgraphia correctly flagged
- **True Negative example:** Visually messy handwriting but low jitter (<30%) → Correctly classified as Normal (stylistic variation)

This mimics clinical reasoning: distinguishing **motor dysfunction** from **aesthetic messiness**.









