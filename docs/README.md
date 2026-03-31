# SAP on Azure – Resiliency and Disaster Recovery Architecture

This repository presents a progressive, architecture-focused design for running **SAP ECC / SAP S/4HANA on Microsoft Azure IaaS**, evolving from a baseline deployment to **in‑region high availability** and **multi‑region disaster recovery**.

The goal of this project is to demonstrate **realistic, SAP-supported architectures** used by enterprise customers — not lab deployments or installation guides.

---

## Architecture Scope

- SAP ECC / SAP S/4HANA on Azure Virtual Machines
- SAP HANA database on Azure IaaS
- High Availability within a single Azure region
- Disaster Recovery across multiple Azure regions
- Architecture and design decisions (not step-by-step builds)

---

## Architecture Evolution

### 1. Baseline SAP on Azure
**Diagram:** `diagrams/sap-baseline-architecture.png`

This represents the normal operating state of SAP running in a single Azure region:

- SAP Application Servers on Azure VMs
- Azure Load Balancer in front of the application layer
- SAP HANA primary database
- Single Azure region
- No high availability or disaster recovery

This baseline is the foundation for all resiliency improvements.

---

### 2. In‑Region High Availability (HA)
**Diagram:** `diagrams/sap-in-region-resiliency.png`

This stage introduces **high availability within the same Azure region**:

- SAP Application Servers distributed across Availability Zones
- SAP HANA deployed in a Primary / Secondary configuration
- Synchronous replication between HANA nodes
- Protection against VM, host, and Availability Zone failures

This architecture improves availability but **does not protect against a regional outage**.

---

### 3. Multi‑Region Disaster Recovery (DR)
**Diagram:** `diagrams/sap-multi-region-dr.png`

This stage introduces **disaster recovery across two Azure regions**:

- Primary Azure region hosting the active SAP workload
- Secondary Azure region hosting a warm standby environment
- SAP HANA System Replication between regions
- Active / Passive configuration
- Controlled, manual or semi‑automated failover

This design protects against **regional failures** and large‑scale outages.

---

## High Availability vs Disaster Recovery

| Aspect | High Availability | Disaster Recovery |
|------|------------------|------------------|
| Scope | Single Azure region | Multiple Azure regions |
| Goal | Minimize downtime | Recover from region loss |
| Replication | Synchronous | Typically asynchronous |
| Downtime | Seconds to minutes | Minutes to hours |

HA and DR address **different failure domains** and are complementary, not interchangeable.

---

## RTO / RPO (Typical Ranges)

- **RPO:** seconds to minutes  
- **RTO:** tens of minutes to a few hours  

Actual values depend on:
- Replication mode
- Automation level
- Operational procedures

Non‑zero RTO/RPO is common and acceptable for enterprise SAP workloads.

---

## Why Active / Passive

- SAP HANA does not support multi‑region active/active
- Data consistency is critical for SAP workloads
- Active / Passive is:
  - SAP-supported
  - Operationally predictable
  - Easier to test and audit

---

## Why IaaS for SAP

SAP requires:
- OS-level control
- Certified VM types
- Database-level replication mechanisms

For these reasons, SAP on Azure is deployed on **Infrastructure as a Service (IaaS)** rather than PaaS offerings.

---
---

## Architecture Deep Dives

The following documents provide deeper insight into key design decisions behind this architecture:

- **Cost Considerations**  
  [`docs/cost-considerations.md`](docs/cost-considerations.md)  
  Explains the main cost drivers for SAP Disaster Recovery on Azure and why Active / Passive is the most common approach.

- **Workload Tiering**  
  [`docs/workload-tiering.md`](docs/workload-tiering.md)  
  Describes Tier 0, Tier 1, and Tier 2 SAP systems and how DR investment is aligned with business criticality.

- **Azure Site Recovery Positioning**
  docs/azure-site-recovery-positioning.md  
  Explains where Azure Site Recovery fits in SAP architectures and why SAP HANA relies on database-native replication instead of VM-level DR.

## Out of Scope (for now)

- ASCS / ERS deep clustering configuration
- Deployment automation or scripts
- Detailed on‑premises hybrid integration  

