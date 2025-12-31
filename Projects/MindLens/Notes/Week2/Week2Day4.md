# 🗓️ Week 2 — Day 4: Build Full NER Dataset

## Objective

Construct the full span-level NER dataset by joining raw article texts with character-level propaganda annotations and converting them into BIO-labeled token sequences.

---

## Dataset Reality

The SemEval dataset does **not** provide pre-joined samples. Instead, it contains:

- `train-articles/`  
    → one text file per article (`article<id>.txt`)
    
- `train-task2-TC.labels`  
    → flat span annotations:
    
    ```
    article_id<TAB>label<TAB>char_start<TAB>char_end
    ```
    

Therefore, dataset samples must be **constructed during preprocessing** by joining texts and spans using `article_id`.

---

## Preprocessing Pipeline

### 1. Load Span Annotations

- Span annotations are parsed from `train-task2-TC.labels`
    
- Spans are grouped by `article_id`
    
- Each span is stored as:
    
    ```
    (char_start, char_end, label)
    ```
    

Resulting structure:

```
article_id → list of spans
```

---

### 2. Load Article Text

- Article text is loaded from `train-articles/article<id>.txt`
    
- Text is treated as raw, unmodified input
    

---

### 3. Span → BIO Conversion

- Each article’s text and spans are passed to the `spans_to_bio` function
    
- Tokenization is performed using `DistilBertTokenizerFast`
    
- Character-level spans are projected onto token-level offsets
    
- BIO tags are assigned using robust overlap logic:
    
    ```
    token_end > span_start AND token_start < span_end
    ```
    

All tokens are initialized as `O`, and only span-overlapping tokens receive `B-` or `I-` labels.

---

### 4. Label Encoding

- BIO labels are converted to integer IDs using a **frozen `label2id` mapping**
    
- The mapping exactly matches dataset label spellings (including British variants such as `Exaggeration,Minimisation`)
    
- Any unknown label results in a hard failure to prevent silent corruption
    

---

### 5. Dataset Serialization

Each article is serialized as one JSON object with:

- `article_id`
    
- `input_ids`
    
- `attention_mask`
    
- `labels`
    

The dataset is saved in **JSONL format**:

```
data/processed/span_ner.jsonl
```

One line corresponds to one article.

---

## Sanity Checks

The following invariants are enforced during dataset construction:

- `len(input_ids) == len(labels)`
    
- All label IDs are valid (`0 ≤ label_id < |label2id|`)
    
- Special tokens (`[CLS]`, `[SEP]`) are labeled as `O`
    
- BIO transitions are valid (`I-*` never appears without a preceding `B-*`)
    

Dataset-level checks:

- Number of samples equals number of unique article IDs
    
- Non-`O` labels are present (dataset is not degenerate)
    

---

## Output Summary

- Total samples generated: **357**
    
- Dataset format: **JSONL**
    
- Labeling scheme: **Single-label BIO**
    
- Dataset is ready for:
    
    - padding
        
    - batching
        
    - model training
        

---

## Key Insight

> The dataset construction step acts as a compiler that translates rhetorical spans defined in character space into token-level BIO sequences that a transformer model can learn from.

---

## Status

✅ Full dataset built  
✅ BIO labels verified on real samples  
✅ Label inconsistencies resolved  
✅ Dataset integrity enforced

**Day 4 complete.**
