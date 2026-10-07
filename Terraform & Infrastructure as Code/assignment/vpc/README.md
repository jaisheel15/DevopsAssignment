# VPC (Virtual Private Cloud)

Amazon VPC (Virtual Private Cloud) is a logically isolated virtual network within AWS where you can launch and manage AWS resources securely.

Think of a VPC as your own private data center inside AWS.

### Benefits

- Network isolation
- IP address control
- Custom routing
- Security controls
- Public and private resources

### Example

```text
AWS Cloud
│
└── VPC (10.0.0.0/16)
     │
     ├── Public Subnet
     │
     └── Private Subnet
```

---

# CIDR

CIDR (Classless Inter-Domain Routing) defines the IP address range available within a network.

### Format

```text
IP_ADDRESS/PREFIX
```

Example:

```text
10.0.0.0/16
```

### Meaning

```text
10.0.0.0 = Network Address
/16      = First 16 bits are network bits
```

### Common CIDR Blocks

| CIDR | Available IPs |
|--------|--------------|
| /24 | 256 |
| /23 | 512 |
| /22 | 1024 |
| /16 | 65,536 |

### Example

```text
VPC CIDR
10.0.0.0/16

Range:
10.0.0.0
to
10.0.255.255
```

---

# Subnets

A Subnet is a smaller network segment inside a VPC.

Subnets help organize resources and control traffic.

### Example

```text
VPC: 10.0.0.0/16
│
├── Public Subnet
│   10.0.1.0/24
│
└── Private Subnet
    10.0.2.0/24
```

### Benefits

- Better network organization
- Security isolation
- Availability Zone distribution
- Workload separation

### Example Architecture

```text
VPC
│
├── Public Subnet
│     ├── Load Balancer
│     └── Bastion Host
│
└── Private Subnet
      ├── EC2 Application
      └── Database
```

---

# Route Tables

A Route Table determines where network traffic should go.

Every subnet must be associated with a route table.

### Example

| Destination | Target |
|------------|---------|
| 10.0.0.0/16 | Local |
| 0.0.0.0/0 | Internet Gateway |

### Traffic Flow

```text
EC2
 │
 ▼
Route Table
 │
 ▼
Destination
```

### Example

```text
Request to Google
      │
      ▼
Destination:
0.0.0.0/0
      │
      ▼
Internet Gateway
```

---

# Internet Gateway (IGW)

An Internet Gateway allows communication between a VPC and the internet.

Without an Internet Gateway, resources inside a VPC cannot access the public internet directly.

### Flow

```text
Internet
    │
    ▼
Internet Gateway
    │
    ▼
Public Subnet
```

### Requirements for Public Internet Access

A resource needs:

- Public IP or Elastic IP
- Route to Internet Gateway
- Security Group permissions

### Example

```text
User
 │
 ▼
Internet
 │
 ▼
Internet Gateway
 │
 ▼
Web Server
```

---

# NAT Gateway

A NAT Gateway allows resources in private subnets to access the internet without exposing them to inbound internet traffic.

### Why NAT?

Private servers often need internet access for:

- Software updates
- Downloading packages
- Accessing AWS APIs

But they should not be publicly reachable.

### Flow

```text
Private EC2
      │
      ▼
NAT Gateway
      │
      ▼
Internet Gateway
      │
      ▼
Internet
```

### Important

- Outbound internet access allowed
- Inbound internet access blocked
- NAT Gateway must be placed in a public subnet

---

# Security Groups

A Security Group acts as a virtual firewall for AWS resources.

Controls:

- Inbound traffic
- Outbound traffic

### Example Rules

| Type | Port | Source |
|--------|------|---------|
| SSH | 22 | Your IP |
| HTTP | 80 | 0.0.0.0/0 |
| HTTPS | 443 | 0.0.0.0/0 |

### Characteristics

- Stateful
- Instance-level security
- Only allow rules
- No explicit deny rules

### Flow

```text
Internet
    │
    ▼
Security Group
    │
    ▼
EC2 Instance
```

### Example

```text
Allow:
22  → SSH
80  → HTTP
443 → HTTPS
```

---

# Network ACLs (NACLs)

A Network ACL is a subnet-level firewall that controls traffic entering and leaving a subnet.

### Characteristics

- Stateless
- Subnet-level security
- Supports Allow rules
- Supports Deny rules

### Example

| Rule | Type | Port | Action |
|--------|------|------|---------|
| 100 | HTTP | 80 | Allow |
| 200 | SSH | 22 | Deny |

### Flow

```text
Internet
    │
    ▼
Network ACL
    │
    ▼
Subnet
    │
    ▼
EC2
```

### Security Group vs NACL

| Feature | Security Group | NACL |
|-----------|---------------|------|
| Level | Instance | Subnet |
| Stateful | Yes | No |
| Allow Rules | Yes | Yes |
| Deny Rules | No | Yes |
| Evaluated | Instance Traffic | Subnet Traffic |

---

# Public vs Private Subnet

The difference depends on routing.

---

## Public Subnet

A subnet is public if it has a route to an Internet Gateway.

### Route Table

```text
0.0.0.0/0
      ↓
Internet Gateway
```

### Example Resources

- Load Balancers
- Bastion Hosts
- Public Web Servers
- NAT Gateways

### Flow

```text
Internet
    │
    ▼
Internet Gateway
    │
    ▼
Public Subnet
```

---

## Private Subnet

A subnet is private if it does not have a direct route to an Internet Gateway.

### Route Table

```text
0.0.0.0/0
      ↓
NAT Gateway
```

### Example Resources

- Databases
- Backend Services
- Internal APIs
- Application Servers

### Flow

```text
Internet
    │
    ▼
NAT Gateway
    │
    ▼
Private Subnet
```

### Benefits

- Increased security
- No direct internet exposure
- Better protection for sensitive workloads

---

# Typical AWS VPC Architecture

```text
VPC (10.0.0.0/16)
│
├── Public Subnet (10.0.1.0/24)
│     │
│     ├── Internet Gateway
│     ├── Load Balancer
│     ├── Bastion Host
│     └── NAT Gateway
│
└── Private Subnet (10.0.2.0/24)
      │
      ├── Application Server
      ├── Backend APIs
      └── Database
```

---

# Request Flow Example

```text
User
 │
 ▼
Internet
 │
 ▼
Internet Gateway
 │
 ▼
Load Balancer
 │
 ▼
Application Server
 │
 ▼
Database
```

---
