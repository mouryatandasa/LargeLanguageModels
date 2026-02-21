#  Fine-Tuning LLMs: Quantization & Modes

------------------------------------------------------------------------

## 1️.  Introduction to Quantization

Quantization is the process of reducing the numerical precision of model
parameters (weights and activations) to make Large Language Models
(LLMs) more memory and compute efficient.

Most LLMs are trained using:

-   **FP32 (32-bit floating point)** -- Full precision\
-   **FP16 (16-bit floating point)** -- Half precision

Quantization reduces precision further:

    FP32 → FP16 → INT8 → INT4

Lower precision leads to:

-   Reduced memory usage\
-   Faster inference\
-   Lower deployment cost\
-   Slight accuracy tradeoff

------------------------------------------------------------------------

## 2️. Why Quantization is Important

Example for a 7B parameter model:

-   FP32 → 7B × 4 bytes = **28 GB**
-   INT8 → 7B × 1 byte = **7 GB**

That is a **75% memory reduction**.

### Benefits

-   Deploy on mobile devices\
-   Deploy on edge devices\
-   Reduce GPU requirements\
-   Lower cloud cost\
-   Faster inference

------------------------------------------------------------------------

## 3️. Full Precision vs Half Precision

### FP32 (Full Precision)

-   32-bit floating point\
-   High numerical stability\
-   Used during training

### FP16 (Half Precision)

-   16-bit floating point\
-   Reduced memory\
-   Faster computation\
-   Used in mixed precision training

------------------------------------------------------------------------

## 4️. Quantization Mathematics

### Scale Formula

Scale = (Xmax - Xmin) / (Qmax - Qmin)

### Quantization Formula

Q = round(X / Scale) + ZeroPoint

Where:

-   X = original floating value\
-   Q = quantized integer\
-   ZeroPoint = offset (used in asymmetric quantization)

------------------------------------------------------------------------

## 5️. Types of Quantization

### 1️. Symmetric Quantization

-   Range centered at zero\
-   ZeroPoint = 0\
-   Uses signed int8

Example:

\[-1000, 1000\] → \[-128, 127\]

✔ Simple\
✔ Faster\
✔ Best for weights

------------------------------------------------------------------------

### 2️. Asymmetric Quantization

-   Range not centered at zero\
-   Uses ZeroPoint\
-   Uses uint8 (0--255)

Example:

\[-20, 1000\] → \[0, 255\]

✔ Better for activations\
✔ Handles shifted distributions

------------------------------------------------------------------------

# 6️. Modes of Quantization

------------------------------------------------------------------------

## --> Post-Training Quantization (PTQ)

Applied after training.

### Workflow

``` mermaid
flowchart TD
A[Train Model FP32] --> B[Pretrained Model]
B --> C[Calibration Dataset]
C --> D[Compute Scale & ZeroPoint]
D --> E[Convert to INT8]
E --> F[Deploy Model]
```

### Advantages

-   No retraining required\
-   Low cost\
-   Fast implementation

### Disadvantages

-   Slight accuracy drop

------------------------------------------------------------------------

## --> Quantization Aware Training (QAT)

Quantization simulated during training.

### Workflow

``` mermaid
flowchart TD
A[Training Data] --> B[Model + Fake Quant Layers]
B --> C[Forward + Backward Pass]
C --> D[Model Learns Quantization Noise]
D --> E[Export INT8 Model]
```

### Advantages

-   Higher accuracy than PTQ\
-   More robust

### Disadvantages

-   Requires retraining\
-   Higher compute cost

------------------------------------------------------------------------

# 7️. PTQ vs QAT Comparison

  Feature      PTQ      QAT
  ------------ -------- ---------
  Retraining   No       Yes
  Accuracy     Medium   High
  Cost         Low      High
  Complexity   Simple   Complex

------------------------------------------------------------------------

# 8️. Quantization in LLM Fine-Tuning

Modern fine-tuning combines quantization with:

-   LoRA\
-   QLoRA\
-   4-bit training\
-   8-bit training

### QLoRA Pipeline

``` mermaid
flowchart TD
A[Pretrained LLM 4-bit] --> B[Attach LoRA Adapters]
B --> C[Fine-Tune on Custom Dataset]
C --> D[Merge or Keep Adapters]
D --> E[Deploy Quantized Model]
```

QLoRA enables fine-tuning 7B--70B models on a single GPU.

------------------------------------------------------------------------

# 9️. Symmetric vs Asymmetric Diagram

``` mermaid
flowchart LR
A[-X ---- 0 ---- +X] --> B[-128 ---- 0 ---- 127]
C[Xmin -------- Xmax] --> D[0 -------- 255]
```

------------------------------------------------------------------------

# 10 .Complete Quantized Fine-Tuning Architecture

``` mermaid
flowchart TD
A[Pretrained Model FP32] --> B[Quantization 4-bit or 8-bit]
B --> C[Attach LoRA Adapters]
C --> D[Fine-Tuning]
D --> E[Quantized Custom LLM]
E --> F[Deployment Mobile / Edge / Cloud]
```

------------------------------------------------------------------------

#  Conclusion

Quantization is essential for:

-   Efficient deployment of LLMs\
-   Reducing infrastructure cost\
-   Enabling edge and mobile AI\
-   Scalable production systems

PTQ is ideal for quick deployment.\
QAT is ideal for high-accuracy production systems.\
QLoRA enables memory-efficient fine-tuning of large models.

------------------------------------------------------------------------

⭐ This repository contains systematic notes and flow diagrams for
understanding LLM quantization and fine-tuning.
