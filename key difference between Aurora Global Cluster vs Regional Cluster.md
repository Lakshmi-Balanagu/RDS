
# 🔹 Aurora **Regional Cluster**

* **Scope**: One AWS Region only.
* **Architecture**:

  * **1 Writer** (primary instance).
  * **0–15 Readers** (read replicas).
  * All instances share the same **storage volume** (highly available, distributed across 3 AZs in that region).
* **Use case**:

  * Standard production workload in one region.
  * HA within a single region (multi-AZ, automatic failover).
  * Most common deployment.

✅ **Think of it as your main DB cluster serving one region**.

---

# 🔹 Aurora **Global Cluster**

* **Scope**: Multi-Region deployment.
* **Architecture**:

  * Built on **a primary Regional Cluster** + **one or more secondary (read-only) clusters in different regions**.
  * Uses **Aurora Global Database replication**:

    * Storage is asynchronously replicated **at the storage layer** (latency \~ <1s).
    * Secondary clusters are **read-only** by default.
* **Failover**:

  * In case of disaster in primary region, you can **promote a secondary cluster to be the new primary**.
  * RPO < 1s, RTO \~ <1 min.
* **Use case**:

  * Global apps (serving users in multiple continents).
  * Disaster Recovery (DR) across regions.
  * Offloading reads to other regions (reduce latency for global users).

✅ **Think of it as: A Global Cluster = multiple Regional Clusters linked together.**

---

# 🔑 Main Differences (Side by Side)

| Feature               | Regional Cluster                      | Global Cluster                                                  |
| --------------------- | ------------------------------------- | --------------------------------------------------------------- |
| **Region Scope**      | Single Region                         | Multi-Region                                                    |
| **Replication**       | Shared storage across AZs             | Asynchronous storage-level replication across regions           |
| **Failover**          | Within region only (multi-AZ)         | Can promote another region to be new primary (DR)               |
| **Latency**           | Millisecond latency within region     | Cross-region replication lag (\~<1s)                            |
| **Readers**           | Up to 15 read replicas in same region | Local read replicas in each region (good for global apps)       |
| **Disaster Recovery** | Limited to 1 region                   | Multi-region DR (RPO < 1s, RTO < 1 min)                         |
| **Cost**              | Cheaper (regional only)               | More expensive (inter-region replication + multi-cluster infra) |

---

# 🔹 Quick Example

* **Regional Cluster**:

  * Aurora MySQL deployed in `us-east-1`.
  * Writer in `us-east-1a`, readers in `us-east-1b` and `us-east-1c`.
  * If `us-east-1a` fails → failover to another AZ in same region.

* **Global Cluster**:

  * Primary in `us-east-1`.
  * Secondary read-only clusters in `eu-west-1` and `ap-southeast-1`.
  * Users in Europe read locally from `eu-west-1`.
  * If entire `us-east-1` fails → promote `eu-west-1` cluster to be new global primary.

---

✅ **In short**:

* **Regional Cluster** = HA within **one region**.
* **Global Cluster** = HA + DR across **multiple regions**.

