

| Abbreviation / Term         | Full Form / Meaning                       | Why / Use in Production                                                                                                |
| --------------------------- | ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **RDS**                     | Relational Database Service               | AWS managed DB service for MySQL, Postgres, MariaDB, Oracle, SQL Server. Handles backups, HA, patching.                |
| **Aurora**                  | Amazon Aurora                             | High-performance, MySQL/Postgres-compatible DB from AWS with storage auto-scaling, multi-AZ HA, and Global DB options. |
| **DB**                      | Database                                  | Stores structured application data.                                                                                    |
| **AZ**                      | Availability Zone                         | Isolated data center in a region. Multi-AZ deployments ensure HA and failover within region.                           |
| **CDC**                     | Change Data Capture                       | Tracks changes in DB (insert/update/delete) to replicate or stream to other systems.                                   |
| **HA**                      | High Availability                         | Ensures DB uptime using multi-AZ or cluster replication.                                                               |
| **RPO**                     | Recovery Point Objective                  | Max tolerable data loss in disaster recovery. Aurora Global Cluster RPO < 1s.                                          |
| **RTO**                     | Recovery Time Objective                   | Max tolerable downtime during DR. Aurora Global Cluster RTO < 1 min.                                                   |
| **ACU**                     | Aurora Capacity Unit                      | Unit for Aurora Serverless v2 scaling (CPU + RAM + I/O).                                                               |
| **CPU**                     | Central Processing Unit                   | Determines compute performance of DB instances.                                                                        |
| **RAM**                     | Random Access Memory                      | Memory available for buffer pools, caching, query execution.                                                           |
| **ROW**                     | Row-Based Logging                         | `binlog_format=ROW` in MySQL/Aurora logs actual row changes for replication/CDC.                                       |
| **STATEMENT**               | Statement-Based Logging                   | Logs SQL statements (MySQL binlog format). Less reliable for CDC.                                                      |
| **DDL**                     | Data Definition Language                  | SQL commands that define schema (CREATE, ALTER, DROP).                                                                 |
| **DML**                     | Data Manipulation Language                | SQL commands to modify data (INSERT, UPDATE, DELETE).                                                                  |
| **DBA**                     | Database Administrator                    | Manages performance, tuning, backups, replication, and DR strategy.                                                    |
| **JSON**                    | JavaScript Object Notation                | Semi-structured data format; sometimes used in Postgres with JSONB for flexibility.                                    |
| **UUID**                    | Universally Unique Identifier             | Globally unique ID used instead of sequential integers.                                                                |
| **TDE**                     | Transparent Data Encryption               | Encrypts DB storage automatically (Oracle, sometimes MySQL via plugin).                                                |
| **S3**                      | Simple Storage Service                    | AWS object storage; used for backups, imports/exports.                                                                 |
| **Pg\_stat\_statements**    | PostgreSQL extension                      | Tracks query performance metrics; used for tuning.                                                                     |
| **Pgcrypto**                | PostgreSQL extension                      | Provides cryptographic functions (hashing/encryption).                                                                 |
| **uuid-ossp**               | PostgreSQL extension                      | Generates UUIDs.                                                                                                       |
| **Parameter Group**         | DB configuration container                | Engine-level settings (memory, connections, replication, logging).                                                     |
| **Cluster Parameter Group** | Aurora-specific Parameter Group           | Settings applied cluster-wide (writer + all readers).                                                                  |
| **DB Parameter Group**      | Instance-level Parameter Group            | Settings applied per instance (writer or reader).                                                                      |
| **Option Group**            | Plugin container for MySQL/MariaDB/Oracle | Enables extra features (audit plugin, S3 integration, encryption).                                                     |
| **Extension**               | PostgreSQL module                         | Adds DB functionality (pg\_stat\_statements, uuid-ossp, pgcrypto).                                                     |
| **B/G**                     | Blue/Green Deployment                     | Strategy for near-zero downtime upgrade (provision green environment, switch traffic).                                 |
| **Terraform**               | Infrastructure as Code tool               | Used to define/manage DB resources (RDS, Aurora, parameter groups, option groups) declaratively.                       |
| **RDS Proxy**               | AWS managed DB connection proxy           | Pools DB connections; helps during failover or B/G cutover.                                                            |
| **OOM**                     | Out of Memory                             | Risk when max\_connections or buffer pool too high for instance RAM.                                                   |
| **ACUs**                    | Aurora Capacity Units                     | Unit of compute/memory for serverless Aurora; scales dynamically.                                                      |
| **Multi-AZ**                | Multiple Availability Zones               | DB instances deployed across 2–3 AZs for failover.                                                                     |
| **HA + DR**                 | High Availability + Disaster Recovery     | Ensures uptime (HA) and cross-region failover (DR).                                                                    |
| **Parameter (Static)**      | Requires DB reboot                        | Some settings need reboot to apply (innodb\_buffer\_pool\_size, shared\_buffers).                                      |
| **Parameter (Dynamic)**     | Applies immediately                       | Settings that don’t require restart (log\_level, slow\_query\_log).                                                    |

