# Amazon S3 (Simple Storage Service)

Amazon S3 (**Simple Storage Service**) is AWS's highly scalable **object storage service** used to store and retrieve any amount of data from anywhere on the internet.

## Key Features

- Highly durable (**99.999999999% durability**)
- Virtually unlimited storage
- Secure and scalable
- Pay only for what you use
- Integrated with many AWS services

---

# Buckets

A **Bucket** is the top-level container in S3 that stores objects.

Think of a bucket as:

- A folder (conceptually)
- A storage container
- A namespace for your files

## Rules

- Bucket names must be **globally unique** across AWS.
- A bucket is created in a specific AWS Region.
- Buckets can contain an unlimited number of objects.

## Example Structure

```text
my-company-assets
├── images/
├── videos/
└── backups/
```

## Example Bucket Names

```text
my-app-assets
company-backups
user-profile-images
```

## How It Works

```text
AWS Account
     │
     ▼
  Bucket
     │
     ▼
  Objects
```

---

# Objects

An **Object** is the actual file stored inside an S3 bucket.

Every object consists of:

```text
Object
├── Data (file contents)
├── Key (file name/path)
├── Metadata
└── Version ID (optional)
```

## Example

Bucket:

```text
my-app-assets
```

Object Key:

```text
images/logo.png
```

Internally:

```text
Bucket
   │
   ▼
images/logo.png
```

## Important Note

S3 does **not** actually have folders.

What looks like folders are simply object keys:

```text
images/logo.png
images/banner.png
videos/demo.mp4
```

The `/` is simply part of the object's key name.

---

# Storage Classes

Storage classes allow you to optimize costs based on how frequently data is accessed.

| Storage Class | Use Case |
|-------------|-----------|
| S3 Standard | Frequently accessed data |
| S3 Standard-IA | Infrequent access |
| S3 One Zone-IA | Infrequent access stored in one AZ |
| Glacier Instant Retrieval | Rarely accessed archives |
| Glacier Flexible Retrieval | Backup archives |
| Glacier Deep Archive | Long-term archival |
| S3 Express One Zone | Ultra-low latency workloads |

## Cost Strategy

```text
Frequently Accessed
        │
        ▼
   S3 Standard
        │
        ▼
Standard-IA
        │
        ▼
Glacier
        │
        ▼
Deep Archive

Cost ↓
Access Time ↑
```

## Example

```text
Recent Photos
      ▼
 S3 Standard

Old Backups
      ▼
 Glacier Deep Archive
```

---

# Versioning

Versioning allows S3 to maintain multiple versions of the same object.

## Without Versioning

```text
report.pdf
     │
Upload New File
     ▼
Old File Lost
```

## With Versioning

```text
report.pdf
├── Version 1
├── Version 2
└── Version 3
```

## Benefits

- Recover deleted files
- Restore older versions
- Protect against accidental overwrites
- Improve disaster recovery

## Example

```text
Upload File
      │
      ▼
 Edit File
      │
      ▼
Previous Version Still Available
```

---

# Lifecycle Policies

Lifecycle policies automatically move or delete objects according to rules you define.

## Example Lifecycle

```text
Day 0
  │
  ▼
S3 Standard
  │
  │ 30 Days
  ▼
Standard-IA
  │
  │ 90 Days
  ▼
Glacier
  │
  │ 365 Days
  ▼
Delete
```

## Common Use Cases

- Application logs
- Backup archives
- Cost optimization
- Compliance retention

## Benefits

✅ Reduced storage costs

✅ Automated data management

✅ No manual cleanup required

---

# Encryption

Encryption protects data stored in S3.

## Server-Side Encryption (SSE)

AWS encrypts the data before storing it.

### SSE-S3

AWS manages encryption keys.

```text
File
 │
 ▼
AWS Encrypts
 │
 ▼
Stored in S3
```

### SSE-KMS

Uses AWS Key Management Service (KMS).

#### Benefits

- Fine-grained key control
- Audit logging
- Access control
- Compliance support

```text
File
 │
 ▼
KMS Key
 │
 ▼
Encrypted Object
```

### SSE-C

Customer supplies encryption keys.

```text
Customer Key
      │
      ▼
 AWS Encrypts Data
      │
      ▼
 Stored in S3
```

Less commonly used because of operational complexity.

## Client-Side Encryption

Data is encrypted before it reaches AWS.

```text
Application
      │
 Encrypt
      │
      ▼
Encrypted Data
      │
      ▼
     S3
```

AWS never sees the unencrypted data.

---

# Bucket Policies

A **Bucket Policy** is a JSON document that controls access to a bucket.

It defines:

- Who can access the bucket
- What actions are allowed
- Which resources are affected

## Example

```json
{
  "Effect": "Allow",
  "Principal": "*",
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::my-bucket/*"
}
```

## Common Controls

- Public read access
- Cross-account access
- Restrict uploads
- Restrict downloads
- Enforce encryption

## Bucket Policy vs IAM Policy

| IAM Policy | Bucket Policy |
|------------|--------------|
| Attached to users, groups, or roles | Attached to buckets |
| Controls identity permissions | Controls resource permissions |
| Works across AWS services | Specific to S3 resources |

## Access Flow

```text
User
 │
 ▼
IAM Policy
 │
 ▼
S3 Bucket
 │
 ▼
Bucket Policy
 │
 ▼
Access Granted / Denied
```

---

# Common Use Cases

## 1. Static Website Hosting

```text
S3 Bucket
     │
     ▼
HTML / CSS / JS
     │
     ▼
 Static Website
```

Examples:

- Portfolio websites
- Documentation sites
- Landing pages

---

## 2. Image Storage

```text
Users
  │
Upload
  │
  ▼
  S3
  │
  ▼
Images
```

Common in:

- Social media platforms
- E-commerce applications
- Content management systems

---

## 3. Backups

```text
Database
    │
 Backup
    │
    ▼
    S3
```

Used for:

- Database backups
- Server backups
- Disaster recovery

---

## 4. Log Storage

```text
Application
      │
      ▼
     Logs
      │
      ▼
      S3
```

Examples:

- CloudTrail logs
- Application logs
- Audit logs

---

## 5. Data Lake

```text
Raw Data
    │
    ▼
    S3
    │
    ▼
Athena / Glue / EMR
    │
    ▼
Analytics & ML
```

Used for:

- Data analytics
- Machine learning
- Big data processing

---

## 6. Media Storage

```text
Videos
Images
Audio
  │
  ▼
  S3
```

Commonly used with:

- Streaming platforms
- Content delivery systems
- Media processing pipelines

---
