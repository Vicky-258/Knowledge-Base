# Week 2 — Dataset Construction & BIO Tagging Notes

This document records the exact decisions, assumptions, and limitations used during Week 2 of the MindLens project.  
The goal is **reproducibility and transparency**, not perfection.

---

## 1. Objective

Convert span-level propaganda annotations into a **token-level BIO tagging format** suitable for NER-style model training.

This dataset is intended as a **stable baseline**, not a final representation of propaganda complexity.

---

## 2. BIO Tagging Scheme

### Label Format
- `B-<LABEL>` : Beginning of a propaganda span
- `I-<LABEL>` : Inside the same propaganda span
- `O` : Outside any propaganda span

Each token is assigned **exactly one label**.

### Core Assumption
> A token cannot belong to more than one propaganda technique.

This constraint is imposed by the BIO scheme and is acknowledged as a simplification.

---

## 3. Tokenization & Span Alignment

- Tokenization is performed at the **word level**.
- Gold annotations are provided as **character-level spans**.
- Character spans are mapped to token indices via start–end overlap.

### Alignment Rules
- If a span partially overlaps a token, that token is considered **inside the span**.
- All tokens fully or partially covered by a span receive the corresponding BIO tag.
- Tokens outside all spans are labeled `O`.

Special tokens (if any) are ignored or labeled `O`.

---

## 4. Overlap Handling Strategy

### Problem
Some text regions contain **overlapping propaganda techniques**, which BIO tagging cannot represent directly.

### Chosen Strategy
**Single-label resolution with information loss**.

- When spans overlap, **only one label is retained**.
- Overlapping spans are resolved using a deterministic rule (e.g., priority / longest-span / first-annotation).
- The discarded label is dropped entirely for that region.

### Rationale
- Maintains BIO consistency
- Avoids ambiguous or multi-label tokens
- Keeps training pipeline simple and reproducible

### Explicit Non-Goals
- Multi-label BIO tagging
- Nested span representation
- Span stacking or parallel tag sequences

These are deferred to future work.

---

## 5. Dataset Statistics

### Overall
- Total samples: `<N>`
- Total tokens: `<N>`
- Average tokens per sample: `<N>`

### Label Distribution (Token-Level)

```
O 154,096  
I-Doubt 3,403  
I-Loaded_Language 3,257  
I-Appeal_to_fear-prejudice 2,280  
I-Causal_Oversimplification 1,518  
I-Name_Calling,Labeling 1,490  
I-Appeal_to_Authority 1,391  
I-Exaggeration,Minimisation 1,196  
I-Flag-Waving 1,094  
B-Loaded_Language 854  
I-Whataboutism,Straw_Men,  
I-Red_Herring 821  

```

### Observations
- Strong class imbalance
- `O` dominates the token distribution
- Several techniques are extremely rare
- Many spans are short (1–3 tokens)

---

## 6. Reflections on BIO Tagging

Key difficulties encountered:
- Span-to-token alignment is error-prone
- Overlaps force unavoidable information loss
- Short spans are fragile during training
- BIO boundaries amplify small annotation errors

Key realization:
> BIO tagging does not represent reality — it represents a **controlled projection of reality** for learning.

---

## 7. Assumptions

- Gold annotations are treated as ground truth
- Each token has only one valid label
- Tokenization noise is acceptable
- Boundary ambiguity is tolerated

These assumptions prioritize **training stability** over expressive power.

---

## 8. Limitations

- Overlapping propaganda techniques cannot be represented
- Nested or hierarchical techniques are flattened
- Rare labels may be under-learned
- Token-level accuracy may overestimate true span quality

These limitations are **known and accepted** for this phase.

---

## 9. Planned Improvements (Future Work)

Not applied in Week 2.

- Span-level or multi-label architectures
- Character-aware boundary modeling
- Separate heads for emotional vs rhetorical techniques
- Better handling of overlapping annotations

---

## 10. Reproducibility Statement

Given:
- Raw annotated dataset
- Tokenization method
- Overlap resolution rule

Another practitioner should be able to reproduce the Week 2 dataset exactly.

If results differ, the discrepancy is considered a **bug**.

[[Projects/MindLens/Week 3|Week 3]]