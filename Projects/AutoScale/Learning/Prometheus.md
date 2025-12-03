
[[Projects/AutoScale/Learning/Week 2#🧠 What you learn|Week 2 Day1]]

## What is a scrape target:

- A scrape target is a URL that Prometheus visits and store whatever it gives as output.
- it can be any URL you feed into it like:
    localhost:8080/metrics
    your-api:8000/metrics

- Now Prometheus call this URL from time to time and save the output

## Prometheus Data Model:

- Prometheus stores data in a custom data model, it's a time series.
- It has four components — A metric name + label + timestamp → value.
- Example:
```
   http_requests_total{method="GET", handler="/"}  1023
```

This means:

- Metric name: `http_requests_total`
    
- Labels: method=GET, handler=/
    
- Value: 1023
    
- Timestamp: when it was recorded

## What are Labels

- labels are metadata for the metrics
- a handler is a label to the path of the URL.
- we can separate a single metric into multiples sub-metrics by this.
```
requests_total{method="GET"}
requests_total{method="POST"}
requests_total{handler="/login"}
requests_total{handler="/api"}
```

