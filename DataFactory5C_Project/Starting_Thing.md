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
