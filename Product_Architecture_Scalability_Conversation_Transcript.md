# Product Architecture and Scalability Resource Mapping Conversation Transcript

**Purpose:** Complete Markdown transcript of the session covering product-based company architecture, product/software architect responsibilities, the Architect's Resource Mapping Framework, real-project application, summary steps, and creation of the first Markdown artifact.

**Participants:**

- **User:** Nagarajan Satyanarayana
- **Assistant:** Microsoft Enterprise Copilot

---

## 1. User Request

> Product based company , product and software architecture what are the important thing as architect to know

---

## Assistant Response

I found several architecture-related discussions and artifacts in your organization, including a recent session where architectural thinking was discussed explicitly, plus architecture review forums and platform-capability decision frameworks.

For a **Product-Based Company**, especially in a role like yours (**Software Architect / Platform Architect**), the most important thing is to think beyond individual features and focus on **long-term product evolution, platform strategy, customer impact, and technical sustainability**. This aligns closely with the architectural thinking discussed in your organization's architecture forums, where architects were described as deciding *where complexity should live*, identifying stable models, and balancing new design with existing customer commitments.

# 1. Product Architecture Knowledge

Understand the product as a business, not just software.

### Know:
- Product vision and roadmap
- Customer personas
- Business workflows
- Revenue model
- Competitive differentiators
- Customer pain points

### Key Questions
- Why does this product exist?
- What problem does it solve?
- What makes customers buy it?
- Which features drive adoption and retention?

Many architects fail because they focus only on technology and not product outcomes.

---

# 2. Domain Architecture

Become an expert in the business domain.

Examples:
- CPQ -> Pricing, quoting, approvals
- Banking -> Accounts, payments, settlements
- Healthcare -> Patients, claims, compliance

### Know:
- Core business entities
- Business rules
- Regulatory requirements
- Industry standards

A good architect speaks both:
- Business language
- Technical language

---

# 3. System Architecture

Know the complete system landscape.

### Components
- UI
- APIs
- Services
- Databases
- Messaging
- Search
- External integrations

### Understand
- Request flow
- Dependency graph
- Data ownership
- Failure points

Be able to draw the architecture from memory.

---

# 4. Product vs Platform Thinking

This is especially relevant to your recent Event Notification platform work.

Instead of asking:

```text
How do I build this feature?
```

Ask:

```text
Is this feature exposing a reusable platform capability?
```

The platform capability framework you created emphasizes:
- Extract reusable capabilities
- Expose stable contracts
- Centralize operational concerns
- Avoid product-specific duplication

Examples:
- Notification Service
- Audit Framework
- Identity Service
- Configuration Service
- Event Framework

---

# 5. Scalability Architecture

Know how the system behaves under growth.

### Capacity Planning
- Users
- Transactions
- Storage
- Throughput

### Architecture Topics
- Horizontal scaling
- Partitioning
- Caching
- Asynchronous processing
- Event-driven architecture

Questions:
- What happens if traffic becomes 10x?
- What happens if a customer becomes 100x larger?

---

# 6. Reliability Architecture

Architects own system reliability.

### Understand
- Availability targets
- Disaster recovery
- Multi-region strategy
- MTTR
- RTO / RPO

### Design For
- Retry handling
- Circuit breakers
- Idempotency
- Backpressure
- Graceful degradation

---

# 7. Data Architecture

Know data better than most engineers.

### Understand
- Data lifecycle
- Ownership boundaries
- Retention policies
- Data classification
- Compliance

### Questions
- Who owns the data?
- Where is the source of truth?
- Can it be deleted safely?
- How does it migrate?

---

# 8. Security Architecture

In product companies this is mandatory.

### Areas
- Authentication
- Authorization
- Secrets management
- Encryption
- Audit logging
- Secure defaults

Your organization's design reviews also frequently involve authentication architecture and connected-app strategies, showing how security becomes an architectural concern, not just an implementation detail.

---

# 9. Integration Architecture

Few products live in isolation.

### Understand
- API design
- Event contracts
- Versioning
- Backward compatibility
- Third-party integrations

Especially in enterprise SaaS products:

```text
A bad integration decision can remain for 10 years.
```

---

# 10. Technology Strategy

Architects choose technology carefully.

Evaluate:
- Build vs Buy
- Open Source vs Commercial
- Cost
- Vendor lock-in
- Team skills

Do not chase trends.

Choose technology because it solves a problem.

---

# 11. Operational Excellence

Know how software runs in production.

### Learn
- CI/CD
- GitHub Actions
- Kubernetes
- Helm
- Monitoring
- Observability
- Incident response

A modern architect spends significant time thinking about runtime operations, not just design-time architecture.

---

# 12. Financial Architecture

Often overlooked.

Understand:
- Infrastructure cost
- Cloud spend
- Storage costs
- Scaling economics

As an architect, every architectural choice has a cost implication.

---

# 13. Organizational Architecture

Architecture is also about people and teams.

Know:
- Team boundaries
- Ownership model
- Conway's Law
- Communication paths

Ask:

```text
Can teams develop independently?
```

---

# 14. Architectural Decision Records (ADR)

Strong architects document decisions.

For every major decision:
- Context
- Options
- Trade-offs
- Decision
- Consequences

Your architecture forums already emphasize ADR reviews and standard templates, reinforcing this practice.

---

# 15. The Most Important Architect Mindset

From a feature mindset:

```text
How do we implement this ticket?
```

To an architecture mindset:

```text
What should remain stable for the next 5 years while allowing change everywhere else?
```

This is very close to the architectural-thinking discussion from your recent session with Rob Fowler, where the focus was on identifying stable models, determining where complexity belongs, and balancing modernization with customer compatibility.

## My "Top 10 Knowledge Areas" for a Product Architect

1. Product Strategy
2. Domain Knowledge
3. Distributed Systems
4. Data Architecture
5. Cloud Architecture
6. Security
7. Scalability & Reliability
8. Platform Thinking
9. Operational Excellence
10. Cost & Business Impact

### Rule of Thumb

A senior engineer asks:

```text
How should we build this?
```

A strong architect asks:

```text
Should we build this, where should it live, who will own it, how will it evolve, and what happens in 5 years?
```

That is the mindset that differentiates an architect in a product company.

---

## 2. User Request

> how to map scalability requirements and resources like compute, storage we have to use based on product business value. Provide the guidance around this

---

## Assistant Response

This is one of the most important skills for a Product Architect.

Many architects start from:

```text
How many servers do I need?
```

But the correct approach is:

```text
What business outcome am I protecting, and what level of investment is justified?
```

This aligns well with the platform-thinking framework you recently presented, which emphasizes starting from the capability and business need before choosing implementation.

# The Architect's Resource Mapping Framework

```text
Business Value
      ↓
Business Criticality
      ↓
SLA / SLO
      ↓
Scalability Requirement
      ↓
Capacity Model
      ↓
Resource Allocation
      ↓
Cost Model
```

---

# Step 1: Classify Features by Business Value

Before discussing CPUs or storage, classify the workload.

| Tier | Business Impact | Example |
|---|---|---|
| Tier 1 | Revenue Critical | Quote Generation, Checkout, Payment |
| Tier 2 | Customer Experience | Search, Notifications |
| Tier 3 | Operational | Reports, Batch Jobs |
| Tier 4 | Internal | Admin Tools |

Example from Conga context:

### High Value

- Quote creation
- Contract generation
- Document merge
- Pricing engine

### Medium Value

- Event notifications
- Audit history

### Lower Value

- Historical reports
- Analytics export

Not every service deserves the same investment.

---

# Step 2: Define Business SLA

Translate business language into measurable targets.

| Business Statement | Technical Requirement |
|---|---|
| Customer can never lose a contract | RPO = 0 |
| Quote generation should feel instant | < 2 sec |
| Merge completion should finish quickly | < 30 sec |
| Report generation can wait | < 15 min |

Example:

```text
Business Goal:
Generate 1 million contracts/month

SLA:
99.95%

Response Time:
<3 seconds

Availability:
24x7
```

Now architecture has requirements.

---

# Step 3: Define Demand Drivers

Identify what drives resource consumption.

### Compute Drivers

- Active users
- API requests
- Parallel jobs
- Workflow executions

### Storage Drivers

- Documents
- Attachments
- Logs
- Audit Events

### Database Drivers

- Transactions
- Queries
- Writes

### Queue Drivers

- Messages/sec
- Peak bursts

Microsoft's Well-Architected guidance recommends forecasting demand from historical usage, seasonal trends, releases, and business events before sizing resources.

---

# Step 4: Convert Business Metrics Into Technical Metrics

Example:

## Product Requirement

```text
100,000 customers
```

Does not help.

Convert it to:

```text
10,000 active/day
1,000 active/hour
200 concurrent users
5 requests/sec average
50 requests/sec peak
```

Now you can size infrastructure.

---

# Step 5: Build Capacity Models

## Compute Model

```text
Peak RPS = 500

Average Processing Time = 200 ms

CPU Core Capacity = 100 RPS
```

Required CPUs

```text
500 / 100

= 5 cores
```

Add safety buffer

```text
5 × 2

= 10 cores
```

Deploy:

```text
5 pods
2 CPU each
```

---

## Storage Model

Predict growth.

Example:

```text
500,000 documents/month

Average size = 2 MB
```

Monthly Growth

```text
500000 * 2 MB

= 1 TB/month
```

3 Years:

```text
36 TB
```

Now determine:

- Storage tier
- Archival strategy
- Retention period

---

# Step 6: Match Investment to Revenue

This is where architects become business leaders.

Example:

### Feature A

Revenue Impact:

```text
$20M ARR
```

Downtime Cost:

```text
$50k/hour
```

Architecture Decision:

```text
Multi-region
Active-active
High redundancy
```

---

### Feature B

Revenue Impact:

```text
Internal reporting
```

Downtime Cost:

```text
Near zero
```

Architecture:

```text
Single region
Best-effort recovery
```

Same company.

Different investment.

---

# Step 7: Create Scalability Tiers

I recommend every product having a workload classification.

| Tier | Users | Availability | Architecture |
|---|---|---|---|
| Bronze | <10k | 99.5% | Single Region |
| Silver | <100k | 99.9% | HA |
| Gold | <1M | 99.95% | Multi-AZ |
| Platinum | >1M | 99.99% | Multi-region |

This prevents emotional architecture decisions.

---

# Step 8: Use Cost per Business Transaction

This is my favorite architect metric.

Instead of:

```text
EC2 Cost = $10,000
```

Use:

```text
Cost per Quote

Cost per Contract

Cost per Merge

Cost per API Request
```

Example:

```text
Monthly Cost
$5000

Merges
5 million
```

```text
Cost per merge

$0.001
```

Very powerful metric.

---

# Step 9: Design for Growth Stages

Think about business growth.

```text
Today
6 months
1 year
3 years
```

For each:

| Metric | Today | 1 Year | 3 Year |
|---|---|---|---|
| Users | 10k | 50k | 500k |
| Storage | 2 TB | 10 TB | 80 TB |
| TPS | 50 | 500 | 5000 |

This avoids re-platforming surprises.

---

# Step 10: Decide When to Scale Platform vs Product

This is especially relevant to your platform capability approach.

When multiple products need:

- Notifications
- Search
- Audit
- Identity
- Workflow

Create a shared platform capability rather than separate implementations. This is exactly the mindset encouraged in your Think Platform session, moving from solving a single requirement to identifying reusable capabilities.

# Architect Decision Matrix

When evaluating a feature:

| Question | Example |
|---|---|
| Business value? | Revenue critical? |
| Customer impact? | How many customers affected? |
| SLA? | 99.9% or 99.99%? |
| Growth forecast? | 10x in 2 years? |
| Cost target? | <$0.01 per transaction? |
| Shared capability? | Can multiple products use it? |
| Storage growth? | 100GB/month or 10TB/month? |
| Peak load? | 10x seasonal spike? |

# Rule I Use Personally

```text
Business Value
      ×
Customer Impact
      ×
Growth Potential
      ×
SLA Criticality
      =
Maximum Justifiable Infrastructure Investment
```

The best architects do not optimize for technology.

They optimize for **business value delivered per dollar spent**, while ensuring the architecture can scale to the product's future growth.

---

## 3. User Request

> The Architect's Resource Mapping Framewo

---

## Assistant Response

For a **Product Architect**, I would refine the framework into something you can repeatedly apply during grooming, roadmap planning, and architecture reviews.

# Architect's Resource Mapping Framework

```text
Business Strategy
       ↓
Business Capability
       ↓
Product Features
       ↓
Workload Characteristics
       ↓
Scalability Targets
       ↓
Resource Model
       ↓
Cost Model
       ↓
Architecture Decisions
```

---

# 1. Start with Business Strategy

Never begin with infrastructure.

### Example

Business Goal:

```text
Grow Composer usage by 10x
Support Enterprise Customers
Reduce document generation latency
```

Architect Questions:

- What revenue depends on this?
- Which customers depend on this?
- Is this a differentiator?
- Is this strategic for next 3 years?

---

# 2. Identify Business Capability

This aligns with the platform-capability thinking discussed in your organization.

Example:

```text
Customer Requirement:
Send merge completion notifications
```

Do not jump into SQS, Kafka, Kubernetes.

Ask:

```text
What capability is exposed?

Event Notification
```

Capability:

```text
Event Notification Service
```

Products:

```text
Composer
Sign
CLM
CPQ
Future products
```

Now you know the investment is platform-level, not product-level.

---

# 3. Estimate Business Value Score

I use:

```text
Business Value =
Revenue Impact
+
Customer Impact
+
Strategic Importance
+
Operational Importance
```

| Factor | Score |
|---|---|
| Revenue Impact | 1-5 |
| Customer Impact | 1-5 |
| Strategic Importance | 1-5 |
| Operational Importance | 1-5 |

Example:

### Contract Generation

```text
Revenue = 5
Customer Impact = 5
Strategic = 5
Operational = 3

Total = 18
```

### Reporting Export

```text
Revenue = 1
Customer Impact = 2
Strategic = 1
Operational = 3

Total = 7
```

Higher score = more investment justified.

---

# 4. Convert Business Metrics to Technical Demand

Business metrics are useless for sizing.

Convert:

### Business

```text
500 customers
```

Into:

### Technical

```text
50,000 daily users
5,000 concurrent users
500 TPS
5 million documents/day
```

These become capacity drivers.

---

# 5. Define Scalability Targets

For each capability define:

### User Scale

```text
Concurrent users
```

### Throughput Scale

```text
TPS
Events/sec
Messages/sec
```

### Data Scale

```text
Storage growth
```

### Growth Rate

```text
3x
10x
100x
```

Example:

```text
Today:
100 TPS

12 Months:
500 TPS

36 Months:
5000 TPS
```

---

# 6. Build Resource Models

## Compute Model

```text
Peak TPS
×
Processing Cost
=
CPU Requirement
```

Example:

```text
200 TPS

10 ms CPU per request
```

```text
200 × 10 ms

≈ 2 CPU cores
```

Add buffer:

```text
2x safety factor
```

Final:

```text
4 cores
```

---

## Storage Model

Example:

```text
2 million documents/month
```

Average:

```text
1 MB
```

Growth:

```text
2 TB/month
```

Retention:

```text
3 years
```

Storage Requirement:

```text
72 TB
```

Now decisions emerge:

```text
S3
Archive
Lifecycle Rules
Compression
```

---

## Messaging Model

Example:

```text
Event Notifications
```

Business:

```text
1M events/day
```

Technical:

```text
12 peak events/sec average

150 events/sec burst
```

Queue size now becomes measurable.

---

# 7. Map Business Value to Availability

Most architects overdesign.

Not everything needs 99.99%.

| Business Value | SLA |
|---|---|
| Mission Critical | 99.99 |
| Revenue Critical | 99.95 |
| Customer Facing | 99.9 |
| Internal | 99.5 |

Example:

### Quote Generation

```text
99.95%
```

### Internal Reporting

```text
99.5%
```

Different architecture.

Different cost.

---

# 8. Map Business Value to Architecture Pattern

## Low Value

```text
Single Region
Single DB
Nightly Backup
```

---

## Medium Value

```text
Multi-AZ
Auto Scaling
Managed DB
```

---

## High Value

```text
Multi Region
Active Active
Zero Downtime
Real Time Replication
```

---

# 9. Build Cost Per Transaction

Best metric for product companies.

Instead of:

```text
AWS Cost = $50k/month
```

Use:

```text
Cost per Quote

Cost per Merge

Cost per Contract

Cost per API
```

Example:

```text
Infrastructure Cost
$10,000
```

```text
10 million merges
```

Result:

```text
$0.001 per merge
```

Business leaders understand this immediately.

---

# 10. Growth-Based Architecture Planning

Create a 4-stage scale model.

| Stage | Users | TPS | Architecture |
|---|---|---|---|
| Startup | 1K | 10 | Monolith |
| Growth | 10K | 100 | Services |
| Scale | 100K | 1000 | Distributed |
| Enterprise | 1M+ | 10000+ | Multi-region |

---

# Practical Example (Composer Event Notification)

### Business Need

```text
Notify customer when merge completes
```

### Capability

```text
Event Notification Platform
```

### Products

```text
Composer
Sign
CLM
CPQ
```

### Volume

```text
5M events/day
```

### Scalability

```text
Peak 500 events/sec
```

### SLA

```text
99.95%
```

### Resource Estimation

```text
API Pods
Queue Capacity
Worker Pods
Database Storage
Observability Capacity
```

### Architecture Choice

```text
Shared Platform Capability
Generic API
Worker Model
Auto Scaling
Queue-Based Processing
```

# Executive Summary

As an Architect, think in this order:

```text
Business Value
      ↓
Capability
      ↓
Workload
      ↓
Scale
      ↓
SLA
      ↓
Capacity
      ↓
Cost
      ↓
Architecture
```

The mistake many architects make is:

```text
Technology
      ↓
Infrastructure
      ↓
Business
```

The best architects work in the reverse direction:

```text
Business Outcome
      ↓
Architecture Investment
```

This creates systems that are both **scalable** and **economically justified**.

---

## 4. User Request

> Explain how to apply this framework in a real project

---

## Assistant Response

Let's use a **real architect scenario** close to your work at Conga.

# Example Project

### Business Request

```text
Composer customers want to receive notifications
when a merge completes or fails.
```

Many teams immediately think:

```text
Let's build Notification API
Let's create Notification DB
Let's create Notification UI
```

That is solution-first thinking.

The framework forces you to think differently.

---

# Phase 1: Understand Business Value

## Meeting with Product

Ask:

### Business Problem

```text
What happens today?
```

Answer:

```text
Customer waits for merge completion.
No automated notification.
```

### Business Impact

```text
Lost productivity

Support tickets

Customer frustration
```

### Revenue Impact

```text
Enterprise customers requesting this.

Needed for future workflow automation.
```

Now we can classify.

| Area | Score |
|---|---|
| Revenue | High |
| Customer Impact | High |
| Strategic | High |
| Operational | Medium |

Result:

```text
Business Value = High
```

---

# Phase 2: Identify the Capability

Instead of:

```text
Composer Notification Feature
```

Ask:

```text
What capability does this reveal?
```

Answer:

```text
Event Notification Capability
```

Potential consumers:

```text
Composer

Sign

CLM

CPQ

Future products
```

Now this becomes:

```text
Platform Capability
```

not

```text
Composer Feature
```

This is exactly the platform-thinking mindset you have been promoting.

---

# Phase 3: Translate Business to Technical Drivers

Product says:

```text
100,000 customers
```

Not useful.

Convert to:

```text
2 million merges/day

Average merge:
5 seconds

Peak hour:
25% of daily traffic

Peak burst:
3x
```

Now calculate.

---

# Phase 4: Build Workload Model

## Daily Events

```text
2,000,000 merges/day

Success notification
Failure notification
```

Notifications:

```text
2,000,000/day
```

Average:

```text
2,000,000 / 86400

= 23 events/sec
```

---

## Peak Hour

```text
500,000 events/hour
```

```text
500000 / 3600

= 139 events/sec
```

---

## Peak Burst

Apply:

```text
3x burst
```

```text
417 events/sec
```

Now:

```text
Scalability Target

500 events/sec
```

Round upward.

---

# Phase 5: Resource Mapping

## API Layer

Assume:

```text
1 pod

Can process

100 events/sec
```

Needed:

```text
500 / 100

= 5 pods
```

Add HA:

```text
8 pods
```

---

## Queue Layer

Requirement:

```text
500 events/sec burst

1 hour buffering
```

Queue Capacity:

```text
1.8 million messages
```

Now SQS/Kafka sizing becomes clear.

---

## Database Layer

Assume:

```text
2M notifications/day
```

Retention:

```text
90 days
```

Storage:

```text
180M records
```

Now database design becomes measurable.

---

# Phase 6: SLA Mapping

Business asks:

```text
Must not lose notifications
```

Translate:

### Availability

```text
99.95%
```

### Recovery

```text
RPO = 0
```

### Delivery

```text
95% notifications within 30 seconds
```

Now architecture can be selected.

---

# Phase 7: Architecture Decision

Because:

```text
High value

High scale

Mission critical
```

Choose:

```text
API

Queue

Worker

Persistence

Retry

DLQ
```

rather than:

```text
Direct synchronous notification
```

---

# Phase 8: Cost Mapping

Suppose:

```text
AWS Cost

$3000/month
```

Volume:

```text
60 million notifications/month
```

Cost:

```text
$3000 / 60M

= $0.00005 per notification
```

This becomes a business metric.

You can tell leadership:

```text
For $3000/month we support
60 million notifications.
```

Which sounds far better than:

```text
Need 6 EC2 instances.
```

---

# Phase 9: Plan for Growth

Architects should never size for today.

Create:

```text
Current
Year 1
Year 3
```

| Metric | Today | Year 1 | Year 3 |
|---|---|---|---|
| Customers | 100K | 500K | 2M |
| Notifications/day | 2M | 10M | 50M |
| Peak Events/sec | 500 | 2500 | 12500 |

Now ask:

```text
Will this architecture survive
12500 events/sec?
```

If not:

```text
Change architecture now.
```

---

# Phase 10: Architecture Review Questions

During grooming/review, use this checklist.

### Business

```text
What business capability are we enabling?
What revenue depends on it?
```

### Scale

```text
Peak users?
Peak TPS?
Growth forecast?
```

### Reliability

```text
Acceptable downtime?
Data loss allowed?
```

### Cost

```text
Cost per transaction?
Cost at 10x growth?
```

### Platform

```text
Is this a reusable capability?
Can another product consume it?
```

---

# What the Final Output Looks Like

A good architect should be able to produce a one-page document like:

```text
Capability:
Event Notification

Business Value:
High

Products:
Composer
Sign
CLM
CPQ

Current Scale:
500 events/sec

3-Year Scale:
12500 events/sec

Availability:
99.95%

RPO:
0

Architecture:
API
Queue
Worker
Storage
DLQ

Estimated Monthly Cost:
$3000

Cost per Notification:
$0.00005
```

Notice that **compute, storage, queues, database, cloud cost, and scaling decisions all came from business value and workload demand, not from technology choices**.

That is the core difference between a **senior engineer** and a **product/platform architect**. The architect starts with business outcomes, converts them into scale models, and only then chooses the technology.

---

## 5. User Request

> Summarize key steps to apply the framework

---

## Assistant Response

# Architect's Resource Mapping Framework — Executive Summary

Think in this sequence:

```text
1. Business Goal
       ↓
2. Business Capability
       ↓
3. Business Value
       ↓
4. Workload Drivers
       ↓
5. Scalability Targets
       ↓
6. Resource Model
       ↓
7. SLA / Reliability
       ↓
8. Cost Model
       ↓
9. Architecture Decision
```

---

## Step 1: Understand the Business Goal

Ask:

- What business problem are we solving?
- What revenue or customer outcome depends on it?
- Why is this important now?

Example:

```text
Customers need merge completion notifications.
```

---

## Step 2: Identify the Capability

Do not focus on the feature.

Ask:

```text
What capability does this feature reveal?
```

Example:

```text
Merge Notifications
      ↓
Event Notification Capability
```

---

## Step 3: Assess Business Value

Score based on:

- Revenue impact
- Customer impact
- Strategic importance
- Operational importance

Classify:

```text
High
Medium
Low
```

Business value determines how much investment is justified.

---

## Step 4: Convert Business Metrics into Technical Demand

Avoid:

```text
100,000 customers
```

Convert into:

```text
Concurrent users
TPS
Documents/day
Messages/day
Data growth
```

Example:

```text
2 Million notifications/day
500 peak events/sec
```

---

## Step 5: Define Scalability Targets

Determine:

```text
Today
1 Year
3 Years
```

Example:

| Metric | Today | 1 Year | 3 Year |
|---|---|---|---|
| Users | 100K | 500K | 2M |
| TPS | 500 | 2500 | 12500 |

Architectural decisions should support future growth, not just today's load.

---

## Step 6: Estimate Resources

### Compute

```text
Peak TPS
↓
CPU
↓
Pods
↓
Nodes
```

### Storage

```text
Records/day
×
Retention
=
Storage Required
```

### Messaging

```text
Events/sec
×
Burst Factor
=
Queue Capacity
```

---

## Step 7: Define SLA Requirements

Translate business expectations into measurable targets.

Example:

```text
Availability = 99.95%

RPO = 0

Notification Delivery < 30 sec
```

---

## Step 8: Calculate Cost

Do not measure:

```text
EC2 Cost
```

Measure:

```text
Cost per Notification

Cost per Merge

Cost per Contract

Cost per API Request
```

Business leaders understand this immediately.

---

## Step 9: Select Architecture Pattern

### Low Business Value

```text
Single Region
Basic Backup
```

### Medium Business Value

```text
Multi-AZ
Auto Scaling
Managed Services
```

### High Business Value

```text
Queue Based
HA
Multi Region
Zero Data Loss
```

---

# One-Page Architect Output

Every major feature/capability should end with:

```text
Capability:
Event Notification

Business Value:
High

Consumers:
Composer
Sign
CLM
CPQ

Current Scale:
500 TPS

Future Scale:
12500 TPS

Availability:
99.95%

RPO:
0

Architecture:
API
Queue
Worker
Storage
DLQ

Estimated Cost:
$X/month

Cost per Transaction:
$Y
```

# Golden Rule

A Senior Engineer asks:

```text
How do we build it?
```

An Architect asks:

```text
Why are we building it?
What capability does it reveal?
How much scale is required?
What business value does it generate?
What is the most cost-effective architecture to support it for the next 3–5 years?
```

This single mindset shift is what connects **product strategy -> scalability requirements -> compute/storage sizing -> architecture decisions**.

---

## 6. User Request

> Lets create .md file for this complete session request and response

---

## Assistant Response

Done, I created the Markdown file with the complete guidance from this session, including the framework, real project application, examples, checklist, and one-page architect template.

File created:

```text
Architect_Resource_Mapping_Framework.md
```

---

## 7. User Request

> Create another md file using all the request and response in this session from the first request and last response

---

## Assistant Action

Created this Markdown transcript file containing all requests and responses from the first request through the last response.

---

# Appendix: Distilled Framework

```text
Business Outcome
    -> Capability
    -> Business Value
    -> Workload Demand
    -> Scalability Target
    -> Resource Model
    -> SLA / Reliability
    -> Cost Model
    -> Architecture Decision
```

## Practical Architect Mindset

A strong product/platform architect does not begin with infrastructure. The architect begins with business impact, identifies the underlying capability, translates demand into measurable workload characteristics, estimates resources, validates cost, and selects an architecture that is economically justified and scalable for future growth.
