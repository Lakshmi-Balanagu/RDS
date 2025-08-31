

#  **Production-Grade RDS & Aurora Setup Guide**

---

## 🔹 Index / Quick Links

* [Aurora MySQL (Regional Cluster)](#aurora-mysql)
* [Aurora PostgreSQL (Regional Cluster)](#aurora-postgresql)
* [Aurora Global Database (Multi-Region)](#aurora-global)
* [RDS for MySQL (Multi-AZ Cluster)](#rds-mysql)
* [RDS for PostgreSQL (Multi-AZ Cluster)](#rds-postgresql)
* [Baseline Parameter Groups](#parameter-groups)

---

<a name="aurora-mysql"></a>

## ✅ Aurora MySQL – Regional Cluster (Production)

| Step | Action                                                                        | Why it Matters                                           |
| ---- | ----------------------------------------------------------------------------- | -------------------------------------------------------- |
| 1    | RDS Console → **Create DB** → Engine: Aurora MySQL → Template: **Production** | Ensures best defaults (backups, monitoring, durability). |
| 2    | Capacity Type: **Provisioned** or **Serverless v2**                           | Provisioned = predictable, Serverless = elastic.         |
| 3    | Multi-AZ: 1 Writer + 2 Readers across AZs                                     | HA + read scaling.                                       |
| 4    | Credentials in **Secrets Manager**                                            | Secure rotation.                                         |
| 5    | Storage: Always encrypted with **KMS CMK**                                    | Compliance + DR readiness.                               |
| 6    | VPC/Subnet Group (private), SG (app-only access)                              | Network security best practice.                          |
| 7    | Monitoring: PI (31d), Enhanced Monitoring (1s), Log exports                   | Deep visibility into DB load.                            |
| 8    | Backups: 7–30d + PITR                                                         | Meet RPO/RTO targets.                                    |
| 9    | Deletion Protection: **ON**                                                   | Prevents accidental deletes.                             |

---

<a name="aurora-postgresql"></a>

## ✅ Aurora PostgreSQL – Regional Cluster (Production)

| Step | Action                                                            | Why it Matters                     |
| ---- | ----------------------------------------------------------------- | ---------------------------------- |
| 1    | Engine: Aurora PostgreSQL                                         | PostgreSQL extensions support.     |
| 2    | Multi-AZ: 1 Writer + 2 Readers                                    | HA + reporting queries.            |
| 3    | Extensions: `pg_stat_statements`, `uuid-ossp`, `pgcrypto`         | Performance + security + UUIDs.    |
| 4    | Logs: export `postgresql` & `upgrade` logs                        | Visibility during upgrades/issues. |
| 5    | Parameters: tune `max_connections`, `work_mem`, `wal_compression` | Stability and performance.         |

---

<a name="aurora-global"></a>

## ✅ Aurora Global Database – Multi-Region

| Step | Action                                               | Why it Matters                |
| ---- | ---------------------------------------------------- | ----------------------------- |
| 1    | Create Aurora cluster in **Region A**                | Primary write region.         |
| 2    | RDS → Databases → Cluster → **Actions → Add Region** | Add Region B as secondary.    |
| 3    | Provide Subnet Group, SG, KMS Key (Region B)         | Ensures secure DR setup.      |
| 4    | Region B: Reader-only cluster                        | Low-latency reads near users. |
| 5    | Switchover (planned) / Failover (unplanned) tested   | Validates DR strategy.        |

---

<a name="rds-mysql"></a>

## ✅ RDS for MySQL – Multi-AZ DB Cluster

| Step | Action                                               | Why it Matters                                |
| ---- | ---------------------------------------------------- | --------------------------------------------- |
| 1    | Engine: MySQL → Deployment: **Multi-AZ DB Cluster**  | New HA model with multiple readable standbys. |
| 2    | Instance class: `db.r6g.large`+                      | Graviton for cost/perf balance.               |
| 3    | Storage: gp3 + autoscaling                           | Elastic and cost-efficient.                   |
| 4    | Monitoring: PI, Enhanced, log exports                | Early detection of bottlenecks.               |
| 5    | Option Group: enable audit / native backup if needed | Extra features.                               |
| 6    | Parameter Group: tune InnoDB settings                | Stable production perf.                       |

---

<a name="rds-postgresql"></a>

## ✅ RDS for PostgreSQL – Multi-AZ DB Cluster

| Step | Action                                                   | Why it Matters                       |
| ---- | -------------------------------------------------------- | ------------------------------------ |
| 1    | Engine: PostgreSQL → Deployment: **Multi-AZ DB Cluster** | High availability.                   |
| 2    | Extensions enabled post-create                           | Needed for UUIDs, analytics.         |
| 3    | Parameter Group tuned for caching, WAL, autovacuum       | Prevents bloat, improves throughput. |
| 4    | Monitoring: PI, Enhanced, log exports                    | Performance visibility.              |
| 5    | Backups: 7–30d + PITR                                    | Recovery readiness.                  |

---

<a name="parameter-groups"></a>

## ⚙️ Baseline Parameter Groups (Templates)

**Aurora MySQL (Cluster)**

```ini
binlog_format = ROW
binlog_row_image = FULL
character_set_server = utf8mb4
collation_server = utf8mb4_unicode_ci
innodb_flush_log_at_trx_commit = 1
innodb_lock_wait_timeout = 50
max_connections = 500
slow_query_log = 1
long_query_time = 2
```

**Aurora PostgreSQL (Cluster)**

```ini
max_connections = 500
shared_buffers = 25% of instance RAM
effective_cache_size = 75% of instance RAM
work_mem = 4MB
maintenance_work_mem = 256MB
wal_compression = on
max_wal_size = 2GB
autovacuum_vacuum_cost_limit = 2000
```

**RDS MySQL (DB Parameter Group)**

```ini
innodb_buffer_pool_size = 60% of instance RAM
innodb_flush_log_at_trx_commit = 1
max_connections = 400
log_bin = 1
binlog_format = ROW
table_open_cache = 2000
slow_query_log = 1
long_query_time = 2
```

**RDS PostgreSQL (DB Parameter Group)**

```ini
max_connections = 400
shared_buffers = 25% of RAM
effective_cache_size = 70% of RAM
work_mem = 4MB
maintenance_work_mem = 256MB
wal_compression = on
max_wal_size = 2GB
autovacuum_vacuum_cost_limit = 2000
```

---

# 📌 Final Checklist (Apply to all)

* [ ] DB Subnet Group across ≥2 AZs
* [ ] SG with least-privilege inbound
* [ ] Encrypted with KMS CMK
* [ ] Backups (≥7d retention, PITR enabled)
* [ ] Monitoring (PI + Enhanced + Logs → CloudWatch)
* [ ] Parameter Group tuned per engine
* [ ] Option Group applied (MySQL only, if needed)
* [ ] Maintenance window set (off-hours)
* [ ] Deletion protection ON
* [ ] Test PITR restore quarterly
* [ ] Test failover / switchover quarterly

