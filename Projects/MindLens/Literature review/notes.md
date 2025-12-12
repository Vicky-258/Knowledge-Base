## **1. Severe Class Imbalance (357 positive vs 14 negative)**

- The training set is **95% propaganda articles**.
    
- A model can get **96% accuracy** just by predicting **everything = propaganda**.
    
- Both TF-IDF and BERT discovered the same shortcut:
    
    > “Why predict negative? The loss function punishes me more when I try.”
    
- Because the dataset punishes false negatives FAR less than false positives, the model collapses into **one-class behavior**.
    

### ✔ This explains why:

- No negative predictions were made.
    
- Precision, recall, and F1 looked artificially high.
    
- The confusion matrix showed zeros in the negative column.
    

---

## **2. Article-Level Labels Are Too Coarse**

The SemEval dataset is _not_ designed for:

> “Does this article contain propaganda?”

It’s designed for:

> “Which _parts_ of this article contain propaganda? What technique is used? Where?”

The article-level binary label throws away **90% of the useful information**.

Example:

An article may contain:

- 7 manipulative phrases
    
- 200 neutral sentences
    

But we treat all of that as:  
**propaganda = 1**

This leads to:

- models learning article _topic style_, not manipulation
    
- errors where non-manipulative articles share topic with manipulative ones
    
- unreliable real-world performance
    

### ✔ This directly causes:

- DistilBERT overfitting to subject matter
    
- inability to distinguish “clean” vs “tainted” articles
    
- false sense of accuracy
    

---

## **3. Very Few Negative Samples → Impossible Supervised Task**

Negative class = **14 articles total**.

If 3 of them fall into the test split, the model has:

- 11 negatives to learn from
    
- 350+ positives overpowering the gradients
    

This is too small for:

- Logistic Regression
    
- BERT
    
- ANY classifier on Earth
    

The negative class is statistically invisible.

### ✔ Result:

Model learns:

> “Propaganda is ALWAYS present.”

Which is consistent with the dataset biases.

---

# 🧨 Final Insight

Both models performed **perfectly wrong**, which is exactly what we needed to prove:

### 🚫 Article-level propaganda detection is a flawed task.

### ✅ Span-level technique detection (BIO NER) is the correct task.

This is why we're moving the entire project into:

👉 token classification  
👉 span detection  
👉 technique tagging  
👉 multi-module manipulation ecosystem

Now the model can actually:

- highlight manipulative phrases
    
- differentiate techniques
    
- avoid being fooled by article topic
    
- detect “clean” vs “tainted” parts properly
    
- handle class imbalance naturally (distribution over tokens)
    

---