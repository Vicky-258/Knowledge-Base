## 🧬 1. BIO Tagging Approach

### 📌 Goal

Explain **how propaganda spans are converted into token-level BIO labels**.

### 🔹 Tag Scheme

- **B-** → beginning of a propaganda span
    
- **I-** → continuation of the same span
    
- **O** → non-propaganda token
    

> Assumption to state explicitly:  
> _Each token can have at most one label._

### 🔹 Tokenization Alignment

Document **exactly**:

- Tokenizer used (word-level / subword / whitespace)
    
- How character spans → token indices are mapped
    

Answer these in bullets:

- What happens if a span starts mid-token?
    
- Do subword tokens inherit the same BIO tag?
    
- Are special tokens ignored or labeled as `O`?
    

📌 _This section should let someone re-implement your span→BIO conversion._

---

## 🔁 2. Overlap Handling Strategy

This is **critical** — reviewers love this question.

### ❓ Problem

Some text spans contain **multiple propaganda techniques overlapping**.

### ✅ Chosen Strategy (state one clearly)

Examples (pick your actual one):

- **Priority-based**
    
    > When overlaps occur, the label with higher predefined priority is kept.
    
- **Longest-span wins**
    
    > The span covering more characters/tokens is retained.
    
- **First-annotation wins**
    
    > Based on dataset order.
    
- **Dropped overlaps**
    
    > Overlapping regions are removed to preserve label purity.
    

### 🔹 Justification

One short paragraph:

- Why this choice?
    
- What tradeoff did you accept? (signal loss vs ambiguity)
    

⚠️ Also state what you **did NOT try** (multi-label BIO, stacking, etc.).

---

## 📊 3. Dataset Statistics

Put numbers. No vibes.

### 📈 Overall

- Total samples: `N`
    
- Total tokens: `N`
    
- Avg tokens per sample: `~N`
    

### 🏷️ Label Distribution

Table or bullet list like:

```
O                              154,096
I-Doubt                          3,403
I-Loaded_Language                3,257
I-Appeal_to_fear-prejudice       2,280
...
```

Also add:

- % of `O` vs non-`O`
    
- Top 5 most frequent labels
    
- Bottom 5 rarest labels
    

📌 This screams _class imbalance awareness_ — very important.

---

## 🧠 4. Assumptions & Limitations

This is where you sound **honest and mature**, not weak.

### ✅ Assumptions

Examples:

- Only one propaganda technique per token
    
- Gold spans are treated as ground truth
    
- Tokenization errors are acceptable noise
    

### ⚠️ Limitations

Be blunt:

- Overlaps force information loss
    
- BIO cannot represent nested spans
    
- Rare labels may be under-learned
    
- Model performance may bias toward `O`
    

End with:

> These limitations are accepted for **baseline stability**, not claimed as optimal.
