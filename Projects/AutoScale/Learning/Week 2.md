# 🚀 WEEK 2 — Day-by-Day Plan

_(Monitoring: Prometheus + cAdvisor + Metrics)_

---

# **📅 DAY 1 — Prometheus Basics: Your First Time Series**

### 🎯 Goal:

Understand HOW Prometheus thinks and get comfy with its UI.

### 🧠 What you learn:

- ~~What is a scrape target~~ [[Prometheus#What is a scrape target|ans]]
    
- ~~Prometheus data model~~ [[Prometheus#Prometheus Data Model|ans]]
    
- ~~Labels~~ [[Prometheus#What are Labels|ans]]
    
- ~~Time series~~ [[Prometheus#Time series]ans]
    
- ~~Why Prometheus PULLS instead of pushes~~ 
    


### 🛠️ Tasks:

1. ~~Run Prometheus locally:
    
    ```
    docker run -p 9090:9090 prom/prometheus
    ```
    
2. ~~Open `localhost:9090`
    
3. ~~Try basic queries:
    
    - `up`
        
    - `prometheus_http_requests_total`
        
    - `rate(prometheus_http_requests_total[1m])`
        

### 🎉 End of Day Win:

You can run PromQL like a baby Grafana wizard.

---

# **📅 DAY 2 — cAdvisor: Container Gym Trainer**

### 🎯 Goal:

Understand container-level metrics: CPU, memory, IO, network.

### 🛠️ Tasks:

1. ~~Run cAdvisor:~~ 
    
    ```
    docker run \
      --volume=/:/rootfs:ro \
      --volume=/var/run:/var/run:ro \
      --volume=/sys:/sys:ro \
      --volume=/var/lib/docker/:/var/lib/docker:ro \
      --publish=8080:8080 \
      gcr.io/cadvisor/cadvisor
    ```
    
1. ~~Visit `localhost:8080`~~ 
    
2. ~~Explore:
    
    - ~~CPU throttling
        
    - ~~Memory working set
        
    - ~~Filesystem usage
        
    - ~~Network bytes
        
3. ~~In Prometheus:  ~~
    Query:
    
    ~~- `container_cpu_usage_seconds_total`~~
        
    ~~- `container_memory_working_set_bytes`~~
        
    ~~- `rate(container_network_transmit_bytes_total[1m])`~~
        

### 🎉 End of Day Win:

You understand what your containers eat for breakfast (CPU) and how much they sleep (idle time).

---

# **📅 DAY 3 — Build a Micro API With Metrics**

### 🎯 Goal:

Make your own service that exposes real metrics.

### 🛠️ Tasks:

Create FastAPI server with:

- ~~Counter
    
- ~~Histogram
    
- ~~`/metrics` endpoint
    

Code (you’ll write it line-by-line—you prefer that 😎 but here's the plan):

1. ~~Counter: `demo_requests_total`
    
2. ~~Latency histogram: `demo_request_latency_seconds`
    
3. ~~Add random sleep to simulate real latency
    
4. ~~Expose `/metrics`
    

Run:

```
uvicorn main:app --port 8000
```

### 🎉 End of Day Win:

You made your first “self-monitoring API.”

---

# **📅 DAY 4 — Prometheus Scrapes Your API**

### 🎯 Goal:

Connect your API + cAdvisor → Prometheus.

### 🛠️ Tasks:

Edit Prometheus config:

```yaml
scrape_configs:
  - job_name: "python-api"
    static_configs:
      - targets: ["host.docker.internal:8000"]

  - job_name: "cadvisor"
    static_configs:
      - targets: ["host.docker.internal:8080"]
```

Restart Prometheus.

Verify:

- `demo_requests_total`
    
- `rate(demo_requests_total[1m])`
    
- `histogram_quantile(...)`
    

### 🎉 End of Day Win:

Prometheus collects metrics **you** created.

You are officially a “metrics exporter.”

---

# **📅 DAY 5 — Load Testing + Visualizing CPU & Latency**

### 🎯 Goal:

Make graphs for:

- CPU usage
    
- p95 latency
    
- Request rate (RPS)
    

### 🛠️ Tasks:

1. Load-test:
    
    ```
    while true; do curl -s localhost:8000 >/dev/null; done
    ```
    
2. In Prometheus graph tab, inspect:
    

#### 🔥 CPU:

```
rate(container_cpu_usage_seconds_total[1m])
```

#### 🕒 p95 latency:

```
histogram_quantile(0.95, rate(demo_request_latency_seconds_bucket[5m]))
```

#### ⚡ RPS:

```
rate(demo_requests_total[1m])
```

### 🎉 End of Day Win:

You read CPU, RPS, latency from charts.  
This is literally what Grafana panels show.

---

# **📅 DAY 6 — Build Your “Mini Grafana”**

### 🎯 Goal:

Plot metrics together using Prometheus graph or a tiny Python plotting script.

### 🛠️ Tasks:

- Export metrics using Prometheus HTTP API
    
- Plot using matplotlib
    
- OR use Prometheus “graph” tab like a dashboard
    
- (optional) Use `promlens` to debug PromQL
    

### 🎉 End of Day Win:

You basically created a Grafana-lite.

---

# **📅 DAY 7 — Review + Apply**

### 🎯 Goal:

Wrap the week with deep understanding.

### 🛠️ Tasks:

- Review queries you used
    
- Write down top 10 useful PromQL expressions
    
- Note down the difference between:
    
    - Counter
        
    - Gauge
        
    - Histogram
        
- Try to detect: “When does CPU spike?”
    

### 🎉 End of Week Win:

You now **understand real-world metrics**.  
You can debug CPU, memory, latency, and RPS like an SRE trainee.
