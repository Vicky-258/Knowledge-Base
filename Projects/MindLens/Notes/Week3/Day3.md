#  Day 3 — Model & Training Pipeline

## Objective

Build a **clean, resumable, GPU-safe training pipeline** for token-level propaganda NER using DistilBERT, with no silent failures during multi-day runs.

This day is about **infrastructure correctness**, not model quality.

---

## Model Setup

- **Base model:** `distilbert-base-uncased`
    
- **Head:** `DistilBertForTokenClassification`
    
- **Number of labels:** 29 (BIO + technique-aware)
    
- **Label mappings:** loaded from `labels.json` and injected into model config
    

```python
model.config.label2id = label2id
model.config.id2label = id2label
```

📌 **Invariant**

- `label2id["O"] == 0`
    
- Label mappings are frozen from Day 1
    

---

## Dataset & Batching

- Training data loaded from `span_ner.jsonl`
    
- Articles have variable length → **custom `collate_fn` required**
    

### Padding Strategy

- `input_ids` → padded with tokenizer pad token
    
- `attention_mask` → padded with `0`
    
- `labels` → padded with `-100` (ignored by loss)
    

This prevents shape errors and ensures padding tokens do not affect gradients.

---

## Loss Function

- **Type:** Weighted CrossEntropyLoss
    
- **Class weights:** loaded from `configs/class_weights.pt` (Day 2 artifact)
    
- **Padding ignored:** `ignore_index = -100`
    

This ensures:

- Background (`O`) does not dominate training
    
- Rare `B-*` labels receive meaningful gradient signal
    

Loss behavior verified to be numerically stable.

---

## Optimization

- **Optimizer:** AdamW
    
    - Learning rate: `5e-5`
        
    - Weight decay: `0.01`
        
- **Scheduler:** Linear warmup
    
    - Warmup steps: `10%` of total training steps
        
    - Prevents early instability with weighted loss
        
- **Gradient clipping:** `max_norm = 1.0`
    
    - Prevents gradient explosion in long runs
        

---

## GPU & Environment Handling

- Explicit device selection:
    
    ```python
    device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
    ```
    
- Model, batches, and class weights explicitly moved to GPU
    
- Kaggle-specific protobuf conflict resolved (`protobuf==3.20.3`)
    

This prevents silent CPU fallback and environment-related crashes.

---

## Checkpointing & Resumability

- Checkpoints saved to:
    
    ```
    /kaggle/working/checkpoints/
    ```
    
- Contents:
    
    - model state
        
    - optimizer state
        
    - scheduler state
        
    - epoch and step
        
- Resume logic:
    
    - Automatically loads latest checkpoint if present
        
    - Training continues from correct epoch and step
        

This makes training safe across:

- Kaggle session resets
    
- multi-day runs
    
- accidental interruptions
    

---

## Training Verification (Sanity Run)

A short multi-epoch run was executed to validate the pipeline.

### Observed Metrics

- Initial loss (step 0): **~3.36**
    
- Later loss (step 200): **~1.88**
    
- Loss decreased smoothly
    
- No NaNs, shape errors, or crashes
    

This confirms:

- Data pipeline is correct
    
- Loss weighting works
    
- Gradients flow as expected
    
- Training loop is stable
    

---

## Outcome

By the end of Day 3:

- End-to-end training runs successfully on GPU
    
- Variable-length batching works correctly
    
- Loss decreases meaningfully
    
- Checkpoints are resumable and reliable
    
- No silent bugs remain in the training pipeline
    

At this point, failures are attributable to **model behavior**, not infrastructure.

---

## Status

✅ **Day 3 — Model & Training Pipeline: COMPLETE**

The system is now ready for **evaluation and analysis**, not debugging.
