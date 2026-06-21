# Pipeline Versioning & TQL Scripts in Striim:
Pipeline Versioning is the practice of storing and managing **TQL scripts** in a version control system such as GitHub, GitLab, or Bitbucket.

This helps teams:

* Track changes
* Collaborate safely
* Roll back to previous versions
* Maintain audit history
* Support CI/CD deployments

---
# Why Version Control TQL?
Without Git:

```text
Developer-1 changes TQL
       |
Developer-2 changes TQL
       |
Previous version lost
```

With Git:

```text
TQL Script
     |
Git Repository
     |
Version History
     |
Rollback Possible
```

**Benefits:**
✅ Change tracking

✅ Collaboration

✅ Rollback capability

✅ Audit compliance

✅ CI/CD integration

---
# Recommended Repository Structure:
```text
striim-pipelines/
│
├── apps/
│   ├── oracle-bigquery/
│   │   ├── source.tql
│   │   ├── cq.tql
│   │   ├── target.tql
│   │   └── deploy.tql
│   │
│   └── mysql-kafka/
│
├── environments/
│   ├── dev.properties
│   ├── qa.properties
│   └── prod.properties
│
└── README.md
```

---
# Example TQL Files:
### source.tql

```sql
CREATE SOURCE OracleSource
USING OracleReader (
 ConnectionURL:'jdbc:oracle:thin:@host:1521:ORCL',
 Username:'user',
 Password:'password'
)
OUTPUT TO EmployeeStream;
```

---
### cq.tql;
```sql
CREATE CQ EmployeeCQ
INSERT INTO FilteredStream
SELECT *
FROM EmployeeStream
WHERE SALARY > 50000;
```

---
### target.tql;
```sql
CREATE TARGET BigQueryTarget
USING BigQueryWriter (
 ProjectID:'gcp-project',
 Dataset:'employee'
)
INPUT FROM FilteredStream;
```

---
# Git Workflow;
## Clone Repository

```bash
git clone https://github.com/company/striim-pipelines.git
```

---

## Create Feature Branch

```bash
git checkout -b feature/add-new-cq
```

---
## Modify TQL:
Example:

```sql
WHERE SALARY > 60000
```

instead of:

```sql
WHERE SALARY > 50000
```

---
## Commit Changes:
```bash
git add .

git commit -m "Updated salary filter threshold"
```

---
## Push Branch

```bash
git push origin feature/add-new-cq
```

---
## Create Pull Request:
```text
feature/add-new-cq
        |
        v
Develop Branch
```

Reviewers verify:

* TQL syntax
* Performance impact
* Business logic

---
# Branch Strategy Example:
Since you're familiar with DevOps and GitHub branch protection:

```text
main
 |
release
 |
stage
 |
develop
 |
feature/*
```

### develop:
Used for:

```text
New CQ
New Source
New Target
```

---
### stage:
Used for:

```text
Testing
Validation
```

---
### release:
Used for:

```text
Pre-production deployment
```

---
### main:
Used for:

```text
Production TQL scripts
```

---
# Versioning Example:
### Version 1

```sql
WHERE SALARY > 50000
```

---
### Version 2:
```sql
WHERE SALARY > 60000
```

Git history:

```bash
git log
```

Output:

```text
v1.0 Initial CQ

v1.1 Updated salary filter

v1.2 Added aggregation
```

---
# Environment-Specific Configuration:
Avoid hardcoding values.

Bad:

```sql
ProjectID:'prod-project'
```

Good:

```sql
ProjectID:'${PROJECT_ID}'
```

Dev:

```text
PROJECT_ID=dev-project
```

Prod:

```text
PROJECT_ID=prod-project
```

---
# CI/CD for Striim Pipelines:
Typical DevOps Flow:

```text
Developer
    |
Git Push
    |
GitHub
    |
Jenkins/Harness
    |
Validate TQL
    |
Deploy to Striim
```

Example:

```text
GitHub
   |
Jenkins
   |
Striim API
   |
Deploy Application
```

---
# Backup Strategy:
Store:

```text
TQL Scripts
Application Definitions
Environment Files
Custom UDF JARs
```

inside Git repositories.

---
# Best Practices:
### One Application Per Folder

```text
oracle-bigquery/
mysql-kafka/
oracle-snowflake/
```

---
### Use Meaningful Commit Messages:
Good:

```text
Added Oracle CDC source

Added Customer aggregation CQ

Updated Kafka target configuration
```

Bad:

```text
changes

fix
```

---
### Use Pull Requests:
Before merging:

✔ Code Review

✔ TQL Validation

✔ Performance Review

---
### Tag Releases:
```bash
git tag v1.0

git tag v2.0
```

Useful for production rollbacks.

---

### Separate Environments

```text
dev
qa
stage
prod
```

Never deploy development scripts directly to production.

---
### Why store TQL scripts in Git?
To:
* Track changes
* Enable collaboration
* Support rollback
* Maintain version history
* Integrate with CI/CD

---
### How do you version-control Striim pipelines?
Store all TQL scripts, application definitions, configuration files, and UDF JARs in Git repositories using feature branches, pull requests, release tags, and environment-specific configurations.

---
### What are the benefits of Git-based pipeline management?
* Auditability
* Change tracking
* Team collaboration
* Rollback support
* Automated deployments

---
> Pipeline versioning in Striim is typically achieved by storing TQL scripts, configuration files, and UDF artifacts in Git repositories. Teams use branching strategies, pull requests, tags, and CI/CD pipelines to manage changes, review updates, track history, and automate deployments across development, testing, and production environments.**
