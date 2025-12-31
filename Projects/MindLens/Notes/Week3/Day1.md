# 🟧 Day 1 — Dataset Engineering (NER Core)

## Objective

Convert raw SemEval Task 2 propaganda annotations (character spans) into **clean, model-ready token-level BIO labels** suitable for training a DistilBERT NER model.

This day establishes the **ground truth pipeline**. Any errors here would silently corrupt all downstream results.

---

## Dataset Overview

- **Input**
    
    - Raw article text files (`article<ID>.txt`)
        
    - Span annotations in TSV format:
        
        ```
        article_id   label   start_char   end_char
        ```
        
- **Output**
    
    - JSONL file (`span_ner.jsonl`)
        
    - One record per article containing:
        
        ```json
        {
          "article_id": "...",
          "input_ids": [...],
          "attention_mask": [...],
          "labels": [...]
        }
        ```
        

---

## Label Space (Frozen)

- BIO tagging scheme
    
- Technique-aware (no generic `TECHNIQUE`)
    

Total labels: **29**

- `O`
    
- `B-<technique>`
    
- `I-<technique>`
    

📌 **Important:**  
`labels.json` is now considered **part of the model definition**.  
Any change requires a new model version.

---

## Tokenization Strategy

- **Tokenizer:** `DistilBertTokenizerFast`
    
- **Why fast tokenizer**
    
    - Provides reliable `offset_mapping`
        
    - Required for robust char-span → token alignment
        
- Special tokens (`[CLS]`, `[SEP]`) are included and always labeled `O`.
    

---

## Span → Token Alignment Logic

### Core Principle

A token is part of a span **iff**:

```
token_start < span_end AND token_end > span_start
```

This overlap-based logic correctly handles:

- subword splits
    
- punctuation boundaries
    
- partial overlaps
    
- mid-word spans
    

### BIO Assignment Rules

- First overlapping token → `B-<LABEL>`
    
- Remaining overlapping tokens → `I-<LABEL>`
    
- Tokens with no overlap → `O`
    
- Special tokens → `O`
    

### Dataset Guarantee

- **No overlapping spans** exist in the dataset  
    → no conflict resolution required  
    → simpler, deterministic labeling
    

---

## `spans_to_bio` Function (Single Source of Truth)

All span logic is centralized in one function:

- Tokenizes text with offsets
    
- Skips special tokens
    
- Applies overlap logic
    
- Assigns BIO labels
    
- Converts labels → label IDs
    

This makes:

- debugging easier
    
- behavior reproducible
    
- future audits possible
    

---

## Sanity Checks Per Sample

During dataset construction, the following invariants are enforced:

- `len(input_ids) == len(labels)`
    
- All label IDs are valid
    
- No illegal `I-*` without preceding `B-*`
    
- Output visually inspected on real samples
    

These checks catch silent corruption early.

---

## Known Limitations (Accepted)

- No handling for overlapping spans (not needed for this dataset)
    
- Attention mask is temporarily set to all `1`s
    
    - Padding will be handled later in the training pipeline
        

---

## Outcome

By the end of Day 1:

- A **trusted**, **frozen**, **training-ready** NER dataset exists
    
- Token-level BIO labels correctly reflect original character spans
    
- Any future model failure is **not attributable to data preprocessing**
    

---

## Status

✅ **Day 1 — Dataset Engineering: COMPLETE**  
Dataset pipeline verified, audited, and frozen for v1.0.