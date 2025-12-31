# 🧠 MindLens — Week 3: DistilBERT Span Detector (v1.0)

## 🎯 Weekly Goal
Train the **first production-grade propaganda span detector** using DistilBERT with technique awareness.

Outcome: **MindLens Span Detector v1.0**

---

## 🗓️ Day 1 — Dataset Engineering (NER Core)

### Goal
Convert raw annotations → clean, model-ready NER samples.

### Tasks
- [x] Review dataset schema (tokens, spans, techniques)
- [x] Define label space  
  - [x] `O`
  - [x] `B-TECHNIQUE`
  - [x] `I-TECHNIQUE`
- [x] Implement `PropagandaNERDataset`
  - [x] Tokenize with offset mappings
  - [x] Align spans → token labels
  - [x] Handle subword tokenization correctly
- [x] Verify BIO correctness on edge cases
- [x] Write 2–3 unit tests for span alignment

### Checkpoint
You can print tokens + BIO tags and *trust them*.

---

## 🗓️ Day 2 — Class Imbalance & Sampling Strategy

### Goal
Prevent the model from learning “everything is O”.

### Tasks
- [x] Compute label distribution
- [x] Calculate class weights
- [x] Integrate weighted loss into training
- [x] Decide:
  - [x] weighted CrossEntropy
  - [x] or focal loss (optional)
- [x] Log class frequencies for experiment tracking

### Checkpoint
Rare techniques no longer vanish during training.

---

## 🗓️ Day 3 — Model & Training Pipeline

### Goal
Get a **clean, reproducible training loop**.

### Tasks
- [x] Load `DistilBertForTokenClassification`
- [x] Configure label mappings (`id2label`, `label2id`)
- [x] Build training loop
- [x] Implement:
  - [x] AdamW optimizer
  - [x] Linear warmup scheduler
- [x] Enable gradient clipping
- [x] Add checkpoint saving

### Checkpoint
Model trains end-to-end without silent bugs.

---

## 🗓️ Day 4 — Token-Level Evaluation

### Goal
Know if the model is learning *anything at all*.

### Tasks
- [x] Implement token-level accuracy
- [x] Mask padding tokens properly
- [x] Log:
  - [x] train loss
  - [x] validation loss
  - [x] token accuracy
- [x] Plot loss curves

### Checkpoint
Validation loss decreases meaningfully.

---

## 🗓️ Day 5 — Span-Level & Technique-Level Metrics

### Goal
Evaluate what *actually matters*.

### Tasks
- [x] Decode BIO predictions → spans
- [x] Implement span-level precision / recall / F1
- [x] Compute technique-wise F1
- [x] Identify weakest techniques
- [x] Save metrics per epoch

### Checkpoint
Metrics reflect *real propaganda detection*, not token noise.

---

## 🗓️ Day 6 — Visualization & Error Analysis

### Goal
See the model think.

### Tasks
- [ ] Render predicted spans on raw text
- [ ] Color-code techniques
- [ ] Compare:
  - [ ] gold spans
  - [ ] predicted spans
- [ ] Collect 10 failure examples
- [ ] Categorize errors:
  - [ ] boundary errors
  - [ ] confusion between techniques
  - [ ] false positives

### Checkpoint
You understand *why* the model fails.

---

## 🗓️ Day 7 — Stabilization & v1.0 Release

### Goal
Freeze a solid baseline.

### Tasks
- [ ] Select best checkpoint
- [ ] Clean training config
- [ ] Write `SpanDetector_v1.md`
  - [ ] architecture
  - [ ] metrics
  - [ ] known limitations
- [ ] Tag version: `span-detector-v1.0`
- [ ] Push results to repo

### Checkpoint
MindLens Span Detector **exists**.

---

## ✅ End-of-Week Definition of Done
- [ ] Trained DistilBERT NER model
- [ ] Span-level F1 reported
- [ ] Technique-level F1 reported
- [ ] Visual span inspection works
- [ ] Model ready for downstream Truth Lens scoring
