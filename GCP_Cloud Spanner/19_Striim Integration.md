# Topic 19: Striim Integration:

# 1. What is Striim?

**Striim** is a real-time data integration and Change Data Capture (CDC) platform.

It continuously captures changes from Oracle and sends them to Cloud Spanner.

Instead of copying the whole database repeatedly,

Striim sends only:

* INSERT
* UPDATE
* DELETE

operations.

---
# 2. Why Do We Use Striim?

Suppose Oracle contains

```text
500 Million Records
```

A new customer registers.

Without Striim

```text
Copy 500 Million Records Again
```

Impossible.

With Striim

```text
New Customer

↓

Capture Change

↓

Cloud Spanner Updated
```

Only one new record is replicated.

---
# 3. Where Does Striim Fit?

```text
                 Oracle
                    │
                    │
          (Database Changes)
                    │
                    ▼
                 Striim
         Reads Oracle Changes
                    │
         Processing / Mapping
                    │
                    ▼
            Cloud Spanner
                    │
                    ▼
              Applications
```

Your responsibility is making sure this pipeline stays healthy.

---
# 4. DevOps Responsibilities Before Integration:

Before the application starts,

you prepare the environment.

---
## Step 1

Cloud Spanner

Create:

* Instance
* Database

Verify

```text
Instance

↓

Database

↓

Ready
```

---
## Step 2

IAM

Create

* Service Account

Grant

* Cloud Spanner Database User
* Required permissions for Striim

---
## Step 3

Networking

Verify

```text
Oracle

↓

Striim

↓

Cloud Spanner
```

Questions

* Can Striim reach Oracle?
* Can Striim reach Cloud Spanner?
* Firewalls OK?
* VPN OK?
* DNS OK?

---
## Step 4

Secrets

Never store

```text
Oracle Password

Cloud Password
```

inside

* Git
* Terraform
* Jenkinsfile
* Harness

Use a secure secrets manager.

---
# 5. Striim Pipeline

A simplified production pipeline looks like:

```text
Oracle
   │
   ▼
Redo Logs
   │
   ▼
Oracle Reader
   │
   ▼
Striim Processing
   │
   ▼
Cloud Spanner Writer
   │
   ▼
Cloud Spanner
```

You don't usually develop this logic, but you must know each stage for troubleshooting.

---
# 6. What DevOps Monitors:

### Pipeline Status

Always check

```text
Running

Stopped

Paused

Failed
```

---
### Oracle Connection

Check

```text
Connected

Disconnected
```

---
### Cloud Spanner Connection

Verify

```text
Healthy

Connected
```

---
### Replication

Check

```text
Events

↓

Moving

↓

Cloud Spanner
```

---
### Lag

One of the most important metrics.

Example

```text
Oracle

10:00

↓

Cloud Spanner

10:00
```

Healthy.

If

```text
Oracle

10:00

↓

Cloud Spanner

10:15
```

Lag is 15 minutes.

Investigate immediately.

---
# 7. Daily Health Check:

Every morning

```text
Login

↓

Open Datadog

↓

Open Striim Dashboard

↓

Open GCP Console

↓

Check Alerts

↓

Check Lag

↓

Check Errors

↓

Healthy?
```

---
# 8. Common Production Alerts:

## Alert 1

Pipeline Stopped

Possible reasons

* Oracle unavailable
* Password expired
* Service account issue
* Network issue

Action

* Check logs.
* Verify connectivity.
* Restart only after identifying the cause.

---
## Alert 2

CDC Lag Increasing

Possible causes

* High Oracle activity
* Slow Cloud Spanner writes
* Network latency
* Striim bottleneck

Action

* Check throughput.
* Check Cloud Spanner CPU.
* Review logs.

---
## Alert 3

Cloud Spanner Rejecting Writes

Possible reasons

* IAM permission removed
* Schema mismatch
* Database unavailable

Action

Verify:

* IAM
* Database status
* Schema
* Logs

---
# 9. Deployment Flow:

Typical deployment

```text
Developer

↓

Git

↓

Harness

↓

Deploy Schema

↓

Cloud Spanner

↓

Verify Striim

↓

Start Validation
```

Your tasks

* Confirm deployment succeeded.
* Confirm schema matches expectations.
* Ensure Striim continues replicating.
* Validate sample records.

---
# 10. Monitoring Tools:

### Google Cloud Monitoring

Monitor

* CPU
* Storage
* Latency
* Errors

---

### Datadog

Monitor

* Dashboards
* Alerts
* Health
* Incident timelines

---

### Striim Dashboard

Monitor

* Pipeline state
* Lag
* Events/sec
* Errors
* Connections

---

### Cloud Logging

Review

* Authentication failures
* Connection failures
* Write errors
* Exceptions

---
# 11. Real Production Scenario:

Suppose Datadog sends

```text
ALERT

CDC Lag = 25 Minutes
```

Your troubleshooting steps

```
Step 1

Is Striim Running?

↓

YES

↓

Step 2

Oracle Connection OK?

↓

YES

↓

Step 3

Cloud Spanner Reachable?

↓

YES

↓

Step 4

Cloud Spanner CPU High?

↓

YES (95%)

↓

Step 5

Check Slow Queries

↓

Check Write Latency

↓

If needed, involve the Database Team

↓

Continue Monitoring
```

---
# 13. DevOps Runbook

If Striim stops unexpectedly:

1. Acknowledge the alert.
2. Check the Striim dashboard (pipeline state, errors, throughput).
3. Verify Oracle connectivity.
4. Verify Cloud Spanner connectivity.
5. Check service account permissions and credentials.
6. Review Striim logs and Cloud Logging.
7. Restart the pipeline only after the root cause is identified.
8. Confirm CDC resumes from the correct checkpoint (no unexpected data loss or duplication).
9. Validate recent records in Cloud Spanner.
10. Continue monitoring until lag returns to normal.

---
### Q1. What is Striim?

**Answer:**

Striim is a real-time data integration and Change Data Capture (CDC) platform that captures changes from source databases like Oracle and continuously replicates them to target systems such as Cloud Spanner.

---

### Q2. What is your role as a DevOps Engineer in a Striim project?

**Answer:**

My responsibilities include:

* Provisioning Cloud Spanner infrastructure.
* Configuring IAM and service accounts.
* Verifying network connectivity.
* Monitoring Striim pipelines.
* Tracking CDC lag and replication health.
* Managing alerts through Datadog and Google Cloud Monitoring.
* Supporting deployments and production troubleshooting.

---

### Q3. What would you check if CDC lag suddenly increases?

**Answer:**

I would:

1. Check the Striim pipeline status.
2. Verify Oracle connectivity.
3. Verify Cloud Spanner connectivity.
4. Check Cloud Spanner CPU, write latency, and transaction metrics.
5. Review Striim logs and Cloud Logging.
6. Confirm whether there were recent deployments or infrastructure changes.

---

### Q4. Which monitoring tools do you use?

**Answer:**

* **Datadog** for dashboards and alerts.
* **Google Cloud Monitoring** for Cloud Spanner metrics.
* **Cloud Logging** for troubleshooting.
* **Striim Dashboard** for pipeline health, lag, and throughput.

---
# Final Advice for Your Project:

Based on everything you've shared, **your daily work will likely revolve around these five areas**:

1. **Cloud Spanner** – Ensure the database is healthy and performant.
2. **Striim** – Keep CDC pipelines running with minimal lag.
3. **Datadog** – Respond to alerts and monitor dashboards.
4. **Harness** – Support schema and application deployments.
5. **Production Support** – Troubleshoot incidents, validate data flow, and coordinate with developers and DBAs.

If you become confident in these five areas, you'll be well prepared for the day-to-day responsibilities of a DevOps engineer on your Oracle → Striim → Cloud Spanner migration project.
