
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


## Time series:

- A list of the same request with the same label but gotten in a different time.
- This list forms a time series and Prometheus even give graph view for this.

## Why PULL instead of PUSH:

- Reason 1 - Accuracy, Prometheus can check in a continuous time interval without depending on the clients to send it timingly.
- Reason 2 - Control, Prometheus can decide when and how much to get as metrics, so metrics can be ensured to be consistent.
- Reason 3 - Reliability, if it has to be pushed - that can be unreliable if the client dies you won't know if it didn't send out of problem or logical mistakes, but if we pulled Prometheus can give a definite output of 0 if the server doesn't respond.
- Reason 4 - targets don't need to buffer metrics, resend failed requests or anything, they can just expose the endpoint and chill.
