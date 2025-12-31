# 🟧 Day 2 — Class Imbalance & Sampling Strategy

## Objective

Prevent the model from collapsing into the trivial solution:

> **“Predict `O` everywhere and look accurate.”**

This day focuses on **measuring**, **understanding**, and **correcting** severe class imbalance in the token-level NER dataset.

---

## Step 1 — Label Distribution Analysis

Token-level label counts were computed from the processed training dataset (`span_ner.jsonl`).

### Raw Label Distribution (Top → Bottom)

```
O                                        154,096

I-Doubt                                  3,403
I-Loaded_Language                        3,257
I-Appeal_to_fear-prejudice               2,280
I-Causal_Oversimplification              1,518
I-Name_Calling,Labeling                  1,490
I-Appeal_to_Authority                    1,391
I-Exaggeration,Minimisation              1,196
I-Flag-Waving                            1,094

B-Loaded_Language                          854
B-Name_Calling,Labeling                   392
B-Repetition                              213
B-Exaggeration,Minimisation               189
B-Doubt                                   178
B-Appeal_to_fear-prejudice                139
B-Flag-Waving                              97
B-Causal_Oversimplification                74
B-Appeal_to_Authority                      72
B-Slogans                                  70
B-Whataboutism,Straw_Men,Red_Herring       51
B-Black-and-White_Fallacy                  34
B-Thought-terminating_Cliches              26
B-Bandwagon,Reductio_ad_hitlerum           23
```

### Key Observations

- `O` alone accounts for **~154k tokens**
    
- Non-`O` tokens total **~18–20k**
    
- Approximate ratio: **O : non-O ≈ 8 : 1**
    
- __B-_ labels are extremely scarce_* (some < 30 total tokens)
    

📌 **Critical Insight**  
Span detection depends on learning `B-*` labels.  
If these are ignored, entire propaganda spans are missed even if `I-*` is predicted correctly.

---

## Step 2 — Why Default Loss Fails

Using plain CrossEntropyLoss:

- Loss is dominated by `O`
    
- Predicting `O` everywhere yields ~90% token accuracy
    
- Rare techniques receive near-zero gradient signal
    
- Token accuracy becomes misleading
    

Therefore, **loss reweighting is mandatory**, not optional.

---

## Step 3 — Loss-Level Correction Strategy

### Decision

Use **Weighted CrossEntropyLoss** with:

- `O` → **small, non-zero weight**
    
- Rare `B-*` → higher weights
    
- `I-*` → medium weights
    

Focal loss and sampling strategies were intentionally **deferred** to keep v1.0 interpretable and stable.

---

## Step 4 — Class Weight Computation

### Method

- Inverse-frequency weighting with smoothing:
    
    ```
    weight_i = 1 / (count_i ^ α)
    ```
    
- Smoothing exponent: **α = 0.5**
    
- Weights normalized so **mean weight = 1**
    

This avoids gradient explosion for ultra-rare labels while still amplifying them.

---

## Step 5 — Verified Weight Statistics (Metrics)

After computation and normalization:

### Global Metrics

- **Number of labels:** 29
    
- **α (alpha):** 0.5
    
- **Mean weight:** 1.0
    
- **Min weight:** 0.036
    
- **Max weight:** 2.95
    

### Highest-Weight Labels (Rare Span Starters)

```
B-Bandwagon,Reductio_ad_hitlerum      count=23   weight=2.949
B-Thought-terminating_Cliches         count=26   weight=2.774
B-Black-and-White_Fallacy             count=34   weight=2.426
B-Whataboutism,Straw_Men,Red_Herring  count=51   weight=1.981
B-Slogans                             count=70   weight=1.691
```

### Lowest-Weight Labels

```
I-Doubt                               count=3,403  weight=0.242
I-Loaded_Language                     count=3,257  weight=0.248
I-Appeal_to_fear-prejudice            count=2,280  weight=0.296
I-Causal_Oversimplification           count=1,518  weight=0.363
O                                    count=154,096 weight=0.036
```

📌 **Invariant**

- `O` has the **lowest weight**
    
- Rare `B-*` labels have the **highest weights**
    
- No weight exceeds ~3 → training remains numerically stable
    

---

## Step 6 — Persistence & Reproducibility

Class weights were saved as a frozen artifact:

```
configs/class_weights.pt
```

Contents:

- `class_weights` tensor
    
- `label_counts`
    
- `alpha`
    

This ensures:

- Resume-safe long training runs
    
- Reproducible experiments
    
- No silent loss drift
    

---

## Outcome

After Day 2:

- Loss function no longer rewards predicting `O` everywhere
    
- Rare techniques meaningfully contribute to gradients
    
- Span-start labels (`B-*`) are amplified appropriately
    
- Training is safe for multi-day runs
    

---

## Status

✅ **Day 2 — Class Imbalance & Sampling Strategy: COMPLETE**

The model is now **allowed to care about rare propaganda**.