# 🗓️ Week 2 — Day 5: Dataset Validation & Statistics

## Objective

Validate the quality, balance, and learnability of the constructed span-level NER dataset before model training.

---

## Dataset Overview

- Total samples (articles): **357**
    
- Total tokens: **175,012**
    
- Propaganda tokens: **20,916**
    
- Non-propaganda tokens (`O`): **154,096**
    
- Propaganda token ratio: **~11.95%**
    

This ratio is within the expected range for span-level NER tasks and indicates a healthy balance between background text and labeled spans.

---

## Propaganda vs Non-Propaganda Distribution

- `O` labels dominate (~88%), which is expected and necessary for learning normal language patterns.
    
- Propaganda tokens (~12%) provide sufficient positive signal without overwhelming the model.
    

This confirms that the task is neither degenerate (almost all `O`) nor unrealistic (too many labeled tokens).

---

## BIO Label Distribution

Observations:

- `I-*` labels occur more frequently than `B-*` labels.
    
- This is expected because propaganda spans typically cover multiple tokens.
    
- No illegal BIO patterns were observed (e.g., `I-*` without a preceding `B-*`).
    

BIO tagging logic appears correct and consistent.

---

## Technique-Level Token Counts (B + I Combined)

Token counts per propaganda technique:

- **Loaded_Language**: 4111
    
- **Doubt**: 3581
    
- **Appeal_to_fear-prejudice**: 2419
    
- **Name_Calling,Labeling**: 1882
    
- **Causal_Oversimplification**: 1592
    
- **Appeal_to_Authority**: 1463
    
- **Exaggeration,Minimisation**: 1385
    
- **Flag-Waving**: 1191
    
- **Whataboutism,Straw_Men,Red_Herring**: 872
    
- **Repetition**: 806
    
- **Black-and-White_Fallacy**: 569
    
- **Bandwagon,Reductio_ad_Hitlerum**: 459
    
- **Slogans**: 372
    
- **Thought-terminating_Cliches**: 214
    

This distribution reflects a realistic long-tail:

- Common techniques (e.g., Loaded Language, Doubt) dominate.
    
- Less frequent techniques are still sufficiently represented to remain learnable.
    

---

## Rare Technique Analysis

- No technique falls below the extreme rarity threshold (e.g., <100 tokens).
    
- Even the least frequent technique (`Thought-terminating_Cliches`) has over 200 tokens.
    

Therefore:

- No techniques need to be removed or merged further at this stage.
    
- Class imbalance exists but is not pathological.
    

---

## Qualitative Inspection

Random samples were manually inspected by printing token–label pairs:

- BIO spans align with human intuition.
    
- No special tokens (`[CLS]`, `[SEP]`) are mislabeled.
    
- Subword tokens (e.g., `##ing`, `##los`) appear as expected due to BERT tokenization and do not indicate errors.
    
- Span boundaries appear reasonable and consistent with annotations.
    

---

## Key Insights

- The dataset is **statistically sane** and **learnable**.
    
- Class imbalance is expected and acceptable for a baseline model.
    
- No immediate rebalancing, label merging, or data filtering is required before training.
    

Any future performance issues on low-frequency techniques should be interpreted in light of their lower token counts, not as preprocessing errors.

---

## Status

✅ Token counts computed  
✅ Propaganda vs non-propaganda ratio validated  
✅ Technique-level distributions analyzed  
✅ No pathological label imbalance detected  
✅ Manual inspection passed

**Day 5 complete.**
