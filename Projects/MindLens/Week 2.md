# 🧠 MindLens — Week 2 Checklist (BIO Tagging Dataset Construction)

**Goal:** Prepare the TRUE dataset needed for span‑level, technique‑aware propaganda detection (NER).

This week is about **data correctness, alignment, and decisions**. No rushing. No shortcuts.

---

## 📅 Day 1 — Label Design & Strategy

**Objective:** Decide *exactly* what the model will predict.

* [ ] List all propaganda techniques present in the dataset
* [ ] Decide BIO format:

  `B-<TECHNIQUE>`
   `I-<TECHNIQUE>`
  `O`
* [ ] Decide overlap handling strategy:

  * [ ] Priority-based (dominant technique wins)
  * [ ] Merge / drop secondary overlaps (if needed)
* [ ] Create final label → id mapping
* [ ] Write down all decisions in NOTES.md

✅ **Done when:** Label scheme is frozen and unambiguous.

---

## 📅 Day 2 — Tokenization & Alignment Exploration

**Objective:** Understand how text spans map to tokens.

* [ ] Load `DistilBertTokenizerFast`
* [ ] Tokenize 2–3 sample articles
* [ ] Inspect `input_ids`
* [ ] Inspect `offset_mapping`
* [ ] Manually verify char → token alignment
* [ ] Identify edge cases (punctuation, quotes, whitespace)
* [ ] Note alignment pitfalls

✅ **Done when:** You trust the tokenizer mapping.

---

## 📅 Day 3 — Span → BIO Tag Conversion Logic

**Objective:** Implement correct BIO labeling.

* [ ] Initialize all tokens as `O`
* [ ] Iterate through propaganda spans
* [ ] Assign `B-TECHNIQUE` to first token in span
* [ ] Assign `I-TECHNIQUE` to continuation tokens
* [ ] Handle spans starting/ending mid-token
* [ ] Apply overlap strategy consistently
* [ ] Unit-test BIO logic on sample articles

✅ **Done when:** BIO tags look correct for samples.

---

## 📅 Day 4 — Build Full NER Dataset

**Objective:** Generate the full dataset.

* [ ] Run BIO tagging on all articles
* [ ] Store per sample:

  * [ ] `input_ids`
  * [ ] `attention_mask`
  * [ ] `labels`
  * [ ] `article_id`
* [ ] Save dataset as `data/processed/span_ner.jsonl`
* [ ] Sanity checks:

  * [ ] token length == label length
  * [ ] no invalid label ids

✅ **Done when:** Dataset is saved and passes sanity checks.

---

## 📅 Day 5 — Dataset Validation & Statistics

**Objective:** Validate dataset quality.

* [ ] Compute label frequency distribution
* [ ] Count total tokens
* [ ] Count propaganda vs non‑propaganda tokens
* [ ] Count tokens per technique
* [ ] Identify extremely rare techniques
* [ ] Visually inspect random samples

✅ **Done when:** Dataset stats are documented.

---

## 📅 Day 6 — Documentation Day

**Objective:** Lock Week 2 work cleanly.

* [ ] Document BIO tagging approach
* [ ] Document overlap handling strategy
* [ ] Add dataset stats to NOTES.md / README
* [ ] Write assumptions & limitations

✅ **Done when:** Someone else could reproduce Week 2.

---

## 📅 Day 7 — Reflection & Prep

**Objective:** Prepare for Week 3 (NER training).

* [ ] Reflect on BIO tagging challenges
* [ ] Note tricky edge cases
* [ ] List fixes/improvements for training
* [ ] Plan Week 3 tasks
* [ ] Chill (mandatory 😌)

✅ **Done when:** You feel mentally ready for training.

---

## 🎯 End of Week 2 Deliverables

* [ ] `span_ner.jsonl` dataset
* [ ] Label vocabulary (label → id)
* [ ] Dataset statistics summary
* [ ] Clear documentation

🔥 **Week 3 = Teach the model to see manipulation.**
