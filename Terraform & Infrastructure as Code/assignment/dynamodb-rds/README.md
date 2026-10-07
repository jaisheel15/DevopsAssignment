# DynamoDB

Amazon DynamoDB is AWS's fully managed **NoSQL database service** designed for high performance, scalability, and low-latency access.

Unlike traditional relational databases, DynamoDB stores data in a flexible schema format and can scale automatically to handle millions of requests per second.

### Key Features

- Fully managed
- Serverless
- Automatic scaling
- Single-digit millisecond latency
- Highly available
- Built-in backup and recovery

---

# NoSQL

DynamoDB is a **NoSQL database**.

Unlike relational databases:

| Relational Database | DynamoDB |
|---------------------|----------|
| Tables + Rows | Tables + Items |
| Fixed Schema | Flexible Schema |
| SQL Queries | API-based Queries |
| Joins Supported | No Joins |
| Vertical Scaling | Horizontal Scaling |

### Example

Relational:

```text
Users Table
├── id
├── name
└── email
```

DynamoDB:

```json
{
  "id": "1",
  "name": "John",
  "email": "john@example.com"
}
```

---

# Tables

A Table is the top-level container that stores data in DynamoDB.

### Example

```text
Users
Orders
Products
Sessions
```

### Structure

```text
Table
   │
   ▼
 Items
```

A table can contain millions or even billions of items.

---

# Items

An Item is a single record in a DynamoDB table.

Equivalent to a row in a relational database.

### Example

```json
{
  "user_id": "123",
  "name": "John Doe",
  "email": "john@example.com"
}
```

### Structure

```text
Table
   │
   ▼
 Item
```

---

# Attributes

Attributes are the individual data fields within an item.

Equivalent to columns in relational databases.

### Example

```json
{
  "user_id": "123",
  "name": "John Doe",
  "age": 25,
  "email": "john@example.com"
}
```

Attributes:

```text
user_id
name
age
email
```

### Supported Types

- String
- Number
- Boolean
- Binary
- List
- Map
- Set

---

# Partition Key

The Partition Key uniquely identifies where data is stored.

DynamoDB uses the partition key to distribute data across storage partitions.

### Example

```text
Users Table

Partition Key:
user_id
```

Item:

```json
{
  "user_id": "123",
  "name": "John"
}
```

### Flow

```text
Partition Key
      │
      ▼
Hash Function
      │
      ▼
Storage Partition
```

### Benefits

- Fast lookups
- Automatic scaling
- Even data distribution

---

# Sort Key

A Sort Key allows multiple items to share the same partition key while remaining uniquely identifiable.

### Composite Primary Key

```text
Partition Key + Sort Key
```

### Example

Orders Table

```text
Partition Key: user_id
Sort Key: order_id
```

Items:

```json
{
  "user_id": "123",
  "order_id": "1001"
}
```

```json
{
  "user_id": "123",
  "order_id": "1002"
}
```

### Structure

```text
user_id
   │
   ├── order_1001
   ├── order_1002
   └── order_1003
```

### Benefits

- Range queries
- Sorted retrieval
- One-to-many relationships

---

# DynamoDB Primary Keys

## Simple Primary Key

```text
Partition Key
```

Example:

```text
user_id
```

---

## Composite Primary Key

```text
Partition Key
      +
Sort Key
```

Example:

```text
user_id + order_id
```

---

# DynamoDB Example

```text
Users Table
│
├── user_1
├── user_2
└── user_3
```

Orders Table:

```text
user_1
│
├── order_1
├── order_2
└── order_3
```

---

# DynamoDB Use Cases

## 1. User Profiles

```text
Application
     │
     ▼
 DynamoDB
     │
     ▼
User Data
```

---

## 2. Shopping Cart

```text
User
  │
  ▼
Cart Data
  │
  ▼
DynamoDB
```

---

## 3. Gaming Leaderboards

```text
Players
   │
   ▼
Scores
   │
   ▼
DynamoDB
```

---

## 4. Session Storage

```text
Application
     │
     ▼
 Session Data
     │
     ▼
 DynamoDB
```

---

## 5. IoT Data

```text
Devices
   │
   ▼
Events
   │
   ▼
DynamoDB
```

---

# Quick Summary (DynamoDB)

```text
DynamoDB
│
├── NoSQL Database
│
├── Tables
│      └── Store Items
│
├── Items
│      └── Records
│
├── Attributes
│      └── Data Fields
│
├── Partition Key
│      └── Data Distribution
│
├── Sort Key
│      └── Ordered Queries
│
└── Serverless Scaling
```

---

# RDS (Relational Database Service)

Amazon RDS (Relational Database Service) is AWS's fully managed service for running relational databases.

AWS handles:

- Provisioning
- Patching
- Backups
- Monitoring
- High Availability

### Benefits

- Managed infrastructure
- Automated backups
- High availability
- Security integration
- Easy scaling

---

# Relational Database

RDS uses relational database engines that organize data into tables with rows and columns.

### Example

```text
Users
│
├── id
├── name
└── email
```

### Features

- SQL support
- ACID transactions
- Foreign keys
- Joins
- Indexes

---

# Supported Engines

RDS supports multiple database engines.

| Engine | Type |
|----------|---------|
| MySQL | Open Source |
| PostgreSQL | Open Source |
| MariaDB | Open Source |
| Oracle Database | Commercial |
| Microsoft SQL Server | Commercial |

### Example

```text
RDS
│
├── MySQL
├── PostgreSQL
├── MariaDB
├── Oracle
└── SQL Server
```

---

# DB Instances

A DB Instance is the actual database server running inside RDS.

It contains:

- CPU
- Memory
- Storage
- Database Engine

### Structure

```text
RDS
   │
   ▼
DB Instance
   │
   ▼
Database
```

### Example

```text
PostgreSQL DB Instance
      │
      ▼
Application
```

---

# Security

RDS integrates with AWS security services.

### Security Features

- VPC Integration
- Security Groups
- Encryption at Rest
- Encryption in Transit
- IAM Authentication (supported engines)

### Example

```text
Application
      │
      ▼
Security Group
      │
      ▼
RDS Database
```

---

# Backups

RDS automatically creates backups.

### Backup Types

#### Automated Backups

- Daily snapshots
- Point-in-time recovery
- Retention period configurable

#### Manual Snapshots

- User-created
- Persist until deleted

### Flow

```text
Database
    │
    ▼
Snapshot
    │
    ▼
Restore
```

---

# Multi-AZ

Multi-AZ provides high availability and automatic failover.

AWS maintains a standby database in another Availability Zone.

### Architecture

```text
Primary DB
     │
     ▼
Replication
     │
     ▼
Standby DB
```

### Failover

```text
AZ-1 Failure
      │
      ▼
Standby Promoted
      │
      ▼
Application Continues
```

### Benefits

- High availability
- Automatic failover
- Improved reliability

---

# Read Replicas

Read Replicas are read-only copies of a database.

Used to offload read traffic.

### Architecture

```text
Primary DB
     │
     ├── Read Replica 1
     │
     ├── Read Replica 2
     │
     └── Read Replica 3
```

### Usage

```text
Writes
  │
  ▼
Primary DB

Reads
  │
  ▼
Read Replicas
```

### Benefits

- Better performance
- Read scaling
- Reporting workloads

---

# RDS Use Cases

## 1. Web Applications

```text
Application
     │
     ▼
PostgreSQL/MySQL
     │
     ▼
RDS
```

---

## 2. E-Commerce Systems

```text
Users
  │
  ▼
Orders
  │
  ▼
RDS
```

---

## 3. Enterprise Applications

```text
Business Apps
      │
      ▼
Relational Data
      │
      ▼
RDS
```

---

## 4. Reporting Systems

```text
Analytics
    │
    ▼
Read Replica
```

---

## 5. Financial Applications

```text
Transactions
      │
      ▼
ACID Database
      │
      ▼
RDS
```

---

# DynamoDB vs RDS

| Feature | DynamoDB | RDS |
|----------|----------|-----|
| Database Type | NoSQL | Relational |
| Schema | Flexible | Fixed |
| SQL Support | No | Yes |
| Joins | No | Yes |
| Scaling | Horizontal | Vertical + Read Replicas |
| Performance | Very High | High |
| Transactions | Limited | Full ACID |
| Best For | Massive Scale Apps | Structured Relational Data |

---

