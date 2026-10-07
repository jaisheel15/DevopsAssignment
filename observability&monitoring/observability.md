# Observability

Observability is the ability to understand the internal state of a system by analyzing the data it produces.

Modern distributed systems generate massive amounts of telemetry data. Observability helps engineers answer questions like:

- Is the system healthy?
- Why is the application slow?
- Which service is failing?
- What changed recently?
- Where is the bottleneck?

Observability is built on three major pillars:

1. Metrics
2. Logs
3. Traces

---

# Why Observability is Required

As applications become distributed across:

- Microservices
- Containers
- Kubernetes
- Cloud Infrastructure

Debugging becomes much harder.

Traditional monitoring answers:

```text
Something is broken.
```

Observability answers:

```text
What is broken?
Why is it broken?
Where is it broken?
How do we fix it?
```

### Benefits

- Faster incident response
- Reduced downtime
- Better debugging
- Performance optimization
- Capacity planning
- Improved user experience

---

# The Three Pillars of Observability

```text
              Observability
                     │
     ┌───────────────┼───────────────┐
     │               │               │
     ▼               ▼               ▼
 Metrics          Logs           Traces
```

Each pillar provides a different perspective on system behavior.

---

# Metrics

Metrics are numerical measurements collected over time.

They help answer:

```text
What is happening?
```

Metrics are lightweight and ideal for monitoring trends and system health.

---

## Examples

CPU Usage

```text
CPU = 75%
```

Memory Usage

```text
RAM = 6GB
```

Request Rate

```text
1000 requests/sec
```

Error Rate

```text
5% failed requests
```

Latency

```text
Average Response Time = 120ms
```

---

## Metric Characteristics

- Numerical values
- Time-series data
- Low storage cost
- Easy aggregation
- Good for alerting

---

## Example

```text
Requests/sec

1200 ┤
1000 ┤
 800 ┤
 600 ┤
 400 ┤
 200 ┤
   0 └─────────────► Time
```

---

## Common Metrics

### Infrastructure Metrics

- CPU Usage
- Memory Usage
- Disk Usage
- Network Throughput

### Application Metrics

- Requests Per Second (RPS)
- Response Time
- Error Rate
- Queue Length

### Business Metrics

- Orders Per Minute
- Active Users
- Revenue
- Conversions

---

## Popular Metric Tools

- Prometheus
- Grafana
- Datadog
- New Relic
- CloudWatch
- VictoriaMetrics

---

# Logs

Logs are timestamped records of events occurring within a system.

They help answer:

```text
What exactly happened?
```

---

## Example

```text
2026-10-07 10:15:23 INFO User logged in
2026-10-07 10:15:24 INFO Order created
2026-10-07 10:15:25 ERROR Payment failed
```

---

## Types of Logs

### Application Logs

```text
User registered
Payment processed
API request received
```

### System Logs

```text
Disk mounted
Service restarted
Kernel message
```

### Security Logs

```text
Failed login attempt
Permission denied
```

---

## Log Levels

| Level | Meaning |
|---------|----------|
| DEBUG | Detailed debugging information |
| INFO | Normal operation |
| WARN | Potential issue |
| ERROR | Failure occurred |
| FATAL | Critical failure |

---

## Example

```text
INFO  User login successful
WARN  High memory usage
ERROR Database connection failed
```

---

## Structured Logging

Bad:

```text
User login failed
```

Good:

```json
{
  "timestamp": "2026-10-07T10:15:23Z",
  "level": "ERROR",
  "user_id": "123",
  "message": "Login failed"
}
```

Structured logs are easier to search and analyze.

---

## Popular Log Tools

- ELK Stack (Elasticsearch, Logstash, Kibana)
- OpenSearch
- Loki
- Splunk
- Datadog Logs
- CloudWatch Logs

---

# Traces

Traces track a request as it travels through multiple services.

They help answer:

```text
Where is the problem?
```

Traces are especially useful in microservices architectures.

---

## Example Request

```text
User Request
      │
      ▼
API Gateway
      │
      ▼
Auth Service
      │
      ▼
Payment Service
      │
      ▼
Database
```

A trace follows the request through every step.

---

## Trace Components

### Trace

Represents the entire request.

```text
Checkout Request
```

### Span

Represents one operation inside the trace.

```text
API Gateway
Auth Service
Payment Service
Database Query
```

---

## Example Trace

```text
Trace ID: abc123

Span 1 → API Gateway     20ms
Span 2 → Auth Service    15ms
Span 3 → Payment Service 250ms
Span 4 → Database        10ms
```

Observation:

```text
Payment Service is slow.
```

---

## Distributed Tracing

Modern applications often consist of dozens of services.

```text
Frontend
   │
   ▼
API Service
   │
   ▼
Auth Service
   │
   ▼
Payment Service
   │
   ▼
Database
```

Distributed tracing helps identify bottlenecks across service boundaries.

---

## Popular Trace Tools

- Jaeger
- Zipkin
- OpenTelemetry
- Datadog APM
- New Relic APM
- AWS X-Ray

---

# Metrics vs Logs vs Traces

| Feature | Metrics | Logs | Traces |
|-----------|---------|-------|--------|
| Data Type | Numeric | Events | Request Flow |
| Storage Cost | Low | Medium/High | Medium |
| Alerting | Excellent | Limited | Limited |
| Debugging | Basic | Detailed | End-to-End |
| Trend Analysis | Excellent | Poor | Poor |
| Root Cause Analysis | Limited | Good | Excellent |

---

# How the Three Pillars Work Together

Imagine a checkout service failure.

### Metrics

```text
Error Rate = 20%
```

Metrics tell us:

```text
Something is wrong.
```

---

### Logs

```text
Database connection timeout
```

Logs tell us:

```text
What happened.
```

---

### Traces

```text
Checkout
   ↓
Payment Service
   ↓
Database Timeout
```

Traces tell us:

```text
Where it happened.
```

---

## Combined Flow

```text
Metrics
   │
Detect Problem
   │
   ▼
Logs
   │
Investigate
   │
   ▼
Traces
   │
Find Root Cause
```

---

# OpenTelemetry

OpenTelemetry (OTel) is the modern standard for collecting observability data.

It can collect:

- Metrics
- Logs
- Traces

### Architecture

```text
Application
      │
      ▼
OpenTelemetry SDK
      │
      ▼
OTel Collector
      │
      ▼
Backend
```

Examples:

- Prometheus
- Grafana
- Jaeger
- Datadog

---

# Kubernetes Observability

Kubernetes introduces additional complexity because:

- Pods are ephemeral
- Containers start and stop frequently
- Applications are distributed
- Scaling happens automatically

Observability is critical for Kubernetes environments.

---

# What Should Be Observed?

## Cluster Level

- Node health
- CPU usage
- Memory usage
- Disk usage
- Network traffic

---

## Pod Level

- Pod status
- Restarts
- Resource consumption
- OOMKills

---

## Application Level

- Request rate
- Latency
- Error rate
- Business metrics

---

# Kubernetes Metrics

Important metrics:

### Node Metrics

```text
CPU Usage
Memory Usage
Disk Usage
```

### Pod Metrics

```text
Pod Restarts
Pod CPU
Pod Memory
```

### Application Metrics

```text
Requests/sec
Latency
Error Rate
```

---

# Kubernetes Logging

Logs are collected from containers.

```text
Pod
 │
 ▼
stdout/stderr
 │
 ▼
Log Collector
 │
 ▼
Storage Backend
```

Common collectors:

- Fluent Bit
- Fluentd
- Vector
- Promtail

---

# Kubernetes Tracing

Tracing follows requests across services running in Kubernetes.

Example:

```text
Ingress
   │
   ▼
Frontend Pod
   │
   ▼
Backend Pod
   │
   ▼
Database
```

Distributed tracing helps locate slow services and failed requests.

---

# Popular Kubernetes Observability Stack

## Metrics

```text
Prometheus
     │
     ▼
Grafana
```

Prometheus:

- Collects metrics

Grafana:

- Visualizes metrics

---

## Logs

```text
Fluent Bit
      │
      ▼
Loki
      │
      ▼
Grafana
```

---

## Traces

```text
OpenTelemetry
       │
       ▼
Jaeger
```

---

# Complete Kubernetes Observability Architecture

```text
Applications
      │
      ▼
OpenTelemetry
      │
      ├── Metrics ──► Prometheus ──► Grafana
      │
      ├── Logs ─────► Loki/OpenSearch
      │
      └── Traces ───► Jaeger
```

---

# Golden Signals

Google SRE defines four important metrics called the Golden Signals.

### Latency

```text
How long requests take.
```

### Traffic

```text
How many requests arrive.
```

### Errors

```text
How many requests fail.
```

### Saturation

```text
How busy the system is.
```

---
