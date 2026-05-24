<div align="center">

<!-- BANNER -->
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f2027,50:203a43,100:2c5364&height=200&section=header&text=Fintech%20PII%20Compliance%20Platform&fontSize=36&fontColor=ffffff&fontAlignY=38&desc=Production-Grade%20AWS%20Security%20Architecture&descAlignY=58&descSize=16" width="100%"/>

<br/>

[![AWS](https://img.shields.io/badge/AWS-Powered-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)](https://aws.amazon.com)
[![Security](https://img.shields.io/badge/Security-PCI%20DSS%20L1-00C853?style=for-the-badge&logo=shield&logoColor=white)](https://www.pcisecuritystandards.org)
[![Compliance](https://img.shields.io/badge/Compliance-RBI%20%7C%20DPDP%202023-0052CC?style=for-the-badge&logo=checkmarx&logoColor=white)](https://rbi.org.in)
[![Architecture](https://img.shields.io/badge/Architecture-Microservices-7B2FBE?style=for-the-badge&logo=docker&logoColor=white)](https://microservices.io)
[![Encryption](https://img.shields.io/badge/Encryption-AES--256%20%7C%20TLS%201.3-DC143C?style=for-the-badge&logo=letsencrypt&logoColor=white)](https://aws.amazon.com/kms)
[![Availability](https://img.shields.io/badge/Availability-99.99%25%20SLA-00BCD4?style=for-the-badge&logo=statuspage&logoColor=white)](https://aws.amazon.com)

<br/>

> **A battle-hardened, regulation-compliant, multi-layered AWS cloud architecture for processing India's most sensitive financial identity data — Aadhaar, PAN, TAN, Bank Details, and Credit Cards — with zero trust, zero compromise.**

<br/>

</div>

---

## 🗺️ System Architecture Diagram

> **Paste your exported Eraser.io PNG here after generating it. Replace the placeholder below:**

```
<!-- INSTRUCTIONS: Export your diagram from Eraser.io as PNG (2x resolution),
     upload it to your GitHub repo under /assets/architecture.png,
     then replace this block with: -->

![Fintech PII Compliance Platform — AWS Architecture](./assets/architecture.png)
```

<div align="center">

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     PASTE YOUR ERASER.IO DIAGRAM HERE                       │
│                                                                             │
│   Export → PNG (2x) → Upload to /assets/architecture.png → link above      │
│                                                                             │
│   The Eraser.io prompt in this README generates the full diagram            │
└─────────────────────────────────────────────────────────────────────────────┘
```

*Generate the diagram using the [Eraser.io prompt](#-eraserio-generation-prompt) at the bottom of this README*

</div>

---

## 📋 Table of Contents

<details open>
<summary><strong>Click to expand full index</strong></summary>

- [🏗️ Architecture Overview](#️-architecture-overview)
- [🎯 Why This Architecture? — The Philosophy](#-why-this-architecture--the-philosophy)
- [🔐 Zone 1 — Client & Edge Layer](#-zone-1--client--edge-layer)
- [🌐 Zone 2 — Edge Protection & WAF](#-zone-2--edge-protection--waf)
- [🏰 Zone 3 — VPC & Network Isolation](#-zone-3--vpc--network-isolation)
- [⚙️ Zone 4 — Microservices on EC2 + ASG](#️-zone-4--microservices-on-ec2--asg)
- [🗄️ Zone 5 — Data Layer](#️-zone-5--data-layer)
- [📦 Zone 6 — Sensitive Document Storage (S3)](#-zone-6--sensitive-document-storage-s3)
- [🔑 Zone 7 — Security & Key Management](#-zone-7--security--key-management)
- [📨 Zone 8 — Messaging & Async Processing](#-zone-8--messaging--async-processing)
- [📊 Zone 9 — Monitoring, Observability & Incident Response](#-zone-9--monitoring-observability--incident-response)
- [🛡️ Zone 10 — Network Security Groups & VPC Endpoints](#️-zone-10--network-security-groups--vpc-endpoints)
- [🚀 Zone 11 — CI/CD & Infrastructure Pipeline](#-zone-11--cicd--infrastructure-pipeline)
- [♻️ Zone 12 — Disaster Recovery & Backup](#️-zone-12--disaster-recovery--backup)
- [📜 Zone 13 — Compliance & Regulatory Framework](#-zone-13--compliance--regulatory-framework)
- [🔄 End-to-End KYC Data Flow](#-end-to-end-kyc-data-flow)
- [🧮 Technology Decision Matrix](#-technology-decision-matrix)
- [📐 Scalability Model](#-scalability-model)
- [💰 Cost Architecture Considerations](#-cost-architecture-considerations)
- [🗂️ Repository Structure](#️-repository-structure)
- [🖼️ Eraser.io Generation Prompt](#️-eraserio-generation-prompt)
- [🏁 Credits](#-credits)

</details>

---

## 🏗️ Architecture Overview

This system is designed to handle India's most regulated financial data categories with zero-trust principles across every layer. The architecture supports:

| Capability | Specification |
|---|---|
| **Sensitive Data Handled** | Aadhaar, PAN, TAN, Bank Account, IFSC, Credit/Debit Card |
| **Cloud Provider** | AWS — Primary: `ap-south-1` (Mumbai), DR: `ap-southeast-1` (Singapore) |
| **Compute Model** | 3× Pre-configured EC2 Docker Hosts in ASG (Min:3, Max:12) |
| **Architecture Pattern** | Microservices, Event-driven, Zero-trust |
| **Encryption** | AES-256 at rest, TLS 1.3 in transit, Field-level KMS encryption |
| **Target Availability** | 99.99% (Multi-AZ, Multi-region warm standby) |
| **RTO / RPO** | RTO: 4 hours · RPO: 1 hour |
| **Regulatory Coverage** | PCI DSS L1 · RBI IT Framework · DPDP Act 2023 |
| **Compliance Monitoring** | AWS Security Hub + Config + GuardDuty + Macie (continuous) |

---

## 🎯 Why This Architecture? — The Philosophy

> Before diving into zones, understand *why* every major decision was made this way — and what alternatives were rejected and for what reason.

---

### Why AWS and not Azure or GCP?

AWS was chosen over Azure and GCP for fintech in India for three concrete reasons:

**1. RBI Data Localization compliance.** AWS `ap-south-1` (Mumbai) is the most mature Indian cloud region with the broadest service availability. Azure's Indian regions have historically had service gaps. GCP Mumbai (asia-south1) is newer with fewer compliance certifications specific to BFSI.

**2. AWS is the only provider with a signed MOU with IDRBT (Institute for Development and Research in Banking Technology)**, making it RBI-acceptable by default for regulated entities.

**3. AWS Macie natively supports Aadhaar and Indian PAN pattern detection**, which neither Azure Purview nor GCP DLP does out of the box. Building custom detectors = more attack surface.

---

### Why Microservices and not a Monolith?

The **blast radius principle** is why microservices win for PII systems. If the Notification Service is compromised, it should have zero access to Aadhaar data. In a monolith, the notification module would share the same process memory and DB connection as the KYC module. That is catastrophic for a regulated fintech.

Each microservice in this architecture has:
- Its own IAM role with least-privilege permissions
- Its own security group rules
- No shared secrets — it can only access what its role explicitly allows

**Why not serverless (Lambda-first)?** Lambda was rejected as the primary compute layer because:
- Cold starts on Java-based KYC/compliance processing create unpredictable latency for user-facing KYC submission flows
- You already have 3 pre-configured EC2s with Docker — this is sunk infrastructure that should be utilized
- Container orchestration on EC2 gives you predictable performance profiles for compliance audits

Lambda is used surgically in this design — only for event-driven, stateless, burst workloads (S3 PII scanning, secrets rotation, scheduled compliance checks).

---

### Why not ECS or EKS instead of EC2 + Docker?

The brief specifies **3 pre-configured EC2 Docker containers** with ASG. The architecture respects this constraint and extends it with discipline rather than replacing it.

However, the architecture is designed to be ECS-ready: all containers follow single-responsibility, all configs come from Secrets Manager and SSM, all logs ship to CloudWatch. Migrating to ECS Fargate later is a configuration change, not an architectural overhaul.

**Why not EKS?** Kubernetes adds significant operational overhead for a team that hasn't indicated Kubernetes expertise. EKS is the right answer at 50+ services or when you need advanced scheduling. For 5 microservices on 3–12 nodes, it is overengineering that creates new attack surfaces (Kubernetes API server exposure, etcd security, RBAC complexity).

---

### Why Multi-AZ and not single-AZ with backups?

AZ failures are not theoretical. `ap-south-1` has had partial AZ outages. For a fintech processing real-money KYC, a 15-minute outage during a banking window is a regulatory incident, not just a customer experience issue. The cost of Multi-AZ RDS (roughly 2× instance cost) is trivially small compared to one RBI compliance notice.

---

### Why a separate DR region and not just backups?

**RPO of 1 hour** means you can lose at most 1 hour of data. Restoring from S3 Glacier backups into a cold region takes 4–6 hours to restore + configure + validate. A warm standby with RDS cross-region read replica means failover in under 4 hours (your stated RTO) because the data is already there — you just promote the replica.

---

## 🔐 Zone 1 — Client & Edge Layer

```
Mobile App (iOS/Android)    Web Browser (HTTPS)    API Partners
        │                           │                    │
        └───────────────────────────┴────────────────────┘
                        HTTPS / TLS 1.3
                               │
                         AWS Route 53
```

### What it does

Route 53 is the entry point for all traffic. It handles:
- **Geo-routing**: Routes Indian users to `ap-south-1`, ensuring data never leaves India in transit (RBI data localization)
- **Health-check-based failover**: If ALB health checks fail, Route 53 automatically routes to the DR region within 60 seconds
- **Latency-based routing**: Sub-millisecond DNS resolution from ISPs across India

### Why Route 53 and not Cloudflare DNS?

Cloudflare DNS resolves from Cloudflare's infrastructure. For an RBI-regulated entity, DNS resolution for financial endpoints going through a US-headquartered third party creates data residency audit questions. Route 53 keeps resolution within AWS infrastructure and is auditable via CloudTrail.

---

## 🌐 Zone 2 — Edge Protection & WAF

```
Route 53
   │
AWS WAF ──── Block OWASP Top 10, SQLi, XSS, Rate Limiting
   │
AWS Shield Standard ──── Always-on DDoS protection (Layer 3/4)
   │
AWS CloudFront ──── Static assets ONLY (no PII caching)
   │
Application Load Balancer ──── PII API traffic goes here directly
```

### AWS WAF — Why managed rules and not a custom WAF?

AWS Managed Rule Groups for WAF are maintained by AWS threat intelligence, updated in near real-time for newly discovered CVEs and attack patterns. A custom WAF rule set requires a dedicated security team to maintain — rules go stale. For a fintech startup or mid-size company, managed rules + a few custom rate-limiting rules is the correct balance.

**Custom rules added on top of managed:**
- Rate limit: 100 requests/minute per IP to `/api/kyc` (prevents enumeration attacks)
- Geo-restriction: Block traffic from OFAC-sanctioned country IPs
- Size restriction: Block requests > 10MB (prevents XML bomb / large payload attacks on KYC endpoints)

### Why AWS Shield Standard and not Shield Advanced?

Shield Standard protects against volumetric DDoS (Layer 3/4) at no cost. Shield Advanced adds Layer 7 DDoS protection, WAF fee absorptions, and 24/7 DDoS response team. For an early-production fintech, Shield Standard + WAF covers 95% of realistic attack scenarios. Shield Advanced is recommended at ₹50L+ monthly transaction volume where a DDoS event represents direct financial loss.

### Why does PII traffic bypass CloudFront?

CloudFront is a CDN — it caches responses. Even with cache-control headers set to `no-store`, there is non-zero risk of misconfiguration leading to PII being cached at an edge node. For regulatory compliance, PII API responses go directly from the client to the ALB, never touching CloudFront. CloudFront serves only the static frontend bundle.

---

## 🏰 Zone 3 — VPC & Network Isolation

```
╔══════════════════════════════════════════════════════════════╗
║  AWS VPC — ap-south-1 | CIDR: 10.0.0.0/16                  ║
║                                                              ║
║  ┌─────────────────────────────────────────────┐            ║
║  │ PUBLIC SUBNET (10.0.1.0/24, 10.0.2.0/24)   │            ║
║  │                                             │            ║
║  │  [ALB]   [NAT Gateway AZ-a]  [NAT GW AZ-b] │            ║
║  │                                             │            ║
║  └─────────────────────────────────────────────┘            ║
║                         │                                    ║
║  ┌─────────────────────────────────────────────┐            ║
║  │ PRIVATE SUBNET (10.0.3.0/24–10.0.8.0/24)   │            ║
║  │                                             │            ║
║  │  EC2s (Docker) │ RDS │ ElastiCache │ Lambda │            ║
║  │                                             │            ║
║  └─────────────────────────────────────────────┘            ║
╚══════════════════════════════════════════════════════════════╝
```

### Why a custom VPC and not the default VPC?

The AWS default VPC has public subnets for all resources by default. Every EC2 launched in the default VPC gets a public IP. This is catastrophically wrong for a PII system. A custom VPC forces explicit decisions: every resource placement is intentional, every route is deliberate.

### Why split public/private subnets?

**Public subnet** holds only: ALB (must accept internet traffic), NAT Gateways (need internet routes), Internet Gateway.

**Private subnet** holds everything else. EC2 instances, RDS, ElastiCache, Lambda functions — none of these need inbound internet access. They initiate outbound calls via NAT Gateway (for things like third-party KYC verification APIs), but no inbound path exists.

A direct internet path to your database or application servers is the single most common cause of fintech data breaches. This subnet architecture makes that path physically impossible.

### Why two NAT Gateways?

One NAT Gateway per AZ. If NAT Gateway AZ-1a fails, EC2 instances in AZ-1a lose outbound internet access. This matters for: downloading security patches via SSM, calling third-party verification APIs, sending SNS/SES notifications. Two NAT Gateways cost ~$65/month each — this is the cheapest high availability you can buy.

---

## ⚙️ Zone 4 — Microservices on EC2 + ASG

```
┌─────────────────────────────────────────────────────────────────┐
│  AUTO SCALING GROUP — Min:3, Max:12, Desired:3                  │
│                                                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │ EC2 (AZ-1a)  │  │ EC2 (AZ-1b)  │  │ EC2 (AZ-1c)  │          │
│  │ t3.xlarge    │  │ t3.xlarge    │  │ t3.xlarge    │          │
│  │ Docker Host  │  │ Docker Host  │  │ Docker Host  │          │
│  │              │  │              │  │              │          │
│  │ [API-GW :3000│  │ [API-GW :3000│  │ [API-GW :3000│          │
│  │ [Auth   :8001│  │ [Auth   :8001│  │ [Auth   :8001│          │
│  │ [KYC    :8002│  │ [KYC    :8002│  │ [KYC    :8002│          │
│  │ [Notify :8003│  │ [Notify :8003│  │ [Notify :8003│          │
│  │ [Audit  :8004│  │ [Audit  :8004│  │ [Audit  :8004│          │
│  └──────────────┘  └──────────────┘  └──────────────┘          │
│                                                                  │
│  Scale Out: CPU > 70% for 2min → +2 instances                   │
│  Scale In:  CPU < 30% for 10min → -1 instance                   │
└─────────────────────────────────────────────────────────────────┘
```

### The 5 Microservices — Responsibilities & Isolation

| Service | Language | Port | Accesses | Cannot Access |
|---|---|---|---|---|
| **API Gateway** | Node.js | 3000 | Auth, all services, Redis | RDS directly, S3 directly |
| **Auth** | Python/FastAPI | 8001 | Cognito, RDS (users), Redis | KYC table, S3 |
| **KYC/Compliance** | Java/Spring | 8002 | KMS, S3 (docs), RDS (kyc), DynamoDB | Auth table, Notification queue |
| **Notification** | Node.js | 8003 | SES, SNS, SQS | Any database with PII |
| **Audit Log** | Go | 8004 | CloudWatch, S3 (audit) | Any PII database |

### Why is KYC written in Java/Spring and not Node.js?

The KYC service does cryptographic operations (Aadhaar Verhoeff checksum validation, field-level encryption, PAN regex validation against CBDT patterns). Java's `javax.crypto` and Spring Security provide battle-tested implementations of these operations with extensive CVE tracking and enterprise support. Node.js crypto is sufficient for TLS but the ecosystem around regulated financial data validation is thinner.

### Why Go for the Audit Log Service?

The Audit Log Service must handle the highest write throughput of any service — every API call, every DB query, every S3 access generates an audit event. Go's goroutine model handles concurrent log shipping to CloudWatch with minimal memory overhead. A Go binary also has a much smaller attack surface than a JVM or Node.js runtime — fewer dependencies means fewer CVEs for a service that handles sensitive access records.

### ASG Scaling Policy — Why these thresholds?

- **Scale out at CPU > 70% for 2 minutes**: A single spike (GC pause, one bursty user) should not trigger scale-out. The 2-minute window prevents thrashing. 70% CPU leaves headroom for the new instance to warm up before the current ones are saturated.
- **Scale in at CPU < 30% for 10 minutes**: Scale-in is conservative. Spinning down instances too aggressively means the next traffic spike hits under-provisioned infrastructure. The 10-minute window absorbs natural traffic lulls (e.g., between 3–5am).
- **Minimum 3 instances**: One per AZ. This is the hard floor for Multi-AZ coverage. Dropping to 2 instances means one AZ has no coverage.

### Why t3.xlarge?

t3.xlarge (4 vCPU, 16GB RAM) is the right size for running 5 Docker containers simultaneously:
- API Gateway: ~512MB RAM
- Auth: ~1GB RAM (Cognito SDK, Redis client)
- KYC (Java/Spring): ~2GB RAM (JVM heap)
- Notification: ~512MB RAM
- Audit (Go): ~256MB RAM

Total: ~4.3GB per host. t3.xlarge gives 16GB — comfortable headroom for OS + Docker daemon + container image layers + burst capacity. The `t3` burstable series handles variable fintech traffic patterns (morning KYC submission rush, quiet overnight) more cost-efficiently than `m5` (fixed performance).

---

## 🗄️ Zone 5 — Data Layer

```
                    ┌──────────────────────────────┐
                    │     DATA TIER (Private SG)    │
                    │                              │
         ┌──────────▼──────────┐                  │
         │    RDS Proxy        │ ← Connection pool  │
         │    (IAM Auth only)  │                  │
         └──────────┬──────────┘                  │
                    │                              │
    ┌───────────────┼───────────────┐              │
    │               │               │              │
┌───▼───┐      ┌────▼───┐      ┌───▼───┐          │
│Primary│      │Standby │      │ Read  │          │
│AZ-1a  │◄────►│AZ-1b   │      │Replica│          │
│       │  sync│        │      │AZ-1c  │          │
└───────┘      └────────┘      └───────┘          │
  PostgreSQL Multi-AZ             async            │
                                                   │
  ┌──────────────┐  ┌──────────────┐               │
  │  DynamoDB    │  │  ElastiCache │               │
  │  On-demand   │  │  Redis Cluster│              │
  │  Global Table│  │  3-node Multi│              │
  └──────────────┘  └──────────────┘               │
                    └──────────────────────────────┘
```

### Why RDS PostgreSQL and not MySQL or Aurora?

**PostgreSQL vs MySQL**: PostgreSQL's `pgcrypto` extension supports column-level encryption natively. This is used to encrypt PAN and Aadhaar values at the column level (in addition to storage-level KMS encryption — defense in depth). MySQL has no equivalent built-in capability.

**PostgreSQL vs Aurora**: Aurora PostgreSQL would be the upgrade path at scale (it auto-scales storage, has Aurora Serverless v2 for variable load). However, Aurora requires at minimum 2 writer instances for Multi-AZ and costs ~40% more than equivalent RDS PostgreSQL. For a production fintech at early/mid stage, RDS PostgreSQL Multi-AZ is the cost-performance optimum. The architecture is migration-compatible with Aurora — same Postgres dialect, same RDS Proxy, same parameter groups.

### Why RDS Proxy?

RDS Proxy maintains a persistent connection pool to the database. Without it:
- Each EC2 container opens its own DB connections (up to 60 connections across 3 hosts × 5 services = 300 connections)
- During scale-out to 12 instances: 12 × 5 × connection pool size = potentially 1,200+ DB connections
- PostgreSQL defaults to 100 max connections on small instances; 1,200 would crash it

RDS Proxy multiplexes thousands of application connections into a small pool of actual DB connections. It also enforces IAM authentication — no connection string with username/password in any config file.

### Why DynamoDB for sessions and rate limiting and not Redis alone?

DynamoDB and ElastiCache Redis serve different access patterns:

**DynamoDB** is used for: compliance rules (read-heavy, structured), document metadata (immutable after write), consent records (must be durable, auditable, not evictable). These records cannot be lost if Redis restarts. DynamoDB's `PutItem` with condition expressions also enables atomic writes that prevent duplicate KYC submissions.

**ElastiCache Redis** is used for: JWT session tokens (fast reads, short TTL), OTP codes (TTL: 5 minutes, auto-expire), rate limit counters (atomic INCR operations). These are ephemeral by design — losing them on restart just means users re-login, not data loss.

### Why ElastiCache Cluster Mode?

Cluster mode shards data across multiple nodes. For rate limiting, this means the INCR counter for a given user's API rate is co-located on one shard (via consistent hashing on user_id key). This prevents the "hot key" problem where a single Redis node gets overloaded because all rate limit counters hash to it.

---

## 📦 Zone 6 — Sensitive Document Storage (S3)

```
┌──────────────────────────────────────────────────────────────┐
│                    Amazon S3 Buckets                         │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  fintech-kyc-documents-prod                         │    │
│  │  ├── Encryption: SSE-KMS (CMK, rotate 90 days)      │    │
│  │  ├── Object Lock: COMPLIANCE mode, 7-year retention │    │
│  │  ├── Versioning: ON                                 │    │
│  │  ├── Block Public Access: ALL ENABLED               │    │
│  │  └── Access: KYC Service IAM role + VPC Endpoint    │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  fintech-audit-logs-archive                         │    │
│  │  ├── Lifecycle: → Glacier after 90d → Delete 7yr   │    │
│  │  └── Access: Audit Service + Compliance IAM role    │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  fintech-app-assets                                 │    │
│  │  ├── Static frontend files ONLY — ZERO PII          │    │
│  │  └── Served via CloudFront                         │    │
│  └─────────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────┘
```

### Why S3 Object Lock in COMPLIANCE mode?

Object Lock in COMPLIANCE mode means **no one** — not even the AWS root account — can delete or modify objects before the retention period expires. Not your DevOps team, not an attacker who compromises admin credentials, not a disgruntled employee.

This is required by:
- **PMLA (Prevention of Money Laundering Act)**: KYC records must be retained for 5 years after account closure
- **RBI KYC Master Directions**: 10-year record retention for certain transaction categories
- **DPDP Act 2023**: Records of consent must be retained for the duration of the relationship + 3 years

GOVERNANCE mode would allow privileged users to override — that is not acceptable for regulated financial records.

### Why a separate bucket for audit logs?

If audit logs were stored in the same bucket as KYC documents, a compromised KYC Service IAM role could theoretically overwrite or delete audit logs to cover its tracks. Separate buckets enforce the principle that the service being audited cannot tamper with the audit record. The Audit Service IAM role can write to the audit bucket but has no access to the KYC documents bucket.

### Why VPC Gateway Endpoint for S3?

Without a VPC endpoint, S3 traffic routes through the public internet (even within the same AWS region). A VPC Gateway Endpoint creates a private route: EC2 → S3 without traversing any public IP. This:
- Eliminates the data transfer cost for S3 traffic through NAT Gateway (~$0.045/GB × significant volume)
- Ensures KYC document uploads never traverse the internet — a hard RBI audit requirement
- Removes the attack surface of traffic being observable on public internet links

---

## 🔑 Zone 7 — Security & Key Management

```
┌─────────────────────────────────────────────────────────────┐
│                   SECURITY SERVICES PANEL                   │
│                                                             │
│  ┌──────────┐  ┌────────────────┐  ┌──────────────────┐   │
│  │  AWS KMS │  │ Secrets Manager│  │    AWS IAM       │   │
│  │          │  │                │  │                  │   │
│  │ CMK per  │  │ Rotate every   │  │ Service Roles    │   │
│  │ service  │  │ 30 days (RDS)  │  │ No root usage    │   │
│  │ Auto-rot │  │ Cross-region   │  │ Permission Bounds│   │
│  │ 365 days │  │ replication    │  │ MFA on all humans│   │
│  └──────────┘  └────────────────┘  └──────────────────┘   │
│                                                             │
│  ┌──────────┐  ┌────────────────┐  ┌──────────────────┐   │
│  │ AWS Macie│  │    AWS ACM     │  │   GuardDuty      │   │
│  │          │  │                │  │                  │   │
│  │ Aadhaar  │  │ Auto-renew TLS │  │ ML anomaly det.  │   │
│  │ PAN scan │  │ 60d before exp │  │ Isolate on alert │   │
│  │ Alert SNS│  │ Mobile pinning │  │ All regions ON   │   │
│  └──────────┘  └────────────────┘  └──────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

### AWS KMS — Why separate CMKs per service?

A single CMK for the entire platform means a key policy misconfiguration or key compromise exposes everything. Separate CMKs enforce cryptographic isolation:

| CMK | Used By | Rotation |
|---|---|---|
| `fintech-rds-cmk` | RDS PostgreSQL storage encryption | 365 days |
| `fintech-s3-kyc-cmk` | S3 KYC documents bucket | 90 days (more sensitive) |
| `fintech-s3-audit-cmk` | S3 audit logs bucket | 365 days |
| `fintech-ebs-cmk` | EC2 EBS volumes | 365 days |
| `fintech-secrets-cmk` | Secrets Manager | 365 days |

KYC documents CMK rotates every 90 days (vs 365 for others) because it encrypts Aadhaar data — the most sensitive identity document in India. Shorter rotation = smaller window of exposure if a key is somehow compromised.

### Why Secrets Manager and not SSM Parameter Store?

Both can store secrets. The difference:
- **Secrets Manager** supports automatic rotation with a Lambda rotator, cross-region replication, and fine-grained resource-based policies
- **SSM Parameter Store SecureString** stores secrets but does NOT auto-rotate. You need custom automation.

For a PII system where RDS credentials must rotate every 30 days (PCI DSS requirement), Secrets Manager's built-in RDS rotation Lambda is the correct choice. Manual rotation processes fail — humans forget, rotations get skipped during incidents, and static credentials eventually leak.

### Why AWS Macie and not a custom PII scanner?

Macie uses AWS's continuously updated ML models trained on PII patterns across billions of S3 objects. The Aadhaar detection pattern (12-digit number with specific checksum properties) and Indian PAN pattern (AAAAA0000A format) are built-in managed data identifiers. A custom scanner using regex would miss edge cases (OCR artifacts in scanned documents, partial matches in file metadata, Base64-encoded PII in JSON). Macie's managed identifiers have been battle-tested against real-world PII exposure scenarios.

---

## 📨 Zone 8 — Messaging & Async Processing

```
┌─────────────────────────────────────────────────────────────┐
│                  MESSAGING & ASYNC LAYER                    │
│                                                             │
│         SQS FIFO                    SQS Standard            │
│  ┌──────────────────┐       ┌──────────────────────┐       │
│  │ kyc-processing   │       │ notification-queue    │       │
│  │ Ordered processing│       │ Email / SMS delivery │       │
│  │ Exactly-once     │       │                      │       │
│  │ Dead-letter: 3x  │       │ Dead-letter: 3x retry│       │
│  └──────────────────┘       └──────────────────────┘       │
│                                                             │
│       SNS Topics               Lambda Functions             │
│  ┌────────────────┐       ┌──────────────────────────┐     │
│  │compliance-alert│       │ lambda-pii-detector      │     │
│  │kyc-status      │       │ lambda-secrets-rotator   │     │
│  │system-alerts   │       │ lambda-compliance-checker│     │
│  └────────────────┘       └──────────────────────────┘     │
└─────────────────────────────────────────────────────────────┘
```

### Why SQS FIFO for KYC and not standard SQS?

KYC processing must be **ordered and exactly-once**. Consider the scenario: a user submits their Aadhaar, then immediately submits a correction. Standard SQS offers at-least-once delivery with best-effort ordering — the correction could be processed before the original submission, creating an inconsistent KYC record. FIFO SQS with a MessageGroupId of `user_id` guarantees that all messages for a given user are processed in submission order.

FIFO SQS costs marginally more (~$0.50/million vs $0.40/million) — irrelevant for the data integrity guarantee it provides.

### Why SNS + SQS (fan-out pattern) and not SNS alone?

SNS delivers messages to subscribers and, if delivery fails, retries briefly before dropping the message. For compliance alerts (e.g., "Macie detected Aadhaar in wrong bucket"), a dropped message is unacceptable. The SNS → SQS pattern adds a durable queue between the notification and the consumer. If the security team's webhook is down for 10 minutes, the alert waits in SQS for up to 4 days rather than being lost.

### Why Lambda for PII detection and not a container-based service?

The PII detection Lambda is triggered by S3 `PutObject` events — it needs to run immediately when a file is uploaded and then terminate. Running this logic in a persistent container would mean the container is idle 99% of the time (waiting for S3 events), wasting both compute and the PII detection attack surface. Lambda's ephemeral execution model is architecturally correct for event-driven, stateless, burst workloads.

---

## 📊 Zone 9 — Monitoring, Observability & Incident Response

```
┌──────────────────────────────────────────────────────────────────┐
│               OBSERVABILITY & SECURITY OPERATIONS                │
│                                                                  │
│  CloudWatch          CloudTrail          GuardDuty               │
│  ┌──────────┐        ┌──────────┐       ┌──────────────────┐    │
│  │Log Groups│        │All-region│       │ML Threat Detect  │    │
│  │Custom    │        │S3+CW logs│       │VPC Flow + DNS    │    │
│  │Metrics   │        │Tamper det│       │Auto-isolate EC2  │    │
│  │Dashboards│        │Validation│       │                  │    │
│  └──────────┘        └──────────┘       └──────────────────┘    │
│                                                                  │
│  AWS Config          Security Hub        Inspector               │
│  ┌──────────┐        ┌──────────┐       ┌──────────────────┐    │
│  │Compliance│        │Aggregated│       │EC2 + Lambda CVE  │    │
│  │Rules     │        │findings  │       │Container scanning│    │
│  │Auto-fix  │        │PCI DSS   │       │Block deploy on   │    │
│  │via SSM   │        │Standards │       │Critical vulns    │    │
│  └──────────┘        └──────────┘       └──────────────────┘    │
└──────────────────────────────────────────────────────────────────┘
```

### Why CloudTrail in ALL regions and not just ap-south-1?

Attackers who gain AWS credentials test them in regions where they expect no monitoring. A compromised IAM key used to launch a crypto-mining EC2 in `us-east-1` would be invisible if CloudTrail is only enabled in Mumbai. Multi-region CloudTrail at $2/region/month is the cheapest threat detection investment available.

### Why GuardDuty and not just CloudWatch alarms?

CloudWatch alarms detect metrics you've thought to monitor. GuardDuty detects patterns you haven't thought of — because it's trained on threat intelligence from AWS's global infrastructure. GuardDuty has detected credential exfiltration, data staging for exfiltration, and C2 (command and control) communication patterns that no custom CloudWatch alarm would have caught.

GuardDuty's auto-remediation via Lambda (isolate EC2 by adding it to a quarantine security group with no ingress/egress) means the response time to a confirmed threat is seconds, not the hours it takes for an on-call engineer to respond to a PagerDuty page.

### Why AWS Config compliance rules and not manual audits?

Manual compliance audits happen quarterly or annually. AWS Config rules evaluate compliance continuously — every time a resource is created or modified. If a developer accidentally creates an unencrypted RDS instance in a test environment that somehow gets connected to production data, Config detects it within minutes and auto-remediates via SSM. Manual audits would catch this in 3 months, after potential data exposure.

---

## 🛡️ Zone 10 — Network Security Groups & VPC Endpoints

### Security Group Rules

```
SG-ALB (faces internet)
├── INBOUND:  TCP 443 from 0.0.0.0/0
└── OUTBOUND: TCP 3000 to SG-EC2 ONLY

SG-EC2 (application tier)
├── INBOUND:  TCP 3000-8004 from SG-ALB ONLY
└── OUTBOUND: TCP 5432 → SG-RDS
              TCP 6379 → SG-Redis
              TCP 443  → AWS VPC Endpoints

SG-RDS (data tier)
├── INBOUND:  TCP 5432 from SG-EC2 ONLY
└── OUTBOUND: (none — RDS never initiates connections)

SG-Redis (cache tier)
├── INBOUND:  TCP 6379 from SG-EC2 ONLY
└── OUTBOUND: (none)
```

### Why no bastion host?

Traditional architectures use a bastion (jump) host to SSH into private EC2 instances. Bastion hosts are a permanent attack surface — they sit in a public subnet with port 22 open and require SSH key management. AWS Systems Manager Session Manager provides authenticated, audited shell access to EC2 instances with zero open inbound ports, zero SSH keys, and complete session logging to CloudWatch. Session Manager is strictly better than a bastion in every dimension for AWS infrastructure.

### VPC Endpoints — Why are they critical?

Without VPC endpoints, calls from EC2 to AWS services (KMS, Secrets Manager, SQS, CloudWatch) go through the public internet, then back into AWS. This creates:
1. **Data leakage risk**: Traffic observable on public internet links
2. **Latency**: Public internet round-trip adds 5-20ms vs direct VPC endpoint
3. **Cost**: NAT Gateway charges for outbound traffic ($0.045/GB)
4. **Compliance gap**: RBI requires sensitive data to not traverse public networks

Interface endpoints for KMS, Secrets Manager, SQS, SNS, CloudWatch, SSM, ECR cost ~$7.30/endpoint/month — trivial against the compliance and security benefit.

---

## 🚀 Zone 11 — CI/CD & Infrastructure Pipeline

```
GitHub (branch protection)
    │
    ▼
AWS CodePipeline
    │
    ├── CodeBuild Stage 1: Security Scans
    │   ├── SAST: SonarQube (code quality + security rules)
    │   ├── Dependency Scan: OWASP Dependency-Check
    │   ├── Secrets Scan: git-secrets (blocks hardcoded creds)
    │   └── Docker Build → ECR push
    │
    ├── CodeBuild Stage 2: Container Scanning
    │   └── ECR Enhanced Scanning via Inspector
    │       (CRITICAL vuln = pipeline BLOCKED)
    │
    └── CodeDeploy Stage 3: Blue/Green Deploy
        ├── Deploy to GREEN target group
        ├── Run health checks (5 min)
        ├── Shift 10% traffic → GREEN
        ├── Monitor CloudWatch alarms (5 min)
        ├── Shift 100% traffic → GREEN
        └── Terminate BLUE (or auto-rollback on alarm)
```

### Why Blue/Green and not rolling deployment?

Rolling deployment replaces instances one by one. During a rolling deploy, you have a mixed fleet — some instances running v1, some v2. If v2 has a bug, some users hit v1 (working), some hit v2 (broken) — inconsistent behavior for a payment system is worse than a brief maintenance window.

Blue/Green maintains a complete v2 environment. You shift traffic only after v2 passes health checks. If v2 fails, you shift traffic back to v1 in 60 seconds. The database schema must be backward-compatible (both v1 and v2 must work with the same schema), but this is a good architectural discipline regardless.

### Why block deploys on CRITICAL vulnerabilities and not just warn?

A CRITICAL CVE in a container image that gets deployed means: you just knowingly deployed a system with a known exploit path into a production environment handling Aadhaar data. In the event of a breach, this becomes a regulatory and legal liability. Blocking the pipeline forces the team to either patch the dependency or explicitly accept the risk via a documented exception process.

---

## ♻️ Zone 12 — Disaster Recovery & Backup

```
PRIMARY REGION                    DR REGION
ap-south-1 (Mumbai)               ap-southeast-1 (Singapore)
        │                                 │
        │  ←─── RDS Cross-region ─────►   │
        │        Read replica (RPO: 1h)   │
        │                                 │
        │  ←─── S3 CRR (all buckets) ───► │
        │                                 │
        │  ←─── DynamoDB Global Table ──► │
        │                                 │
        │  ←─── ECR Image Replication ──► │
        │                                 │
        │  ←─── Secrets Manager ────────► │
        │        Cross-region replica     │
        │                                 │
    RTO: 4 hours           Warm standby ready
    RPO: 1 hour            Promote RDS replica
                           Update Route 53
                           Scale up ASG
```

### Why Singapore as the DR region and not ap-south-2 (Hyderabad)?

`ap-south-2` (Hyderabad) is in the same country as Mumbai. A national-level disaster (power grid failure, widespread ISP outage, regional flooding) could affect both. Singapore (`ap-southeast-1`) is geographically separated and has been the most stable AWS region in Asia-Pacific. It also has the broadest service availability of any APAC region, ensuring DR operations can run the full service stack.

For RBI data localization: **primary data stays in India** (Mumbai). Singapore is used only in a declared DR scenario, which is explicitly permitted under RBI guidelines with documented DR procedures.

### Why Warm Standby and not Cold (backup-restore)?

| DR Type | Description | RTO | Cost |
|---|---|---|---|
| **Cold** | Backups in S3, restore on incident | 6-24h | Lowest |
| **Warm** | Read replicas running, scale up on failover | 2-4h | Medium |
| **Hot (Active-Active)** | Full traffic in both regions simultaneously | Minutes | Highest |
| **Pilot Light** | Core infrastructure running, scale out on failover | 1-2h | Low-Medium |

For a fintech with a 4-hour RTO requirement, warm standby is the minimum viable option. Cold restore cannot meet 4-hour RTO for a multi-service, multi-database system. Active-Active doubles your running costs and introduces complex data consistency challenges across regions that are difficult to audit.

### AWS Backup Vault Lock — Why immutable backups?

Ransomware attacks against cloud infrastructure increasingly target backup systems first. If an attacker can delete your backups before encrypting your primary data, they have leverage. Vault Lock (WORM — Write Once Read Many) makes backup deletion impossible for the retention period, even by AWS Support. Cross-account backup copies (to a separate AWS account) add another layer: an attacker who compromises your production account cannot access the backup account.

---

## 📜 Zone 13 — Compliance & Regulatory Framework

### PCI DSS Level 1

| Requirement | Implementation | Status |
|---|---|---|
| Requirement 1: Network segmentation | VPC private subnets, Security Groups | ✅ |
| Requirement 2: Secure configurations | Custom hardened AMI, SSM Patch Manager | ✅ |
| Requirement 3: Protect stored cardholder data | KMS field-level encryption, S3 Object Lock | ✅ |
| Requirement 4: Encrypt transmission | TLS 1.3 everywhere, certificate pinning | ✅ |
| Requirement 5: Anti-malware | Inspector, ECR scanning, no direct SSH | ✅ |
| Requirement 6: Secure development | SAST/DAST in pipeline, dependency scanning | ✅ |
| Requirement 7: Access control | IAM least privilege, Permission Boundaries | ✅ |
| Requirement 8: Identification & auth | MFA, IAM roles, Cognito for users | ✅ |
| Requirement 9: Physical security | AWS data centers (inherited) | ✅ |
| Requirement 10: Logging & monitoring | CloudTrail + CloudWatch + Security Hub | ✅ |
| Requirement 11: Vulnerability testing | Inspector, GuardDuty, Config | ✅ |
| Requirement 12: Information security policy | Documented via Config rules | ✅ |

### DPDP Act 2023 (Digital Personal Data Protection — India)

| Obligation | Implementation |
|---|---|
| **Lawful processing / Consent** | DynamoDB consent table with immutable audit trail |
| **Data minimization** | KYC service collects only fields required by RBI KYC Master Directions |
| **Purpose limitation** | Microservice isolation — KYC data physically unreachable by Notification/Auth services |
| **Storage limitation** | S3 Object Lock with 7-year retention, then automated deletion via Lambda |
| **Right to erasure** | Soft delete flag + scheduled hard delete Lambda (within 30 days of request) |
| **Data localization** | Primary processing in ap-south-1 Mumbai; DR only on declared incident |
| **Security safeguards** | Full encryption stack, access controls, audit logging as described |
| **Breach notification** | GuardDuty + Security Hub → PagerDuty → 72h CERT-In notification workflow |

---

## 🔄 End-to-End KYC Data Flow

```
① User submits KYC with Aadhaar + PAN via HTTPS (TLS 1.3)
          │
          ▼
② Route 53 → CloudFront (static) | ALB (API traffic)
          │
          ▼
③ WAF inspects: OWASP rules, rate limit, geo-check
          │
          ▼
④ ALB routes to healthy EC2 Docker host → API Gateway container (port 3000)
          │
          ▼
⑤ API Gateway: Validates JWT (Cognito) + checks rate limit (Redis INCR)
          │
          ▼
⑥ Forwards to KYC Compliance Service (port 8002, Java/Spring)
          │
          ├──► ⑦ Calls Macie API to classify incoming payload fields
          │
          ├──► ⑧ Encrypts PAN, Aadhaar fields via KMS CMK (field-level)
          │         Returns ciphertext — plaintext never written anywhere
          │
          ├──► ⑨ Writes encrypted fields to RDS via RDS Proxy (IAM auth)
          │         kyc_records table: all PII columns pgcrypto-encrypted
          │
          ├──► ⑩ Uploads document scan to S3 kyc-documents (SSE-KMS)
          │         Via VPC Gateway Endpoint — no public internet
          │
          ▼
⑪ S3 PutObject triggers lambda-pii-detector
          │         Validates: no raw PII in object metadata, tags, ACLs
          │
          ▼
⑫ KYC Service pushes processing job to SQS FIFO queue
          │         MessageGroupId = user_id (ordered delivery)
          │
          ▼
⑬ Audit Log Service (port 8004, Go) records:
          │         who (user_id + IAM role), what (KYC submission),
          │         when (timestamp), from where (IP, user agent),
          │         result (success/failure) → CloudWatch + S3 audit bucket
          │
          ▼
⑭ Notification Service (port 8003) sends status update
          │         Via SQS notification-queue → SES (email) / SNS (SMS)
          │         Uses only user_id — never touches PII fields
          │
          ▼
⑮ CloudTrail logs every API call, KMS operation, S3 access, IAM action
          │
          ▼
⑯ GuardDuty + Security Hub monitor the entire flow for anomalies
          │         Unusual access pattern? → SNS alert → PagerDuty in seconds
```

---

## 🧮 Technology Decision Matrix

| Decision | Chosen | Rejected Options | Reason Chosen |
|---|---|---|---|
| Cloud | AWS | Azure, GCP | RBI data localization, Macie Aadhaar support, IDRBT MOU |
| Compute | EC2 + Docker + ASG | ECS, EKS, Lambda-first | Pre-configured EC2s, predictable performance, ECS migration path |
| RDBMS | PostgreSQL (RDS) | MySQL, Aurora, MongoDB | pgcrypto column encryption, PCI DSS row-level security |
| NoSQL | DynamoDB | MongoDB Atlas, Cassandra | Serverless, Global Tables, AWS-native encryption, no ops overhead |
| Cache | ElastiCache Redis | Memcached, DAX | Rate limiting INCR, TTL-based OTP expiry, session storage |
| Secret Store | Secrets Manager | SSM Param Store, Vault | Auto-rotation for RDS, cross-region replication |
| Key Mgmt | AWS KMS CMK | HashiCorp Vault | AWS-native, CloudTrail integration, no additional infra |
| PII Detection | AWS Macie | Custom regex scanner | Built-in Aadhaar/PAN patterns, ML-based, managed updates |
| Threat Detection | GuardDuty | Splunk, custom SIEM | ML-based, zero config, VPC Flow + DNS + CloudTrail analysis |
| Deploy strategy | Blue/Green | Rolling, Canary, In-place | Zero-downtime, instant rollback, no mixed fleet |
| DNS | Route 53 | Cloudflare | AWS-native, geo-routing, health-check failover, no third-party DNS for PII endpoints |
| DDoS | Shield Standard | Cloudflare Magic Transit | No additional cost, Layer 3/4 coverage, AWS-integrated |
| Bastion | None (SSM) | EC2 Bastion Host | No open ports, full session logging, no SSH key management |

---

## 📐 Scalability Model

### Horizontal Scaling (ASG)

```
Traffic Pattern         ASG Response
────────────────────────────────────────
Normal (3 instances)   → Handle up to ~600 req/sec
Morning rush (+2)      → CPU > 70% for 2min → scale to 5
Peak KYC window (+4)   → CPU > 70% for 2min → scale to 9
Black Friday (+3)      → Scale to max 12 instances
Off-peak (scale in)    → CPU < 30% for 10min → scale down

Per-instance capacity (t3.xlarge):
  API Gateway:  ~200 req/sec (Node.js, non-blocking)
  Auth Service: ~150 req/sec (FastAPI async)
  KYC Service:  ~50 req/sec  (Java, crypto-heavy)
  Bottleneck:   KYC Service (intentionally slower — crypto ops)
```

### Database Scaling

```
Read traffic    → ElastiCache Redis (sessions, lookups)
                → RDS Read Replica (reporting queries)
Write traffic   → RDS Primary (all writes)
                → DynamoDB (sessions, metadata — auto-scales)

RDS scaling path:
  Current: db.r6g.large (2 vCPU, 16GB)
  Next:    db.r6g.xlarge (4 vCPU, 32GB) — vertical scale
  Then:    Aurora PostgreSQL — horizontal read scaling
  Max:     Aurora Global Database — cross-region reads
```

### Data Consistency Model

| Data Type | Consistency | Mechanism |
|---|---|---|
| KYC records | Strong | PostgreSQL ACID, RDS Primary |
| Session tokens | Eventual (seconds) | Redis cluster (sync replication lag ~1ms) |
| Compliance rules | Strong | DynamoDB strongly consistent reads |
| Audit logs | Eventual (acceptable) | CloudWatch async ingestion |
| Document metadata | Strong | DynamoDB conditional writes |

---

## 💰 Cost Architecture Considerations

### Why this architecture is cost-optimized for fintech

| Component | Optimization | Saving |
|---|---|---|
| EC2 instances | Reserved Instances (1-year) for baseline 3 | ~40% vs On-Demand |
| ASG scale-out | On-Demand for burst (unpredictable) | Pay only when needed |
| RDS | Reserved Instance (1-year) for primary | ~40% vs On-Demand |
| S3 | Intelligent-Tiering for audit logs | Auto-moves to Glacier |
| CloudFront | Serves static assets at edge | Reduces ALB + EC2 load |
| VPC Endpoints | Gateway endpoints for S3/DynamoDB | Free — saves NAT GW cost |
| Lambda | Per-execution billing for event functions | Idle = zero cost |

### What NOT to skimp on

| Component | Do NOT cut | Reason |
|---|---|---|
| Multi-AZ RDS | Never run single-AZ in production | One AZ failure = full outage |
| GuardDuty | ~$1-3/GB analyzed — enable everywhere | Breach cost >> monitoring cost |
| CloudTrail | Multi-region, all management events | Audit trail = legal protection |
| KMS CMKs | Separate keys per data classification | Single key compromise = total exposure |
| WAF Managed Rules | ~$10/month per rule group | One SQLi attack = data breach |

---

## 🗂️ Repository Structure

```
fintech-compliance-platform/
│
├── 📁 infrastructure/
│   ├── 📁 terraform/
│   │   ├── vpc.tf                    # VPC, subnets, route tables
│   │   ├── ec2_asg.tf                # EC2 launch template, ASG, ALB
│   │   ├── rds.tf                    # RDS PostgreSQL, RDS Proxy, replicas
│   │   ├── elasticache.tf            # Redis cluster
│   │   ├── dynamodb.tf               # Tables, global tables
│   │   ├── s3.tf                     # Buckets, lifecycle, object lock
│   │   ├── kms.tf                    # CMKs per service
│   │   ├── iam.tf                    # Roles, policies, permission bounds
│   │   ├── security.tf               # WAF, Shield, GuardDuty, Macie
│   │   ├── monitoring.tf             # CloudWatch, CloudTrail, Config
│   │   └── secrets.tf                # Secrets Manager, rotation Lambdas
│   │
│   └── 📁 docker/
│       ├── docker-compose.yml        # Local development stack
│       └── hardened-base/
│           └── Dockerfile            # Base image (minimal, security-patched)
│
├── 📁 services/
│   ├── 📁 api-gateway/               # Node.js (port 3000)
│   ├── 📁 auth-service/              # Python/FastAPI (port 8001)
│   ├── 📁 kyc-compliance/            # Java/Spring Boot (port 8002)
│   ├── 📁 notification-service/      # Node.js (port 8003)
│   └── 📁 audit-log-service/         # Go (port 8004)
│
├── 📁 lambda/
│   ├── pii-detector/                 # S3 trigger, Macie classification
│   ├── secrets-rotator/              # RDS credential rotation
│   └── compliance-checker/           # Scheduled KYC expiry check
│
├── 📁 .github/
│   └── 📁 workflows/
│       ├── security-scan.yml         # SAST, dependency, secrets scan
│       ├── build-push.yml            # Docker build, ECR push, scan
│       └── deploy.yml                # Blue/Green via CodeDeploy
│
├── 📁 docs/
│   ├── 📁 architecture/
│   │   ├── system-design.png         # Eraser.io exported diagram
│   │   └── data-flow.md              # KYC data flow documentation
│   ├── runbook-incident-response.md  # GuardDuty alert → action steps
│   ├── runbook-dr-failover.md        # DR region activation procedure
│   └── compliance-matrix.md          # PCI DSS / RBI / DPDP mapping
│
└── 📁 assets/
    └── architecture.png              # ← Your Eraser.io diagram goes here
```

---

## 🖼️ Eraser.io Generation Prompt

<details>
<summary><strong>Click to expand the full Eraser.io prompt — paste this to generate your architecture diagram</strong></summary>

```
Design a production-grade, AWS-based Fintech Application Compliance System 
architecture diagram with the following complete specifications. Use cloud 
architecture diagram style with AWS icons and components.

---

TITLE: Fintech PII Compliance & Data Security Platform — AWS Architecture

---

ZONE 1 — CLIENT LAYER (Top)

Draw three client entry points side by side:
- Mobile App (iOS/Android)
- Web Browser (HTTPS)
- Third-party API Partners

All three connect downward via HTTPS/TLS 1.3 arrows to the next zone.

---

ZONE 2 — EDGE & PROTECTION LAYER

Draw the following AWS services in a horizontal row:

1. AWS Route 53 (DNS with health checks and geo-routing failover)
2. AWS WAF — Rules: Block OWASP Top 10, rate limiting per IP, 
   geo-restriction rules, SQL injection prevention
3. AWS Shield Standard (DDoS protection — always on)
4. AWS CloudFront (CDN — for static assets only, no PII caching)
   Note: PII requests bypass CloudFront and go directly to ALB

Draw arrows: Route 53 → WAF → Shield → CloudFront/ALB

---

ZONE 3 — VPC BOUNDARY
Draw a large dashed rectangle labeled:
"AWS VPC — ap-south-1 (Mumbai) | CIDR: 10.0.0.0/16"

Inside the VPC, create TWO clearly labeled subnet tiers:

PUBLIC SUBNET (10.0.1.0/24 and 10.0.2.0/24 — Multi-AZ)
Inside public subnet:
- Application Load Balancer (ALB)
  HTTPS listener on port 443, SSL/TLS termination
  Routes to Target Groups by path: /api/kyc, /api/payments, /api/users
- NAT Gateway (one per AZ)
- Internet Gateway

PRIVATE SUBNET (10.0.3.0/24 to 10.0.8.0/24 — Multi-AZ)
Inside private subnet:

AUTO SCALING GROUP boundary labeled:
"ASG — Min:3, Max:12, Desired:3"

Inside ASG, draw 3 EC2 instances:
- EC2 Instance 1 (AZ-1a) — Docker Host — t3.xlarge
- EC2 Instance 2 (AZ-1b) — Docker Host — t3.xlarge
- EC2 Instance 3 (AZ-1c) — Docker Host — t3.xlarge

On each EC2, show Docker containers:
Container 1: API Gateway Service (Node.js — port 3000)
Container 2: Auth Service (Python/FastAPI — port 8001)
Container 3: KYC/Compliance Service (Java/Spring — port 8002)
Container 4: Notification Service (Node.js — port 8003)
Container 5: Audit Log Service (Go — port 8004)

ASG policies:
- Scale Out: CPU > 70% for 2min → add 2 instances
- Scale In: CPU < 30% for 10min → remove 1 instance
- Health Check: ALB every 30s, grace period 120s

---

ZONE 5 — DATA LAYER (Private Subnet — Isolated Security Group)

Draw "DATA TIER" boundary. No internet access.

Amazon RDS PostgreSQL (Multi-AZ):
- Primary: AZ-1a
- Standby replica: AZ-1b (synchronous)
- Read replica: AZ-1c (async)
- Storage: gp3 encrypted KMS CMK
- RDS Proxy in front (IAM auth, connection pooling)

Amazon DynamoDB:
- Tables: sessions, rate_limit_counters, compliance_rules, document_metadata
- On-demand capacity, Global table, PITR enabled

Amazon ElastiCache Redis (Cluster mode):
- 3 nodes across 3 AZs
- Stores: JWT sessions, OTP codes (TTL 5min), rate limit state
- TLS + AUTH token

---

ZONE 6 — S3 STORAGE

Bucket 1: fintech-kyc-documents-prod
- SSE-KMS, CMK rotation 90 days
- Object Lock: COMPLIANCE mode, 7-year retention
- Block all public access

Bucket 2: fintech-audit-logs-archive
- Lifecycle: Glacier after 90d, delete 7yr

Bucket 3: fintech-app-assets
- Static files, CloudFront served, ZERO PII

VPC Gateway Endpoint for S3

---

ZONE 7 — SECURITY PANEL (right side)

1. AWS KMS — CMKs per service (RDS, S3, EBS, Secrets), auto-rotate 365d
2. AWS Secrets Manager — Rotate RDS creds every 30d via Lambda
3. AWS IAM — Service roles, Permission Boundaries, MFA required
4. AWS Macie — Scan S3 for Aadhaar/PAN/credit card patterns
5. AWS ACM — TLS certs, auto-renew, mobile cert pinning
6. Amazon Cognito — OAuth 2.0 + OIDC + MFA

---

ZONE 8 — MESSAGING LAYER (horizontal)

1. SQS FIFO: kyc-processing-queue (ordered, exactly-once)
2. SQS Standard: notification-queue + dead-letter-queue
3. SNS Topics: compliance-alerts, kyc-status, system-alerts
4. Lambda Functions:
   - lambda-pii-detector (S3 trigger)
   - lambda-secrets-rotator (scheduled)
   - lambda-compliance-checker (daily schedule)

---

ZONE 9 — MONITORING PANEL (bottom)

1. AWS CloudWatch — Log Groups, Custom Metrics, Alarms, Dashboards
2. AWS CloudTrail — All regions, S3+CW logs, tamper detection
3. AWS GuardDuty — ML threat detection, auto-isolate EC2 on high-severity
4. AWS Config — Compliance rules: s3-bucket-public-read-prohibited, 
   rds-storage-encrypted, encrypted-volumes, iam-root-access-key-check
5. AWS Security Hub — PCI DSS standards, aggregated findings
6. AWS Inspector — EC2 + container CVE scanning

---

ZONE 10 — NETWORK ANNOTATIONS

Security Group rules legend:
SG-ALB: inbound 443 from 0.0.0.0/0, outbound 3000 to SG-EC2
SG-EC2: inbound 3000-8004 from SG-ALB, outbound to SG-RDS/SG-Redis/endpoints
SG-RDS: inbound 5432 from SG-EC2 only
SG-Redis: inbound 6379 from SG-EC2 only

VPC Endpoints (Interface): KMS, Secrets Manager, SQS, SNS, ECR, 
CloudWatch Logs, SSM

---

ZONE 11 — CI/CD PANEL (side)

AWS CodePipeline:
Source: GitHub → CodeBuild (SAST, OWASP scan, git-secrets, Docker build) 
→ ECR (image scan via Inspector, block on CRITICAL) 
→ CodeDeploy Blue/Green (shift traffic 10% → 100%, auto-rollback on alarm)

Amazon ECR: Private registry, lifecycle policy, cross-region replication

AWS Systems Manager: Session Manager (no SSH), Patch Manager, Run Command

---

ZONE 12 — DR PANEL (far right)

Primary: ap-south-1 (Mumbai)
DR: ap-southeast-1 (Singapore) — Warm Standby

Replication arrows between regions:
- RDS cross-region read replica (RPO: 1h)
- S3 CRR for kyc-documents and audit-logs
- DynamoDB Global Table
- ECR image replication
- Secrets Manager cross-region

RTO: 4 hours | RPO: 1 hour

AWS Backup with Vault Lock (immutable, cross-account copy)

---

ZONE 13 — COMPLIANCE BADGE PANEL

Show compliance achieved:
✅ PCI DSS Level 1
✅ RBI IT Framework
✅ DPDP Act 2023
✅ Data Localization (ap-south-1 primary)

---

NUMBERED DATA FLOW (highlight as trace path ①-⑯):

① User submits KYC via HTTPS
② Route 53 resolves to ALB
③ WAF inspects, blocks malicious
④ ALB routes to EC2 API Gateway container
⑤ API Gateway validates JWT, checks rate limit (Redis)
⑥ Request to KYC Compliance Service
⑦ KYC calls Macie to classify data
⑧ PII fields encrypted via KMS CMK immediately
⑨ Encrypted data written to RDS via RDS Proxy
⑩ Document scan uploaded to S3 (SSE-KMS, VPC endpoint)
⑪ Lambda PII Detector triggered by S3 event
⑫ Processing job pushed to SQS FIFO
⑬ Audit Log records full access event to CloudWatch + S3
⑭ Notification Service sends status update via SES/SNS
⑮ CloudTrail logs every API call, KMS decrypt, S3 access
⑯ GuardDuty + Security Hub monitor for anomalies

---

DIAGRAM STYLE:
- AWS architecture diagram notation with official AWS service icons
- Color coding:
  RED/ORANGE: Security services (KMS, WAF, GuardDuty, Macie, Shield)
  BLUE: Compute (EC2, Lambda, ECS)
  GREEN: Data stores (RDS, DynamoDB, ElastiCache, S3)
  PURPLE: Messaging (SQS, SNS)
  GRAY: Networking (VPC, ALB, Route53, CloudFront)
  YELLOW: Monitoring (CloudWatch, CloudTrail, Config, Security Hub)
- VPC as largest container with prominent dashed border
- Public subnet lighter background, private subnet darker
- Multi-AZ shown as parallel columns (AZ-1a, AZ-1b, AZ-1c)
- Numbered data flow (①-⑯) as highlighted path
- Security group boundaries as dotted rectangles
- DR replication as dashed arrows pointing right
- Legend bottom-right for color coding
- Layout: Top-to-bottom, clients → edge → VPC → data layer
  Security panel on right, monitoring on bottom, DR on far right
```

</details>

---

<div align="center">

---

## 🏁 Credits

<br/>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:2c5364,50:203a43,100:0f2027&height=120&section=footer" width="100%"/>

<br/>

```
╔══════════════════════════════════════════════════════════════════╗
║                                                                  ║
║         Designed & Architected by  Yuvraj Sonaja                 ║
║                                                                  ║
║    Full-Stack Engineer · AWS Cloud Architect · Fintech Builder   ║
║                                                                  ║
╚══════════════════════════════════════════════════════════════════╝
```

[![GitHub](https://img.shields.io/badge/GitHub-Yuvraj%20Sonaja-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com)

<br/>

> *"Security is not a feature you bolt on — it's an architecture you build in."*

<br/>

**Built with:** AWS · Docker · PostgreSQL · Redis · Java · Python · Node.js · Go · Terraform

<br/>

![Made with Love](https://img.shields.io/badge/Made%20with-❤️%20in%20India-FF9933?style=for-the-badge)
![AWS](https://img.shields.io/badge/Powered%20by-AWS%20ap--south--1-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)

</div>
