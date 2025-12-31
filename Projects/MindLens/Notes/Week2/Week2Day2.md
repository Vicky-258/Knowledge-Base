# 🗓️ Week 2 — Day 2: Tokenization & Span Alignment

## Objective

Understand how character-level propaganda spans map to token-level representations and establish correct alignment rules for BIO tagging.

---

## Why Tokenization Matters

- The dataset provides propaganda annotations as **character-level spans** `(char_start, char_end)`.
    
- Transformer models (DistilBERT) operate on **tokens**, not characters or words.
    
- Tokens are often **subwords**, meaning:
    
    - One word may map to multiple tokens
        
    - One span may cover multiple tokens
        

Therefore, a reliable **character → token mapping** is required.

---

## Tokenizer Choice

- We use `DistilBertTokenizerFast`
    
- Reason:
    
    - Fast tokenizers provide `offset_mapping`
        
    - Offset mapping is essential for span alignment
        
    - Slow tokenizers cannot support this task
        

---

## Offset Mapping

For each token, the tokenizer provides:

`(token_char_start, token_char_end)`

Key properties:

- Offsets are **half-open intervals** `[start, end)`
    
- Tokens like `[CLS]` and `[SEP]` have offsets `(0, 0)`
    
- `(0, 0)` tokens do **not** correspond to original text
    

These offsets create the bridge between:

- character-level annotations
    
- token-level labels
    

---

## Relationship Between Offset Mapping and BIO

- Offset mapping answers:  
    **“Which characters produced this token?”**
    
- BIO answers:  
    **“What role does this token play in a span?”**
    

Process:

1. Use offsets to determine which tokens fall inside a span
    
2. Use BIO to mark:
    
    - `B-<LABEL>` → first token of the span
        
    - `I-<LABEL>` → continuation tokens
        
    - `O` → tokens outside all spans
        

BIO is necessary because spans often cover **multiple tokens**, especially with subword tokenization.

---

## Alignment Rules (Formalized)

### Rule 1 — Ignore Special Tokens

- Tokens with offset `(0, 0)` (e.g., `[CLS]`, `[SEP]`)
    
- Always labeled as `O`
    

---

### Rule 2 — Token Inside Span

A token belongs to a span if:

`token_start >= span_start AND token_end <= span_end`

This avoids partial overlaps and off-by-one errors.

---

### Rule 3 — BIO Assignment

For tokens inside a span:

- First matching token → `B-<LABEL>`
    
- Remaining tokens → `I-<LABEL>`
    

---

### Rule 4 — Outside Tokens

- Any token not claimed by a span → `O`
    

---

### Rule 5 — Single Label per Token

- Each token receives at most one label
    
- The dataset already resolves overlaps by using merged labels
    

---

## Observations from Exploration

- Words may remain intact or be split into subwords depending on vocabulary
    
- Punctuation is often tokenized separately
    
- Multiple spaces and special characters can affect offsets
    
- Alignment must always be done using offsets, never token strings
    

---

## Key Insight

> Offset mapping projects character-level truth onto token-level reality, and BIO encodes span boundaries in a form the model can learn.

Without offsets, BIO cannot be assigned correctly.  
Without BIO, spans cannot be represented structurally.

---

## Status

✅ Tokenizer behavior explored  
✅ Offset mapping understood  
✅ BIO–offset relationship clarified  
✅ Alignment rules formalized  
✅ Preprocessing code implemented

**Day 2 complete.**