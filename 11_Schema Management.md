# Schema Management in Striim:

## What is Schema Management?

**Schema Management** is the process of handling the structure of data (tables, columns, data types) between source and target systems.

It ensures that data captured from the source is correctly mapped and written to the target.

Example:

### Source Table:
```text
EMPLOYEE
--------
EMP_ID      NUMBER
EMP_NAME    VARCHAR2
SALARY      NUMBER
```

### Target Table:
```text
EMPLOYEE
--------
EMP_ID      INTEGER
EMP_NAME    STRING
SALARY      FLOAT
```

Striim maps source schema to target schema during data processing.

---
# Why Schema Management is Important:
Without schema management:

```text
Source Table Changed
        |
        v
Pipeline Failure
```

With proper schema management:

```text
Source Change
      |
Schema Mapping
      |
Target Updated
```

Benefits:

✅ Prevents pipeline failures

✅ Handles schema evolution

✅ Supports data type conversions

✅ Maintains data consistency

---

# Schema Flow in Striim

```text
Oracle Table
      |
OracleReader
      |
Stream Schema
      |
CQ Transformation
      |
Target Schema
      |
BigQuery/Snowflake
```

---

# Schema Discovery

Striim automatically discovers source table schemas.

Example:

```text
EMPLOYEE
--------
EMP_ID
EMP_NAME
SALARY
DEPARTMENT
```

OracleReader reads metadata and creates corresponding stream structures.

---

# Stream Schema

Example:

```sql
CREATE STREAM EmployeeStream (
   EMP_ID Integer,
   EMP_NAME String,
   SALARY Double,
   DEPARTMENT String
);
```

The stream schema acts as the data contract inside the pipeline.

---

# Data Type Mapping

### Oracle → BigQuery

| Oracle    | BigQuery      |
| --------- | ------------- |
| NUMBER    | INTEGER/FLOAT |
| VARCHAR2  | STRING        |
| DATE      | DATE          |
| TIMESTAMP | TIMESTAMP     |
| CLOB      | STRING        |

---

### Oracle → Kafka

```text
Oracle
   |
Striim
   |
JSON
   |
Kafka
```

Data is often serialized as JSON.

---

# Schema Mapping

Example:

### Source

```text
EMP_ID
EMP_NAME
```

### Target

```text
ID
NAME
```

Using TQL:

```sql
CREATE CQ EmployeeMapping
INSERT INTO EmployeeTargetStream
SELECT
    EMP_ID AS ID,
    EMP_NAME AS NAME
FROM EmployeeStream;
```

---

# Adding New Columns (Schema Evolution)

### Original Table

```text
EMP_ID
EMP_NAME
SALARY
```

Later:

```text
EMP_ID
EMP_NAME
SALARY
EMAIL
```

This is called **Schema Evolution**.

---

# Handling Schema Evolution

Options:

### Option 1: Auto Schema Evolution

Striim detects:

```text
New Column Added
```

and updates schema automatically (depending on source/target support).

---

### Option 2: Manual Update

Update stream definition:

```sql
CREATE STREAM EmployeeStream (
   EMP_ID Integer,
   EMP_NAME String,
   SALARY Double,
   EMAIL String
);
```

---

# Dropping Columns

Example:

Old:

```text
EMP_ID
EMP_NAME
EMAIL
```

New:

```text
EMP_ID
EMP_NAME
```

Update mappings and targets accordingly.

---

# Data Type Conversion

Example:

Convert String to Integer.

```sql
CREATE CQ ConvertData
INSERT INTO NewStream
SELECT
   TO_INT(EMP_ID) AS EMP_ID
FROM EmployeeStream;
```

---

# Null Handling

Example:

```sql
CREATE CQ NullHandling
INSERT INTO NewStream
SELECT
   NVL(EMP_NAME,'UNKNOWN') AS EMP_NAME
FROM EmployeeStream;
```

Output:

```text
NULL
```

becomes:

```text
UNKNOWN
```

---

# Schema Validation

Before deployment:

Validate:

```text
Source Columns

Target Columns

Data Types

Mappings
```

Example:

```text
EMP_ID Integer
      |
      v
ID Integer
```

Valid

---

# Common Schema Issues

## Column Missing

Error:

```text
EMAIL column not found
```

Cause:

```text
Target expects EMAIL
Source does not provide it
```

---

## Data Type Mismatch

Error:

```text
STRING cannot be converted to INTEGER
```

Example:

```text
ABC123
```

into:

```text
INTEGER
```

---

## New Column Added

Source:

```text
EMAIL
```

Target:

```text
EMAIL not present
```

May require schema update.

---

# Schema Management with Kafka

### Without Schema Registry

```text
Kafka Topic
     |
JSON Payload
```

Risk:

* Producers and consumers may use different schemas.

---

### With Schema Registry

```text
Producer
    |
Schema Registry
    |
Kafka
    |
Consumer
```

Benefits:

* Schema validation
* Versioning
* Compatibility checks

---

# GCP Example

```text
Cloud SQL
    |
MySQLReader
    |
CustomerStream
    |
BigQueryWriter
    |
BigQuery Table
```

When a new column is added:

```text
CUSTOMER_EMAIL
```

Verify:

* Stream schema
* BigQuery schema
* Mapping logic

---

# Best Practices

### Use Explicit Mappings

Good:

```sql
SELECT
EMP_ID AS ID,
EMP_NAME AS NAME
```

Avoid:

```sql
SELECT *
```

in production pipelines.

---

### Validate Schema Changes

Before deployment:

✔ New columns

✔ Dropped columns

✔ Data types

✔ Target compatibility

---

### Version-Control Schema Changes

Store:

```text
TQL Scripts
Schema Definitions
Mapping Files
```

in Git.

---

### Test Schema Evolution

Always test:

```text
Add Column

Drop Column

Change Data Type
```

in lower environments first.

---
### What is Schema Management?
Schema Management is the process of managing source and target data structures, mappings, data types, and schema changes to ensure successful data movement through Striim pipelines.

---
### What is Schema Evolution?
Schema Evolution refers to changes in the source schema, such as adding, removing, or modifying columns, while maintaining pipeline functionality.

---
### How does Striim handle schema changes?
Striim can detect schema changes, support schema evolution, and allow manual updates to stream definitions, mappings, and target schemas depending on the source and target systems.


>Schema Management in Striim involves discovering source schemas, mapping them to stream and target schemas, handling data type conversions, validating compatibility, and managing schema evolution such as adding or removing columns. Proper schema management ensures reliable CDC processing and prevents pipeline failures caused by schema mismatches.

---