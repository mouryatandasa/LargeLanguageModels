
# Sample - Large Language Model (From Scratch)

This repository demonstrates how a basic character-level language model pipeline works using a very small dataset.

We manually trace how the text:

"hello world"

is processed step-by-step through the pipeline.

---

##  Pipeline Overview

Text
↓
Tokenization
↓
Integer Mapping
↓
Embedding Vectors
↓
Dense Output (Logits)
↓
Probability Distribution (Softmax)

---

##  Dataset and Input

Dataset:
"hello world"

Model Input:
"hello"

Goal:
Given "hello" → Predict the next character.

---

## 🔹 Step 1 — Vocabulary Creation

Unique characters in "hello world":

{'h', 'e', 'l', 'o', ' ', 'w', 'r', 'd'}

After sorting:

[' ', 'd', 'e', 'h', 'l', 'o', 'r', 'w']

Vocabulary size:
8

Character → Index Mapping:

' ' → 0  
'd' → 1  
'e' → 2  
'h' → 3  
'l' → 4  
'o' → 5  
'r' → 6  
'w' → 7  

---

## 🔹 Step 2 — Convert Input to Integers

Input:
"hello"

Conversion:

h → 3  
e → 2  
l → 4  
l → 4  
o → 5  

Integer sequence:
[3, 2, 4, 4, 5]

Tensor shape:
(1, 5)

Explanation:
1 → Batch size  
5 → Sequence length  

Tensor representation:
[[3, 2, 4, 4, 5]]

---

## 🔹 Step 3 — Embedding Layer

Embedding dimension:
10

Embedding Matrix Shape:
(8, 10)

Meaning:
8 rows → one per character  
10 columns → embedding size  

Each integer is replaced with its corresponding embedding vector.

Input:
[3, 2, 4, 4, 5]

Becomes:

[
 embedding(h),
 embedding(e),
 embedding(l),
 embedding(l),
 embedding(o)
]

Output shape:
(1, 5, 10)

---

## 🔹 Step 4 — Dense Layer (Prediction Layer)

Each 10-dimensional embedding is transformed into an 8-dimensional output vector (vocab size = 8).

Output shape:
(1, 5, 8)

For each input token, the model predicts scores for all 8 possible next characters.

---

## 🔹 Step 5 — Last Token Selection

We select the last token ("o") output.

Shape:
(8,)

This represents:

"Given 'hello', what is the next character?"

These values are called logits (raw scores).

---

## 🔹 Step 6 — Softmax

Softmax converts logits into probabilities:

P(i) = e^(logit_i) / Σ e^(logit_j)

Properties:
• Values between 0 and 1  
• Sum of probabilities = 1  

Example (random since untrained):

' ' → 0.18  
'd' → 0.03  
'e' → 0.05  
'h' → 0.07  
'l' → 0.12  
'o' → 0.09  
'r' → 0.22  
'w' → 0.24  

---

## 🔹 Step 7 — Argmax

Highest probability index → 7  

Index 7 → 'w'

Final prediction:

"hello" → next character = "w"

---

##  Main Aim:

• How text becomes numbers  
• How embeddings convert tokens into vector space  
• How neural networks produce probability distributions  
• Core mechanics behind language models  

Even large language models follow the same fundamental principle — just at massive scale.
