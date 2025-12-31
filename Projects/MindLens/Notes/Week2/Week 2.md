# 🧠 MindLens — Week 2 Checklist (BIO Tagging Dataset Construction)

**Goal:** Prepare the TRUE dataset needed for span‑level, technique‑aware propaganda detection (NER).

This week is about **data correctness, alignment, and decisions**. No rushing. No shortcuts.

---

## 📅 Day 1 — Label Design & Strategy

**Objective:** Decide *exactly* what the model will predict.

* [x] List all propaganda techniques present in the dataset
* [x] Decide BIO format:

  `B-<TECHNIQUE>`
   `I-<TECHNIQUE>`
  `O`
* [x] Decide overlap handling strategy:

  * [x] Priority-based (dominant technique wins)
  * [x] Merge / drop secondary overlaps (if needed)
* [x] Create final label → id mapping
* [x] Write down all decisions in NOTES.md

✅ **Done when:** Label scheme is frozen and unambiguous.

---

## 📅 Day 2 — Tokenization & Alignment Exploration

**Objective:** Understand how text spans map to tokens.

* [x] Load `DistilBertTokenizerFast`
* [x] Tokenize 2–3 sample articles
* [x] Inspect `input_ids`
* [x] Inspect `offset_mapping`
* [x] Manually verify char → token alignment
* [x] Identify edge cases (punctuation, quotes, whitespace)
* [x] Note alignment pitfalls

✅ **Done when:** You trust the tokenizer mapping.

---

## 📅 Day 3 — Span → BIO Tag Conversion Logic

**Objective:** Implement correct BIO labeling.

* [x] Initialize all tokens as `O`
* [x] Iterate through propaganda spans
* [x] Assign `B-TECHNIQUE` to first token in span
* [x] Assign `I-TECHNIQUE` to continuation tokens
* [x] Handle spans starting/ending mid-token
* [x] Apply overlap strategy consistently
* [x] Unit-test BIO logic on sample articles

✅ **Done when:** BIO tags look correct for samples.

---

## 📅 Day 4 — Build Full NER Dataset

**Objective:** Generate the full dataset.

* [x] Run BIO tagging on all articles
* [x] Store per sample:

  * [x] `input_ids`
  * [x] `attention_mask`
  * [x] `labels`
  * [x] `article_id`
* [x] Save dataset as `data/processed/span_ner.jsonl`
* [x] Sanity checks:

  * [x] token length == label length
  * [x] no invalid label ids

✅ **Done when:** Dataset is saved and passes sanity checks.

---

## 📅 Day 5 — Dataset Validation & Statistics

**Objective:** Validate dataset quality.

* [x] Compute label frequency distribution
* [x] Count total tokens
* [x] Count propaganda vs non‑propaganda tokens
* [x] Count tokens per technique
* [x] Identify extremely rare techniques
* [x] Visually inspect random samples

✅ **Done when:** Dataset stats are documented.

---

## 📅 Day 6 — Documentation Day

**Objective:** Lock Week 2 work cleanly.

* [x] Document BIO tagging approach
* [x] Document overlap handling strategy
* [x] Add dataset stats to NOTES.md / README
* [x] Write assumptions & limitations

✅ **Done when:** Someone else could reproduce Week 2.

---

## 📅 Day 7 — Reflection & Prep

**Objective:** Prepare for Week 3 (NER training).

* [x] Reflect on BIO tagging challenges
* [x] Note tricky edge cases
* [x] List fixes/improvements for training
* [x] Plan Week 3 tasks
* [x] Chill (mandatory 😌)

✅ **Done when:** You feel mentally ready for training.

---

## 🎯 End of Week 2 Deliverables

* [x] `span_ner.jsonl` dataset
* [x] Label vocabulary (label → id)
* [x] Dataset statistics summary
* [x] Clear documentation

🔥 **Week 3 = Teach the model to see manipulation.**
