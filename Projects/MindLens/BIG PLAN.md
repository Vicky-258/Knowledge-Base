# 🧠 MindLens — Updated 4‑Month Roadmap (Complete Manipulation Detection Ecosystem)

This is the expanded and explicit roadmap for building **MindLens**, not just as a propaganda span detector but as a **complete manipulation‑detection ecosystem**.

We extend the plan from 3 months → **4 months**, because the original vision includes multiple modules:

- Propaganda span detection (NER)
    
- Emotional manipulation / sentiment analysis
    
- Clickbait detection
    
- Bias & framing analysis
    
- Statistical manipulation & source vagueness
    
- Unified Truth Lens scoring system
    

This expanded plan reflects realistic engineering pace while maintaining high quality.

---

# 🔥 MONTH 1 — Core Modeling: Propaganda Span Detection (NER)

Goal: Build a research‑grade **BIO‑tagged technique‑aware span detection model**.

---

## 📅 WEEK 1 — Dataset + Baselines (Already Done)

**Goal:** Establish a working foundation.

- Download, extract, and parse SemEval dataset
    
- Merge article + span + technique labels
    
- Save raw dataset → `data/raw/`
    
- Clean text & validate spans → `data/processed/`
    
- Build baseline TF‑IDF model (binary classification)
    
- Build baseline DistilBERT model (binary)
    
- Evaluate & record weaknesses
    

**Outcome:** Foundation is complete.

---

## 📅 WEEK 2 — BIO Tagging Dataset Construction

**Goal:** Prepare the TRUE dataset needed for span detection.

Tasks:

- Tokenize articles with DistilBERT tokenizer
    
- Convert spans + techniques → **BIO tags**
    
- Choose strategy for overlapping spans
    
- Create label vocabulary for all techniques
    
- Verify token‑to‑span alignment
    
- Save dataset → `data/processed/span_ner.jsonl`
    
- Document dataset stats
    

**Outcome:** NER‑ready dataset.

---

## 📅 WEEK 3 — Train DistilBERT NER (Technique‑Aware)

**Goal:** Train the first real propaganda span detector.

Tasks:

- Build `PropagandaNERDataset`
    
- Train `DistilBertForTokenClassification`
    
- Handle imbalance with class weights
    
- Use linear warmup scheduler
    
- Evaluate:
    
    - token‑level accuracy
        
    - span‑level F1
        
    - technique‑level F1
        
- Visualize predicted spans
    

**Outcome:** MindLens Span Detector v1.0.

---

## 📅 WEEK 4 — Model Upgrades + Deep Error Analysis

**Goal:** Improve the model with stronger architectures.

Tasks:

- Train RoBERTa‑base NER
    
- Train DeBERTa‑base NER
    
- Compare all models
    
- Analyze technique confusions
    
- Identify missing traits for curated dataset
    
- Select production candidate
    

**Outcome:** Finalize MindLens Propaganda NER Model v1.0.

---

# ⚙️ MONTH 2 — Expanding Capabilities (Manipulation Modules)

Goal: Extend MindLens beyond propaganda → full manipulation detection ecosystem.

---

## 📅 WEEK 5 — Emotional Manipulation & Sentiment Module

Tasks:

- Build sentiment classifier (fear, anger, joy, sadness)
    
- Add emotional‑language detector
    
- Highlight emotionally charged spans
    
- Combine emotional scores with NER output
    

**Outcome:** Emotional manipulation detection module.

---

## 📅 WEEK 6 — Clickbait & Headline Manipulation Module

Tasks:

- Build rule‑based + ML clickbait classifier
    
- Detect exaggeration, sensationalism, withholding info
    
- Integrate with article headline pipeline
    

**Outcome:** Clickbait detection module.

---

## 📅 WEEK 7 — Bias & Framing Analysis Module

Tasks:

- Implement framing detection (positive/negative stance)
    
- Entity‑level sentiment
    
- Subjectivity scoring
    
- Detect ideological framing
    

**Outcome:** Bias/framing module.

---

## 📅 WEEK 8 — Statistical Manipulation & Source Vagueness Module

Tasks:

- Detect vague sources ("experts say", "research shows")
    
- Highlight statistical manipulation ("up to 300%", "study proves…")
    
- Combine rule‑based + transformer classifier
    

**Outcome:** Advanced manipulation module.

---

# 🏗️ MONTH 3 — Pipeline & MLOps Foundations

Goal: Convert models → modular, maintainable ML system.

---

## 📅 WEEK 9 — Pipeline Foundation

Tasks:

- Create `src/` structure:
    
    - `data_pipeline.py`
        
    - `preprocess.py`
        
    - `ner_dataset_builder.py`
        
    - `emotion_module.py`
        
    - `clickbait_module.py`
        
    - `bias_module.py`
        
    - `stats_module.py`
        
    - `train_ner.py`
        
    - `evaluate_ner.py`
        
- Add `config.yaml`
    
- Convert notebooks → proper modules
    

**Outcome:** Maintainable ML architecture.

---

## 📅 WEEK 10 — MLflow Integration

Tasks:

- Track experiments (NER + other modules)
    
- Log metrics, artifacts, confusion matrices
    
- Manage multiple model versions
    

**Outcome:** Full experiment tracking.

---

## 📅 WEEK 11 — Model Registry + Versioning

Tasks:

- Register NER + sentiment + clickbait models
    
- Semantic versioning
    
- Add loader that fetches model from registry
    

**Outcome:** Real model lifecycle system.

---

## 📅 WEEK 12 — Inference API (FastAPI)

Tasks:

- Unified `/analyze` endpoint
    
- Output:
    
    - spans + techniques
        
    - emotional analysis
        
    - clickbait score
        
    - bias/framing cues
        
    - vague/statistical manipulation flags
        
    - final Truth Score
        
- Add structured logging
    

**Outcome:** MindLens becomes a real multi‑module inference API.

---

# 📡 MONTH 4 — Deployment, Monitoring, Feedback Loop, UI

Goal: Productionize MindLens and build an impressive demonstration.

---

## 📅 WEEK 13 — Containerization (Docker)

Tasks:

- Dockerfiles for training + inference
    
- Docker Compose for local simulation
    
- Multi‑module serving environment
    

**Outcome:** Containerized, deployable system.

---

## 📅 WEEK 14 — Monitoring + Observability

Tasks:

- Prometheus metrics:
    
    - request count
        
    - latency
        
    - errors
        
    - technique distribution
        
- Optional Grafana dashboard
    
- Structured logs for each module
    

**Outcome:** Fully observable ML system.

---

## 📅 WEEK 15 — Human Feedback Loop

Tasks:

- `/feedback` endpoint
    
- Store corrected spans, sentiments, bias labels
    
- Build `retrain.py` to incorporate feedback
    
- Push new versions to registry
    

**Outcome:** MindLens becomes self‑improving.

---

## 📅 WEEK 16 — Final Polish + Demo

Tasks:

- Clean README
    
- Architecture diagram
    
- Comparison charts (baseline → final)
    
- Frontend demo notebook or mini UI
    
- Optional 1–2 minute demo video
    

**Final Outcome:**  
A complete, production‑grade **Manipulation Detection Ecosystem** with:

- Propaganda span detection (NER)
    
- Emotional manipulation detection
    
- Clickbait detection
    
- Bias/framing analysis
    
- Statistical manipulation detection
    
- Unified Truth Score system
    
- ML pipeline + MLOps
    
- Monitoring + logging
    
- Versioning
    
- Deployment
    
- Feedback loop
    

**MindLens v1.0 — The Truth Lens — is born.****