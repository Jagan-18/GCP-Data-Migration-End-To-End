# Data Factory 5C – Complete DevOps & Data Migration Guide

**Source:** Oracle (On-Prem)
**Target:** Google Cloud Spanner
**Migration Tool:** Striim (CDC)
**Cloud:** GCP
**CI/CD:** Harness
**Monitoring:** Datadog
**Infrastructure:** Terraform

---

# 1. End-to-End Architecture

## High-Level Architecture

```text
+--------------------+
| Oracle Database    |
| (On-Premise)       |
+---------+----------+
          |
          | CDC (Redo Logs)
          |
          v
+--------------------+
| Striim Platform    |
| - Readers          |
| - CDC Engine       |
| - TQL Pipelines    |
+---------+----------+
          |
          |
          v
+----------------------+
| Google Cloud Spanner |
+----------+-----------+
           |
           |
           +-------------------+
           |                   |
           v                   v

+---------------+     +----------------+
| Datadog       |     | GCP Monitoring |
| Monitoring    |     | Logging        |
+---------------+     +----------------+

            ^
            |
     Harness CI/CD
            |
     Terraform Infra
```

---

# 2. DevOps Engineer Responsibilities

In Data Migration projects, DevOps does much more than CI/CD.

## Phase 1 – Infrastructure Setup

You will:

### Provision

* GCP Projects
* VPC
* Subnets
* Firewall Rules
* Service Accounts
* Secrets
* Cloud Storage
* Cloud Spanner

Tools:

* Terraform
* GCP Console
* Harness

---

## Phase 2 – Striim Environment Setup

You will:

* Install Striim
* Configure HA
* Configure Oracle Readers
* Configure Spanner Writers
* Manage Secrets

---

## Phase 3 – CI/CD

Using Harness:

Deploy:

* Striim Applications
* TQL Scripts
* Terraform Infrastructure
* Configuration Changes

---

## Phase 4 – Monitoring

Monitor:

* CDC Lag
* Pipeline Failures
* Database Connectivity
* Replication Delay
* GCP Resource Usage

Using:

* Datadog
* GCP Monitoring

---

# 3. Data Migration Lifecycle

## Step 1 – Discovery

Source Team provides:

```text
Oracle Database
Tables
Views
Stored Procedures
Data Volume
LOB Data
PK Information
```

Example:

```sql
CUSTOMER
ORDER
PAYMENT
```

---

## Step 2 – Assessment

Questions:

* How many TB?
* Downtime allowed?
* Full migration or CDC?
* Historical data?

---

## Step 3 – Initial Load

```text
Oracle
   |
   v
Striim Snapshot
   |
   v
Cloud Spanner
```

Load existing data.

---

## Step 4 – CDC Replication

Capture:

```text
INSERT
UPDATE
DELETE
```

in real time.

---

## Step 5 – Validation

Compare:

```text
Source Count = Target Count
```

Example:

```sql
SELECT COUNT(*) FROM CUSTOMER;
```

Both sides should match.

---

## Step 6 – UAT

Business validates:

* Reports
* Dashboards
* Applications

---

## Step 7 – Go Live

Cutover:

```text
Stop Writes
Final Sync
Switch Applications
```

---

# 4. Striim Architecture

## Components

```text
Oracle Reader
      |
      v
Stream
      |
      v
CQ(Query)
      |
      v
Target Writer
      |
      v
Spanner
```

---

## CDC Flow

```text
Oracle Redo Logs
        |
        v
Striim Reader
        |
        v
Memory Stream
        |
        v
Transformation
        |
        v
Spanner Writer
```

---

# Basic TQL Example

## Create Source

```sql
CREATE SOURCE OracleSource
USING OracleReader (
 Username:'admin',
 Password:'password',
 ConnectionURL:'oraclehost:1521/orcl'
);
```

---

## Stream

```sql
CREATE STREAM OracleStream;
```

---

## CQ

```sql
CREATE CQ FilterOrders
INSERT INTO OrderStream
SELECT * FROM OracleStream
WHERE TableName='ORDERS';
```

---

## Target

```sql
CREATE TARGET SpannerTarget
USING DatabaseWriter (
 ConnectionURL:'spanner'
);
```

---

# Troubleshooting Striim

## Issue 1

CDC Stopped

Reason:

```text
Archive Logs Missing
```

Solution:

```text
Enable ARCHIVELOG mode
```

---

## Issue 2

Pipeline Lag

Reason:

```text
Heavy Transaction Load
```

Solution:

```text
Increase Parallelism
```

---

## Issue 3

OOM Error

Reason:

```text
Large Cache
```

Solution:

```text
Increase JVM Heap
```

---

## Issue 4

Target Writes Slow

Reason:

```text
Spanner Throttling
```

Solution:

```text
Increase Processing Units
```

---

# 5. GCP Services Involved

## Cloud Spanner

Target Database

---

## Cloud Storage

Snapshots

---

## Secret Manager

Passwords

---

## IAM

Access Control

---

## VPC

Network Connectivity

---

## Cloud Logging

Logs

---

## Cloud Monitoring

Metrics

---

## Cloud KMS

Encryption

---

# Access Required

## DevOps Team

```text
Project Viewer
Storage Admin
Spanner Admin
Monitoring Viewer
Secret Manager Admin
```

---

## Striim Team

```text
Spanner User
Storage User
Network User
```

---

# 6. Harness CI/CD Setup

## Pipeline Flow

```text
GitHub
   |
   v
Harness Trigger
   |
   v
Terraform Apply
   |
   v
Deploy Striim
   |
   v
Validation
   |
   v
Approval
   |
   v
Production
```

---

## Stages

### DEV

```text
Deploy
Validate
```

---

### UAT

```text
Deploy
Smoke Test
```

---

### PROD

```text
Approval
Deploy
Post Validation
```

---

# 7. Datadog Monitoring

## Dashboard 1

Migration Dashboard

Metrics:

```text
CDC Lag
Transactions/sec
Events/sec
Failed Events
```

---

## Dashboard 2

Infrastructure Dashboard

```text
CPU
Memory
Disk
Network
```

---

## Dashboard 3

Spanner Dashboard

```text
Read Latency
Write Latency
CPU Utilization
Storage
```

---

# Alerts

## Critical

CDC Lag

```text
> 10 Minutes
```

---

## Critical

Pipeline Down

```text
No Events For 5 Minutes
```

---

## Warning

CPU

```text
>80%
```

---

## Warning

Memory

```text
>85%
```

---

# 8. Terraform Example

## Spanner Instance

```hcl
resource "google_spanner_instance" "spanner" {

  name         = "prod-spanner"

  config       = "regional-us-central1"

  display_name = "prod-spanner"

  processing_units = 1000
}
```

---

## Database

```hcl
resource "google_spanner_database" "db" {

  instance = google_spanner_instance.spanner.name

  name     = "customer-db"
}
```

---

# Recommended Terraform Structure

```text
terraform/

├── modules
│   ├── network
│   ├── spanner
│   ├── storage
│   ├── monitoring
│
├── dev
├── uat
├── prod
│
├── backend.tf
├── provider.tf
├── variables.tf
└── outputs.tf
```

---

# 9. SOP Documents

# A. POC SOP

### Inputs

```text
Source DB Details
Target Details
Tables
Volume
```

### Steps

```text
Create POC Environment
Configure Striim
Run Snapshot
Enable CDC
Validate Data
Prepare Report
```

---

# B. Deployment SOP

```text
1. Backup Configurations
2. Deploy TQL
3. Start Pipelines
4. Validate Connectivity
5. Monitor CDC
6. Sign Off
```

---

# C. Rollback SOP

```text
Stop CDC
Disable Target Writes
Restore Previous Config
Restart Old Pipeline
Validate
```

---

# D. Go-Live SOP

### T-7 Days

```text
Performance Testing
```

### T-3 Days

```text
Freeze Changes
```

### T-1 Day

```text
Full Validation
```

### Go Live Day

```text
Stop Writes
Final Sync
Cutover
Business Validation
Sign Off
```

---

# 10. Common Production Issues

| Issue              | Reason             | Fix                |
| ------------------ | ------------------ | ------------------ |
| CDC Lag            | Heavy Transactions | Increase Resources |
| Pipeline Failure   | DB Connection Lost | Restart Source     |
| Oracle Reader Down | Password Expired   | Update Secret      |
| Spanner Timeout    | High Writes        | Scale Spanner      |
| Data Mismatch      | Missed CDC Event   | Reprocess Data     |
| High Memory        | Large Cache        | Tune JVM           |
| Duplicate Records  | CDC Restart        | Check Checkpoints  |

---

# 11. Daily Operational Activities

## Morning Checklist

### Check Striim

```text
Applications Running?
CDC Active?
```

---

### Check Datadog

```text
Errors
Lag
CPU
Memory
```

---

### Check GCP

```text
Spanner Health
Storage
Logs
```

---

### Check Harness

```text
Failed Deployments
Pending Approvals
```

---

### Daily Report

```text
Migration Status
Pipeline Health
Issues
Risks
```

---

# 12. Interview Questions & Answers

## Q1. What is CDC?

Answer:

CDC (Change Data Capture) captures database changes such as Insert, Update, Delete from Oracle redo logs and replicates them to the target system without impacting source applications.

---

## Q2. Why use Striim?

Answer:

* Real-time replication
* Low latency
* High throughput
* Built-in CDC
* Supports Oracle → Spanner migration

---

## Q3. What is the difference between Initial Load and CDC?

### Initial Load

```text
Historical Data
```

### CDC

```text
Ongoing Changes
```

---

## Q4. How does Striim capture Oracle changes?

Answer:

Striim reads Oracle Redo Logs and Archive Logs using OracleReader and converts changes into streams.

---

## Q5. What would you do if CDC lag increases?

Answer:

1. Check Oracle transaction volume.
2. Check Striim processing rate.
3. Check network latency.
4. Check Spanner write latency.
5. Scale Striim or Spanner.

---

## Q6. What is your role as a DevOps Engineer?

Answer:

* Provision infrastructure using Terraform.
* Deploy Striim applications via Harness.
* Manage CI/CD pipelines.
* Configure monitoring using Datadog.
* Support Go-Live and production operations.
* Troubleshoot CDC, cloud, network, and deployment issues.

---

# What Your Lead Will Expect From You in Data Factory 5C

Within the first 30 days, you should become comfortable with:

✅ Oracle CDC concepts
✅ Striim Architecture & TQL
✅ GCP (Spanner, IAM, VPC, Logging, Monitoring)
✅ Harness Pipelines & Approvals
✅ Datadog Dashboards & Alerts
✅ Terraform Infrastructure
✅ Migration Runbooks & SOPs
✅ Go-Live Activities
✅ Production Support & Troubleshooting

If you master these areas, you will be able to independently support Oracle → Cloud Spanner migration projects as a DevOps Engineer in Data Factory 5C.
