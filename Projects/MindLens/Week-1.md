# 📌 MindLens — Week 1 Task Board
Goal of the week → Load dataset → Clean → Train 2 baselines → Compare → Document

---

## ✅ Status Legend
- [ ] Not started
- [/] In progress
- [x] Completed

---

## 📅 Day 1 — Dataset Setup
- [x] Download Propaganda dataset (SemEval / NLP4IF)
- [x] Create project folders (data/raw, data/processed, notebooks, src)
- [x] Place dataset into `data/raw/`
- [x] Inspect dataset in a notebook (columns, labeling, distribution)

---

## 📅 Day 2 — Light Data Cleaning
- [x] Text cleaning (lowercase, remove URLs/HTML, normalize spaces)
- [x] Label cleaning
- [x] Save cleaned dataset into `data/processed/`

---

## 📅 Day 3 — Baseline Model #1 (TF-IDF + Logistic Regression)
- [x] Vectorize text using TF-IDF
- [x] Train logistic regression classifier
- [x] Evaluate performance (accuracy, precision, recall, F1)
- [x] Export confusion matrix

---

## 📅 Day 4 — Baseline Model #2 (DistilBERT)
- [x] Tokenize dataset using HF tokenizer
- [x] Fine-tune DistilBERT on the dataset
- [x] Evaluate using same metrics as Day 3
- [x] Save results screenshot or table

---

## 📅 Day 5 — Comparison & Insights
- [x] Compare results of Model 1 vs Model 2
- [x] Analyze 3 types of errors:
  - severe class imbalance
  - article-level labels too coarse
  - fewer negative samples
- [x] Write insights in a notebook or NOTES.md

---

## 📅 Day 6 — Documentation Day
- [ ] Implement class-weighted cross entropy
- [ ] Compare to Day 4 results
- [ ] Document differences

---

## 📅 Day 7 — Reflection (No Code Day)
- [ ] No coding
- [ ] Write a short Week 1 reflection
- [ ] Plan Week 2 goals (BIO tagging + NER prep)
- [ ] Chill (mandatory 🙂)

---

### 🏁 End-of-Week Checklist
- [x] Project structure = ready
- [x] Dataset = parsed and cleaned
- [ ] Baseline models = implemented
- [ ] Clear understanding of dataset weaknesses
- [ ] Prepared for **Week 2: BIO tagging dataset construction**
