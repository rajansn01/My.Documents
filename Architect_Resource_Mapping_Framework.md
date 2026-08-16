# Architect Resource Mapping Framework

**Purpose:** Guidance for product and software architects on how to map product business value to scalability requirements and infrastructure resources such as compute, storage, database, messaging, and cost.

**Context:** Product-based company, product architecture, software architecture, platform thinking, scalability planning, and resource estimation.

---

## 1. Product-Based Company: What an Architect Should Know

In a product-based company, an architect should think beyond individual features. The architect must connect product strategy, business value, customer impact, platform capability, scalability, reliability, cost, and long-term maintainability.

A senior engineer usually asks:

```text
How should we build this?
```

A strong architect asks:

```text
Should we build this?
Where should it live?
Who will own it?
How will it evolve?
What happens in 3 to 5 years?
What business value does this architecture protect?
```

### Key Knowledge Areas for a Product Architect

1. Product strategy
2. Domain knowledge
3. System architecture
4. Product vs platform thinking
5. Scalability architecture
6. Reliability architecture
7. Data architecture
8. Security architecture
9. Integration architecture
10. Technology strategy
11. Operational excellence
12. Cost and financial architecture
13. Organizational architecture
14. Architectural decision records
15. Long-term architecture evolution

---

## 2. Architect's Resource Mapping Framework

The core framework is:

```text
Business Strategy
    -> Business Capability
    -> Product Features
    -> Workload Characteristics
    -> Scalability Targets
    -> Resource Model
    -> Cost Model
    -> Architecture Decisions
```

A more execution-focused version is:

```text
Business Goal
    -> Business Capability
    -> Business Value
    -> Workload Drivers
    -> Scalability Targets
    -> Resource Model
    -> SLA / Reliability
    -> Cost Model
    -> Architecture Decision
```

The purpose of this framework is to avoid starting from infrastructure too early. Instead of asking, "How many servers do we need?", the architect starts by asking, "What business outcome are we protecting, and what level of investment is justified?"

---

## 3. Step 1: Understand the Business Goal

Never begin with compute, storage, queues, or databases. Start with the business problem.

### Questions to Ask

- What business problem are we solving?
- Why is this important now?
- What customer pain point does this address?
- What revenue, retention, compliance, or operational outcome depends on this?
- Is this a strategic product capability or a tactical feature?

### Example

Business goal:

```text
Customers want to receive notifications when a document merge completes or fails.
```

Do not immediately jump to:

```text
Create Notification API
Create Notification DB
Create Notification UI
```

First understand the business impact:

```text
Customers are waiting manually.
Support tickets increase.
Automation workflows are blocked.
Enterprise customers expect better visibility.
```

---

## 4. Step 2: Identify the Business Capability

Architects should identify the underlying capability behind a product request.

### Feature Thinking

```text
Composer needs merge notification.
```

### Platform Thinking

```text
This reveals an Event Notification capability.
```

Potential consumers may include:

```text
Composer
Sign
CLM
CPQ
Future products
```

If multiple products need the same pattern, avoid building separate product-specific implementations. Instead, define a reusable platform capability with stable contracts.

---

## 5. Step 3: Assess Business Value

Before allocating infrastructure investment, classify the workload based on business value.

### Business Value Factors

| Factor | Description |
|---|---|
| Revenue impact | Is this tied to revenue generation or retention? |
| Customer impact | How many customers are affected? |
| Strategic importance | Is this aligned with product roadmap or platform strategy? |
| Operational importance | Does it reduce support, manual work, or operational risk? |
| Compliance impact | Is this required for audit, data protection, or regulation? |

### Simple Scoring Model

Use a 1 to 5 score for each factor.

```text
Business Value Score =
Revenue Impact
+ Customer Impact
+ Strategic Importance
+ Operational Importance
+ Compliance Impact
```

### Example

Contract generation:

```text
Revenue impact = 5
Customer impact = 5
Strategic importance = 5
Operational importance = 3
Compliance impact = 3
Total = 21
```

Internal report export:

```text
Revenue impact = 1
Customer impact = 2
Strategic importance = 1
Operational importance = 3
Compliance impact = 1
Total = 8
```

Higher business value justifies stronger scalability, reliability, observability, and cost investment.

---

## 6. Step 4: Convert Business Metrics into Technical Demand

Business metrics must be converted into workload drivers before they can be used for resource planning.

### Avoid Vague Metrics

```text
100,000 customers
```

This is not directly useful for infrastructure sizing.

### Convert Into Technical Metrics

```text
Daily active users
Concurrent users
Requests per second
Transactions per second
Documents per day
Messages per second
Storage growth per month
Peak burst factor
Retention period
```

### Example Conversion

Business input:

```text
100,000 customers
```

Technical demand model:

```text
10,000 active users per day
1,000 active users per hour
200 concurrent users
50 requests per second peak
5 million documents per month
```

Now the architect can estimate compute, storage, database load, queue size, and cost.

---

## 7. Step 5: Define Scalability Targets

Scalability should be defined across time horizons, not only based on today's load.

### Recommended Time Horizons

```text
Current state
6 months
1 year
3 years
```

### Example

| Metric | Today | 1 Year | 3 Years |
|---|---:|---:|---:|
| Customers | 100K | 500K | 2M |
| Notifications per day | 2M | 10M | 50M |
| Peak events per second | 500 | 2,500 | 12,500 |
| Storage | 2 TB | 10 TB | 80 TB |

Architectural decisions should survive expected growth. If the architecture cannot support the 3-year model, either redesign now or define a clear migration path.

---

## 8. Step 6: Build the Resource Model

Resource mapping converts workload demand into infrastructure requirements.

---

### 8.1 Compute Model

Use this model:

```text
Peak TPS x Processing Cost = CPU Requirement
```

Example:

```text
Peak TPS = 500
One pod capacity = 100 TPS
```

Required pods:

```text
500 / 100 = 5 pods
```

Add high availability and buffer:

```text
5 pods + HA buffer = 8 pods
```

Compute decision:

```text
API service requires 8 pods at peak load.
Autoscaling should be configured based on CPU, memory, queue depth, or request rate.
```

---

### 8.2 Storage Model

Use this model:

```text
Records per day x Average record size x Retention period = Storage Requirement
```

Example:

```text
2 million documents per month
Average size = 1 MB
Retention = 36 months
```

Storage growth:

```text
2 TB per month
36 months = 72 TB
```

Storage decisions:

```text
Use object storage for documents.
Use lifecycle policies for archival.
Use compression where appropriate.
Separate hot, warm, and cold storage.
```

---

### 8.3 Messaging Model

Use this model:

```text
Events per second x Burst Factor x Buffer Duration = Queue Capacity
```

Example:

```text
Peak events per second = 500
Buffer duration = 1 hour
```

Queue capacity:

```text
500 x 3600 = 1,800,000 messages
```

Messaging decisions:

```text
Use queue-based processing.
Add dead-letter queue.
Support retries.
Track queue depth.
Scale workers based on queue depth.
```

---

### 8.4 Database Model

Consider:

```text
Read TPS
Write TPS
Record growth
Index size
Retention
Query patterns
Archival needs
Partitioning strategy
```

Example:

```text
2 million notifications per day
90-day retention
Total records = 180 million
```

Database decisions:

```text
Partition by tenant, date, or event type.
Avoid unbounded tables.
Archive older records.
Add read replicas if query volume is high.
```

---

## 9. Step 7: Map Business Value to SLA and Reliability

Not every feature needs the same reliability investment.

### SLA Mapping

| Business Value | Suggested Availability | Architecture Direction |
|---|---:|---|
| Mission critical | 99.99% | Multi-region, active-active, zero data loss where required |
| Revenue critical | 99.95% | Multi-AZ, strong redundancy, automated recovery |
| Customer facing | 99.9% | HA, autoscaling, backup and restore |
| Internal operational | 99.5% | Single region may be acceptable |
| Low value / admin | Best effort | Basic recovery and backup |

### Reliability Questions

- What downtime is acceptable?
- Is data loss acceptable?
- What is the recovery time objective?
- What is the recovery point objective?
- Can the system degrade gracefully?
- Should processing be synchronous or asynchronous?

### Example

For event notifications:

```text
Availability = 99.95%
RPO = 0 for accepted events
95% of notifications delivered within 30 seconds
Failed deliveries moved to DLQ
Retries supported
```

---

## 10. Step 8: Build the Cost Model

Architects should express infrastructure cost in product and business terms, not only cloud resource terms.

### Avoid Only Saying

```text
AWS cost is $10,000 per month.
```

### Say Instead

```text
Cost per merge
Cost per notification
Cost per quote
Cost per contract
Cost per API request
```

### Example

```text
Monthly infrastructure cost = $3,000
Monthly notifications = 60,000,000
```

Cost per notification:

```text
$3,000 / 60,000,000 = $0.00005
```

This helps leadership understand business value per dollar spent.

---

## 11. Step 9: Select the Architecture Pattern

The architecture pattern should be selected only after business value, scale, SLA, and cost are understood.

### Low Business Value

```text
Single region
Basic backup
Manual recovery acceptable
Limited observability
```

### Medium Business Value

```text
Multi-AZ
Autoscaling
Managed database
Standard monitoring
Reasonable retry handling
```

### High Business Value

```text
Queue-based processing
High availability
Strong observability
Automated recovery
DLQ and replay
Multi-region if justified
Zero or near-zero data loss where needed
```

---

## 12. Real Project Example: Event Notification Capability

### Business Request

```text
Composer customers want to receive notifications when a merge completes or fails.
```

### Step 1: Business Goal

```text
Improve customer visibility into merge processing.
Reduce manual waiting.
Enable downstream automation.
Reduce support tickets.
```

### Step 2: Capability

```text
Event Notification Capability
```

### Step 3: Consumers

```text
Composer
Sign
CLM
CPQ
Future products
```

### Step 4: Business Value

```text
Revenue impact = High
Customer impact = High
Strategic importance = High
Operational importance = Medium
```

### Step 5: Workload Model

```text
2 million merges per day
1 notification per merge
2 million notifications per day
```

Average events per second:

```text
2,000,000 / 86,400 = approximately 23 events per second
```

Peak hour assumption:

```text
25% of traffic during peak hour
500,000 events per hour
500,000 / 3,600 = approximately 139 events per second
```

Burst factor:

```text
3x burst = 417 events per second
Round up to 500 events per second
```

### Step 6: Resource Mapping

API layer:

```text
1 pod supports 100 events per second
500 events per second requires 5 pods
Add HA buffer = 8 pods
```

Queue layer:

```text
500 events per second x 1 hour = 1.8 million message buffer
```

Database layer:

```text
2 million notifications per day
90-day retention = 180 million records
```

Worker layer:

```text
Scale workers based on queue depth and delivery latency.
Use retry and DLQ for failures.
```

### Step 7: SLA and Reliability

```text
Availability = 99.95%
RPO = 0 for accepted notification events
Delivery target = 95% within 30 seconds
Retry supported
DLQ supported
Replay supported
```

### Step 8: Cost Model

```text
Estimated monthly cost = $3,000
Monthly volume = 60 million notifications
Cost per notification = $0.00005
```

### Step 9: Architecture Decision

Recommended architecture:

```text
Generic Event Notification API
Queue-based ingestion
Worker-based delivery
Notification persistence
Retry policy
Dead-letter queue
Observability dashboard
Tenant-aware throttling
Product-level integration contracts
```

---

## 13. One-Page Architect Output Template

Use this template for architecture reviews.

```text
Capability:

Business Goal:

Business Value:
High / Medium / Low

Consumers:

Current Scale:

1-Year Scale:

3-Year Scale:

Peak TPS / Events per Second:

Storage Growth:

Retention:

Availability Target:

RPO:

RTO:

Latency Target:

Resource Model:
- API pods:
- Worker pods:
- Queue capacity:
- Database capacity:
- Storage capacity:

Architecture Pattern:

Estimated Monthly Cost:

Cost per Business Transaction:

Key Risks:

Open Decisions:

ADR Required:
Yes / No
```

---

## 14. Architecture Review Checklist

### Business

- What business problem are we solving?
- What capability does this reveal?
- What revenue or customer impact depends on it?
- Is this product-specific or platform-level?

### Scale

- What is the current volume?
- What is the expected 1-year and 3-year growth?
- What is the peak TPS or events per second?
- What is the burst factor?

### Compute

- What is the processing cost per request?
- How many pods, containers, or instances are required?
- What is the autoscaling trigger?

### Storage

- What data is stored?
- What is the average size?
- What is the retention period?
- Is archival required?

### Messaging

- What is the event rate?
- What is the queue depth target?
- What is the retry policy?
- Is DLQ required?

### Reliability

- What is the availability target?
- What is the RPO?
- What is the RTO?
- Is graceful degradation possible?

### Cost

- What is the monthly infrastructure cost?
- What is the cost per business transaction?
- What is the cost at 10x scale?

### Architecture Decision

- What options were considered?
- Why was this option selected?
- What are the trade-offs?
- Is an ADR required?

---

## 15. Golden Rule

Do not start with technology.

Wrong order:

```text
Technology
    -> Infrastructure
    -> Cost
    -> Business justification
```

Right order:

```text
Business Outcome
    -> Capability
    -> Workload Demand
    -> Scalability Target
    -> Resource Model
    -> Cost Model
    -> Architecture Decision
```

The best architects optimize for:

```text
Business value delivered per dollar spent
```

while ensuring the architecture can scale safely over the next 3 to 5 years.

---

## 16. Final Summary

To apply the Architect Resource Mapping Framework in a real project:

1. Start with the business goal.
2. Identify the underlying business or platform capability.
3. Score business value.
4. Convert business metrics into technical demand.
5. Define current, 1-year, and 3-year scalability targets.
6. Estimate compute, storage, database, and messaging resources.
7. Map business value to SLA, RPO, RTO, and reliability needs.
8. Calculate cost per business transaction.
9. Select the architecture pattern based on value, scale, reliability, and cost.
10. Capture the final decision in an architecture review document or ADR.

A senior engineer asks:

```text
How do we build it?
```

An architect asks:

```text
Why are we building it?
What capability does it reveal?
How much scale is required?
What business value does it generate?
What is the most cost-effective architecture to support it for the next 3 to 5 years?
```
