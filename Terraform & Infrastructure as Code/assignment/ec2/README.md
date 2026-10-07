# EC2

Amazon EC2 (Elastic Compute Cloud) is AWS's virtual machine service that allows you to run servers in the cloud.

---

## AMI (Amazon Machine Image)

An AMI is a template used to create EC2 instances.

It contains:

- Operating System
- Preinstalled software
- Configuration settings
- Storage configuration

### Examples

- Amazon Linux 2023
- Ubuntu 24.04
- Windows Server 2025

When launching an EC2 instance:

```text
AMI
 ↓
Create EC2 Instance
```

Example:

```text
Ubuntu AMI
   ↓
EC2 Instance
```

---

## Instance Types

An Instance Type defines the hardware resources of an EC2 instance.

It determines:

- CPU
- RAM
- Network performance
- Storage performance

### Format

```text
Family.Size
```

### Example

```text
t3.micro
m7i.large
c7g.xlarge
```

---

## Key Pairs

A Key Pair is used to securely connect to EC2 instances.

Consists of:

- Public Key (stored by AWS)
- Private Key (downloaded by you)

### Authentication flow

```text
Private Key
     ↓
SSH
     ↓
EC2 Instance
```

### Linux login

```bash
ssh -i mykey.pem ec2-user@PUBLIC_IP
```

### Best practices

- Never share private keys
- Store securely
- Rotate when necessary

---

## Security Groups

A Security Group acts as a virtual firewall for EC2 instances.

Controls:

- Incoming traffic (Inbound Rules)
- Outgoing traffic (Outbound Rules)

### Example

Allow:

```text
22  → SSH
80  → HTTP
443 → HTTPS
```

### Example configuration

| Type | Port | Source |
|--------|------|----------|
| SSH | 22 | Your IP |
| HTTP | 80 | 0.0.0.0/0 |
| HTTPS | 443 | 0.0.0.0/0 |

### Important

- Stateful firewall
- Deny rules do not exist
- Only Allow rules

---

## EBS (Elastic Block Store)

EBS is persistent storage attached to EC2 instances.

Similar to:

- SSD
- Hard Disk

Used for:

- Operating System
- Databases
- Application data

### Relationship

```text
EC2 Instance
      ↓
EBS Volume
```

### Characteristics

- Data persists after reboot
- Can create snapshots
- Can be resized

### Common types

| Type | Use Case |
|--------|-----------|
| gp3 | General-purpose SSD |
| io2 | High-performance SSD |
| st1 | Throughput workloads |
| sc1 | Low-cost HDD |

---

## Public vs Private IP

Every EC2 instance can have:

### Public IP

Accessible from the internet.

Example:

```text
44.212.10.5
```

Used for:

- Websites
- Public APIs
- SSH access

Flow:

```text
Internet
   ↓
Public IP
   ↓
EC2
```

### Private IP

Accessible only inside the VPC.

Example:

```text
10.0.1.25
```

Used for:

- Databases
- Internal services
- Backend communication

Flow:

```text
EC2 App Server
     ↓
Private IP
     ↓
Database Server
```

---

## Instance Lifecycle

An EC2 instance goes through several states.

```text
Pending
   ↓
Running
   ↓
Stopping
   ↓
Stopped
   ↓
Starting
   ↓
Running
```

Or:

```text
Running
   ↓
Terminated
```

### States

| State | Meaning |
|---------|-----------|
| Pending | Launching |
| Running | Active |
| Stopping | Shutting down |
| Stopped | Powered off |
| Starting | Booting |
| Terminated | Deleted permanently |

### Important

- Stopped → Can restart
- Terminated → Cannot recover

---

## Common Use Cases

### 1. Web Server

```text
Users
  ↓
EC2
  ↓
Nginx + Application
```

### 2. Backend API

```text
Frontend
   ↓
EC2
   ↓
REST API
```

### 3. Database Server

```text
Application
     ↓
EC2
     ↓
PostgreSQL/MySQL
```

(Though RDS is usually preferred.)

### 4. CI/CD Runner

```text
GitHub Actions
       ↓
EC2 Runner
       ↓
Build & Deploy
```

### 5. Machine Learning

```text
GPU EC2
    ↓
Model Training
```

Examples:

- PyTorch
- TensorFlow
- LLM Fine-tuning

### 6. Kubernetes Worker Nodes

```text
EKS Cluster
      ↓
EC2 Instances
      ↓
Run Pods
```