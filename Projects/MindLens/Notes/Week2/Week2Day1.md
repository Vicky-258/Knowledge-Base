# 🗓️ Week 2 — Day 1: Label Design & Strategy

## Objective

Define an unambiguous label space and tagging strategy for span-level propaganda detection before dataset preprocessing and model training.

---

## Dataset Observations

- The dataset provides **span-level annotations** with:
    
    - document ID
        
    - character-level `start` and `end`
        
    - a **single technique label per span**
        
- Labels are stored as **single strings**, even when they contain multiple technique names separated by commas.
    
- There is **no explicit multi-label annotation** for the same span.
    

---

## Conceptual Overlap vs Dataset Representation

- Propaganda techniques may **conceptually overlap** in real-world text.
    
- However, the dataset **does not represent overlap explicitly**.
    
- Instead:
    
    - Low-frequency but related techniques were **merged into superclasses** by the dataset authors.
        
    - This merging was done to address **class imbalance**, not to model overlap.
        

Example:

- `"Whataboutism"`, `"Straw Men"`, and `"Red Herring"`  
    → merged into `"Whataboutism,Straw_Men,Red_Herring"`
    

As a result:

- Each annotated span has **exactly one (possibly merged) label**.
    
- No custom overlap-resolution logic is required.
    

---

## Final Technique Label Set (Frozen)

The following **14 technique labels** are used exactly as provided by the dataset:

- `Appeal_to_Authority`
    
- `Appeal_to_fear-prejudice`
    
- `Bandwagon,Reductio_ad_Hitlerum`
    
- `Black-and-White_Fallacy`
    
- `Causal_Oversimplification`
    
- `Doubt`
    
- `Exaggeration,Minimization`
    
- `Flag-Waving`
    
- `Loaded_Language`
    
- `Name_Calling,Labeling`
    
- `Repetition`
    
- `Slogans`
    
- `Thought-terminating_Cliches`
    
- `Whataboutism,Straw_Men,Red_Herring`
    

The technique  
`Obfuscation,Intentional Vagueness,Confusion`  
was removed by the dataset authors due to extreme underrepresentation and is **not used**.

---

## Tagging Scheme

We use a **BIO (Begin–Inside–Outside) sequence tagging scheme**:

- `B-<TECHNIQUE>` — first token of a propaganda span
    
- `I-<TECHNIQUE>` — continuation token of the same span
    
- `O` — token outside any propaganda span
    

This enables **span-level detection** rather than isolated token classification.

---

## Overlap Handling Strategy

- No additional overlap handling is implemented.
    
- The dataset already resolves overlaps by assigning a **single merged label per span**.
    
- The task is therefore modeled as **single-label BIO NER**.
    

---

## Label Encoding (`label2id`)

- `O` is assigned ID `0`
    
- All BIO-prefixed labels are assigned IDs deterministically
    
- Ordering is **alphabetical**, ensuring reproducibility
    
- The mapping is fixed and shared across preprocessing, training, and evaluation
    

Total labels:

- `1` (`O`) + `14 × 2` (`B`/`I`) = **29 labels**
    

The `label2id` mapping is frozen and must not be modified.

---

## Status

✅ Label space finalized  
✅ BIO scheme locked  
✅ Overlap ambiguity resolved  
✅ `label2id` frozen

**Day 1 complete.**
