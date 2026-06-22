Since you're joining a new **Data Factory 5C** team focused on **Data Migration (On-Prem → GCP Cloud)**, your role as a DevOps Engineer will be broader than a normal CI/CD role. You will support infrastructure, migration pipelines, monitoring, automation, security, and release processes.

## 1. First Things to Understand from Your Lead

Before starting, collect these details:

### Project Overview

* What is the business goal of the migration?
* Which applications are involved?
* Is it a one-time migration or continuous replication?
* Expected Go-Live date?
* Number of databases being migrated?

### Source & Target Details

**Source**

* Oracle Database (On-Prem)
* Version?
* Size (GB/TB)?
* Number of schemas/tables?
* CDC enabled?

**Target**

* Google Cloud Spanner
* Spanner instance details
* Database naming standards
* Region and HA configuration

---

## 2. Tools You Need to Learn

### Striim

Main migration tool.

Learn:

* CDC (Change Data Capture)
* Oracle → Spanner pipeline
* TQL
* Pipeline creation
* Checkpoints
* Lag monitoring
* Error handling
* Recovery process
* Go-Live cutover process

Questions:

* Who manages Striim?
* Existing SOP?
* Existing TQL templates?
* Existing migration projects?

---

### GCP Cloud

Focus on:

#### IAM

* Service Accounts
* Roles
* Permissions

#### Networking

* VPC
* Firewall
* VPN
* Private Connectivity

#### Databases

* Cloud Spanner
* Cloud Storage

#### Monitoring

* Cloud Logging
* Cloud Monitoring

---

### Harness (CI/CD)

Learn:

* Pipelines
* Triggers
* Templates
* Secrets
* Service Accounts
* Deployment Process

Questions:

* Existing Harness project?
* Existing pipeline templates?
* Promotion process?

---

### Datadog Monitoring

Learn:

* Dashboards
* Monitors
* Alerts
* Log Management
* Infrastructure Monitoring
* Database Monitoring

Monitor:

* Striim Lag
* CPU
* Memory
* Network
* Oracle Connection
* Spanner Connection
* Pipeline Failures

---

## 3. Infrastructure Knowledge Required

Understand:

### Environment Structure

* DEV
* QA
* UAT
* PROD

Questions:

* How many GCP projects?
* Separate projects per environment?
* Naming conventions?

### Infrastructure Provisioning

Ask:

* Terraform used?
* Harness IaC?
* Manual provisioning?

If Terraform exists, collect:

* Modules
* State files
* Backend configuration

---

## 4. Data Migration Process Understanding

Typical flow:

```text
Oracle (On-Prem)
      ↓
   Striim CDC
      ↓
 GCP Network
      ↓
Cloud Spanner
      ↓
Validation
      ↓
Go-Live
```

Understand:

### Initial Load

* Full database migration

### CDC

* Real-time changes

### Cutover

* Stop source writes
* Validate counts
* Switch application

---

## 5. Monitoring & Operational Support

Know:

### Health Checks

* Striim running?
* Oracle reachable?
* Spanner reachable?
* CDC lag?
* Data loss?
* Pipeline status?

### Incident Handling

Prepare SOP for:

* Oracle connectivity failure
* CDC lag increase
* Striim crash
* Spanner write failure
* Network issue
* GCP outage

---

## 6. POC Process (Very Important)

Your lead mentioned:

> "Any team comes and asks for a POC."

Create a standard process:

### POC Intake Template

1. Source Database
2. Target Database
3. Data Size
4. Expected Downtime
5. Migration Window
6. CDC Required?
7. Security Requirements
8. Success Criteria

### POC Execution

```text
Requirement Collection
        ↓
Architecture Review
        ↓
Infra Provisioning
        ↓
Striim Pipeline Setup
        ↓
Testing
        ↓
Validation
        ↓
Sign-Off
```

### SOP Document

Include:

* Prerequisites
* Access Required
* Deployment Steps
* Rollback Steps
* Validation Steps
* Monitoring Steps
* Troubleshooting

---

## 7. Security Topics

Collect:

* VPN setup
* Database credentials management
* Secrets Manager usage
* IAM roles
* Service Accounts
* Audit logs
* Encryption requirements

---

## 8. Go-Live Checklist

Before production:

### Infrastructure

* Servers ready
* GCP resources ready
* Monitoring enabled

### Striim

* Pipeline tested
* CDC validated
* Checkpoint verified

### Data Validation

* Record counts match
* No missing tables
* No replication lag

### Monitoring

* Datadog alerts configured
* Escalation matrix ready

---

## 9. Questions to Ask Your Lead

1. Project architecture diagram available?
2. Existing SOP documents?
3. Existing Striim applications?
4. Existing Harness pipelines?
5. Existing Datadog dashboards?
6. Terraform repositories?
7. Access request process?
8. Migration runbook?
9. Incident management process?
10. Go-Live support expectations?
11. On-call support required?
12. Team responsibilities matrix?

---

## AI Prompt for Deep Learning

Use this prompt in ChatGPT, Claude, Gemini, or Perplexity:

```text
I am a DevOps Engineer joining a Data Migration project called Data Factory 5C.

Tech Stack:
- Source Database: Oracle (On-Prem)
- Target Database: Google Cloud Spanner
- Migration Tool: Striim
- Cloud Platform: GCP
- CI/CD: Harness
- Monitoring: Datadog
- Infrastructure: Terraform (if applicable)

Please act as a Senior Data Migration Architect and DevOps Lead.

Provide:

1. End-to-end architecture explanation.
2. Roles and responsibilities of a DevOps Engineer.
3. Data migration lifecycle from Oracle to Spanner.
4. Striim architecture, CDC flow, TQL examples, troubleshooting.
5. GCP services involved and access requirements.
6. Harness CI/CD setup for migration projects.
7. Datadog monitoring dashboards and alerts.
8. Infrastructure requirements and Terraform examples.
9. SOP documents for POC, deployment, rollback and Go-Live.
10. Common production issues and resolutions.
11. Daily operational activities.
12. Interview-style questions and answers for this project.

Explain everything from beginner to advanced level with real-world examples and diagrams.
```

This prompt will give you a complete roadmap for your new Data Factory 5C project.
