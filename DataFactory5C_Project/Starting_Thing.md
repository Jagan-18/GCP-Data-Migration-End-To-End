Don't worry. Since you're joining a **new Data Migration project (Data Factory 5C)**, nobody expects you to know everything on Day 1.

As a **DevOps Engineer**, your first goal is **not to learn everything at once**. Your goal is to understand the overall flow and then learn the tools one by one.

# Week 1: Understand the Big Picture

First understand this simple architecture:

```text
Oracle Database
      |
      | CDC
      v
   Striim
      |
      v
Cloud Spanner (GCP)
      |
      v
 Applications
```

Your project is moving data from Oracle to Cloud Spanner.

### Learn these basic questions:

1. What is Oracle?
2. What is Cloud Spanner?
3. What is Data Migration?
4. What is CDC (Change Data Capture)?
5. Why is Striim used?

If you understand these 5 topics, you'll already understand 50% of the project.

---

# Step 1: Learn CDC

This is the most important concept.

### Without CDC

```text
Oracle -----> Export File -----> Import
```

Manual process.

### With CDC

```text
Oracle
   |
   | Real-time changes
   v
Striim
   |
   v
Cloud Spanner
```

Example:

Customer table:

```sql
INSERT INTO CUSTOMER VALUES(101,'Reddy');
```

Immediately Striim captures it and sends it to Spanner.

This is CDC.

---

# Step 2: Learn Striim Basics

Your lead already mentioned Striim.

Focus on:

### Components

```text
Oracle Reader
Stream
CQ
Target Writer
```

Learn:

* What is Oracle Reader?
* What is Stream?
* What is CQ?
* What is Target Writer?

Don't start with TQL coding yet.

Just understand the flow.

---

# Step 3: Learn GCP Basics

As a DevOps Engineer, you'll need GCP access.

Start with:

### Must Know Services

* IAM
* VPC
* Cloud Spanner
* Cloud Storage
* Logging
* Monitoring
* Secret Manager

Understand:

```text
Who can access what?
How is networking configured?
Where are logs stored?
```

---

# Step 4: Learn Harness

Since CI/CD is Harness.

Learn:

### Harness Architecture

```text
Git
 |
 v
Harness
 |
 v
Deploy
```

Understand:

* Projects
* Pipelines
* Connectors
* Secrets
* Environments
* Delegates

Don't worry about advanced pipelines initially.

---

# Step 5: Learn Datadog

Focus on:

### Dashboard

```text
CPU
Memory
Network
Errors
```

### Alerts

```text
CPU > 80%
Memory > 85%
CDC Lag > 10 mins
```

As DevOps, monitoring is one of your daily activities.

---

# Step 6: Learn Terraform

You already know some DevOps tools, so Terraform will be easier.

Understand:

```text
provider.tf
main.tf
variables.tf
outputs.tf
terraform.tfstate
```

Then learn:

```hcl
resource "google_spanner_instance" ...
```

No need to become an expert immediately.

---

# What You Should Ask Your Lead

During KT sessions, ask:

### Project Questions

1. What is the source Oracle version?
2. What is the target Spanner environment?
3. How many migration pipelines exist?
4. Is Striim self-managed or cloud-managed?
5. Who owns Oracle access?
6. What monitoring dashboards already exist?
7. What deployments are done through Harness?
8. Is Terraform used for all infrastructure?

These questions show interest and understanding.

---

# Your Daily Work Will Likely Be

### Morning

* Check Striim pipelines
* Check CDC lag
* Check Datadog alerts
* Check GCP logs

### During Day

* Support deployment requests
* Create Harness pipelines
* Update Terraform
* Troubleshoot connectivity issues

### Evening

* Health check
* Status report
* Incident follow-up

---

# First 15-Day Learning Plan

### Days 1–3

Learn:

* Oracle basics
* Cloud Spanner basics
* CDC concept

### Days 4–6

Learn:

* Striim architecture
* Oracle Reader
* Streams
* CQ

### Days 7–9

Learn:

* GCP IAM
* VPC
* Cloud Spanner

### Days 10–12

Learn:

* Harness
* Pipeline execution
* Delegates

### Days 13–15

Learn:

* Datadog
* Terraform
* Monitoring

---

## Priority Order (Very Important)

Learn in this order:

```text
1. CDC Concept
2. Oracle → Spanner Flow
3. Striim Architecture
4. GCP Basics
5. Harness
6. Datadog
7. Terraform
```

Do **not** start with Terraform or Harness first.

For your project, **Striim + CDC + Oracle → Spanner flow** is the foundation. Once you understand that flow, all the DevOps activities will make much more sense.

Based on your previous DevOps experience with Jenkins, GitHub, Terraform, Helm, and CI/CD, you already have a good foundation. The biggest new area for you is **Data Migration and Striim**, so spend most of your learning time there first.

---
Since you're completely new to **Data Migration + Striim**, don't try to learn everything together.

# Your Learning Roadmap (Follow This Exactly)

## Phase 1 (Day 1-2) – Understand the Project Flow

First learn:

### 1. What is Data Migration?

Simple example:

```text
Old Database (Oracle)
        |
        v
New Database (Cloud Spanner)
```

Moving data from Oracle to Spanner is called Data Migration.

### 2. What is CDC (Change Data Capture)?

Example:

Customer inserted into Oracle:

```sql
INSERT INTO CUSTOMER VALUES (101,'Jagadeesh');
```

Striim captures the change and sends it to Spanner automatically.

This is CDC.

### Goal

Understand:

```text
Oracle --> Striim --> Cloud Spanner
```

Nothing else.

---

# Phase 2 (Day 3-5) – Learn Oracle Basics

You are not a DBA.

But you must know:

### Learn:

* What is a Database?
* What is a Table?
* Primary Key
* Insert
* Update
* Delete
* Schema

Practice:

```sql
SELECT * FROM CUSTOMER;

INSERT INTO CUSTOMER VALUES(1,'ABC');

UPDATE CUSTOMER
SET NAME='XYZ'
WHERE ID=1;
```

### Goal

Understand what Striim is reading from Oracle.

---

# Phase 3 (Day 6-8) – Learn Striim

This is your MOST IMPORTANT topic.

Learn architecture:

```text
Oracle Reader
      |
      v
    Stream
      |
      v
      CQ
      |
      v
Target Writer
      |
      v
Cloud Spanner
```

Understand:

### Oracle Reader

Reads Oracle changes.

### Stream

Temporary data flow.

### CQ

Continuous Query.

### Target Writer

Writes to Spanner.

### Goal

Be able to explain:

> "How data moves from Oracle to Cloud Spanner using Striim."

---

# Phase 4 (Day 9-11) – Learn GCP

Focus only on:

### IAM

Permissions.

### VPC

Networking.

### Cloud Spanner

Target database.

### Cloud Storage

Backup files.

### Logging

Application logs.

### Goal

Know where to check logs and access.

---

# Phase 5 (Day 12-14) – Learn Harness

Since you already know Jenkins concepts, this will be easy.

Learn:

```text
Connector
Delegate
Pipeline
Stage
Environment
Secrets
```

Understand:

```text
Git Repo
   |
Harness Pipeline
   |
Deploy Striim Config
```

---

# Phase 6 (Day 15-17) – Learn Datadog

Learn:

### Dashboards

* CPU
* Memory
* Network
* Errors

### Alerts

* Pipeline Down
* CDC Lag
* High CPU

---

# Phase 7 (Day 18-20) – Learn Terraform

Since you already have Terraform knowledge:

Focus on:

```text
Provider
Resource
Module
State File
Variables
```

GCP Resources:

* VPC
* Service Accounts
* Cloud Storage
* Cloud Spanner

---

# What Your Lead Will Mostly Ask You Initially

### Week 1

Can you:

* Access GCP?
* Access Striim?
* Access Harness?
* Access Datadog?

### Week 2

Can you:

* Check pipeline status?
* Check CDC lag?
* Check logs?
* Raise incidents?

### Week 3+

Can you:

* Deploy changes?
* Update configurations?
* Support migration cutovers?

---

# First 5 Questions to Ask Your Lead Tomorrow

1. Can you share the architecture diagram?
2. Can you show one running Striim pipeline?
3. Where do we monitor CDC lag?
4. Which deployments are done through Harness?
5. Do we have Terraform repositories for infrastructure?

---

# If I Were You

I would start with only these 3 topics this week:

### Day 1

* What is Data Migration?
* What is CDC?

### Day 2

* Oracle Basics

### Day 3

* Striim Architecture

After that, everything else (GCP, Harness, Datadog, Terraform) will become much easier because you'll understand the actual business flow.

**Remember:** As a DevOps Engineer in this project, the most valuable knowledge is:

```text
Oracle --> CDC --> Striim --> Cloud Spanner
```

Master this flow first. Everything else supports that flow.
