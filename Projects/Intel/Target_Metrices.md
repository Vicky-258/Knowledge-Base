## Text Extraction + OCR

### **Metric 1: Accuracy (OCR/ Extraction Quality)**

- Use **Word Error Rate (WER)** = (substitutions + deletions + insertions) / total words.
    
- Target: **<15% WER** on scanned PDFs.
    
- For digital PDFs, accuracy should be >95%.
### **Metric 2: Speed**

- Pages processed per second (pp/s).
    
- Target: **≥2 pages/sec** on average laptop CPU.
    
### **Metric 3: Memory Usage**
    
- Peak RAM usage while processing 100-page PDF.
    
-  Target: <500 MB (to avoid bloat).

## Section Splitting

- **Metric 1: Segmentation Accuracy**
    
    - % of chapters/sections correctly segmented (manual check).
        
    - Target: ≥85%.
        
- **Metric 2: Speed**
    
    - Processing time per 100-page document.
        
    - Target: ≤5 sec overhead on top of text extraction.
        

---

## Table Extraction

- **Metric 1: Structural Accuracy**
    
    - Precision = % of correctly extracted cells out of extracted.
        
    - Recall = % of actual table cells extracted.
        
    - Target: ≥80% precision & recall.
        
- **Metric 2: Query Latency (NoSQL retrieval)**
    
    - Time to fetch a table given a query.
        
    - Target: ≤50 ms for MongoDB on 10k tables.
        

---

## Images & Charts

- **Metric 1: Caption Quality**
    
    - Manual eval: % of images with meaningful captions.
        
    - Target: ≥70% correctness.
        
- **Metric 2: Processing Time**
    
    - Time to process & caption each image.
        
    - Target: ≤1 sec/image (baseline OCR captions).
        

---

## Vector Search (Text & Images)

- **Metric 1: Latency**
    
    - Average query time.
        
    - Target: ≤100 ms/query on 10k chunks.
        
- **Metric 2: Recall@k**
    
    - % of correct answers in top-k retrieved results (measured via gold labels).
        
    - Target: Recall@5 ≥85%.
        
- **Metric 3: Indexing Speed**
    
    - Embeddings created + indexed per sec.
        
    - Target: ≥200 chunks/sec.
        

---

## End-to-End Search System

- **Metric 1: Query Latency**
    
    - Full round trip (UI → DB → result).
        
    - Target: ≤500 ms for single query.
        
- **Metric 2: Throughput**
    
    - Queries handled per second under load.
        
    - Target: ≥10 queries/sec on modest infra.
        
- **Metric 3: User Satisfaction (Relevance)**
    
    - Manual rating of search results (Good/Okay/Bad).
        
    - Target: ≥80% “Good/Okay”.
        

---