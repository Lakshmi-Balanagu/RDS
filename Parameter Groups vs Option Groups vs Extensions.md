

# 📊 Parameter Groups vs Option Groups vs Extensions

| Feature Type        | Applies To                                                      | Purpose                                                                                     | Real-Time Examples                                                                                                                                                   | Notes                                                                                                                                                                       |
| ------------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Parameter Group** | ✅ RDS MySQL, MariaDB, PostgreSQL, Oracle, Aurora MySQL/Postgres | Control **engine configuration settings** (performance, memory, replication, logging, etc.) | - MySQL: `innodb_buffer_pool_size`, `max_connections`, `binlog_format` <br> - Postgres: `shared_buffers`, `autovacuum` settings, `work_mem`                          | - **Aurora has 2 layers**: Cluster Parameter Group + DB Parameter Group <br> - Some parameters = **dynamic** (no reboot) <br> - Some parameters = **static** (needs reboot) |
| **Option Group**    | ✅ RDS MySQL, MariaDB, Oracle <br> ❌ Aurora, ❌ Postgres          | Enable **extra plugins/features** not part of default DB                                    | - MySQL: `MARIADB_AUDIT` plugin (compliance logging), `S3_INTEGRATION` (native backup/restore), SSL features <br> - Oracle: TDE, OEM options                         | - Not available in Aurora <br> - Not used in Postgres (uses extensions instead) <br> - Changes may require reboot                                                           |
| **Extension**       | ✅ RDS PostgreSQL, Aurora PostgreSQL                             | Add **Postgres-specific modules** to extend DB functionality                                | - `pg_stat_statements` (query performance stats) <br> - `uuid-ossp` (UUID generation) <br> - `pgcrypto` (encryption functions) <br> - `postgis` (geospatial support) | - Enabled inside DB with `CREATE EXTENSION` <br> - Not needed in MySQL/MariaDB (handled via Option Groups instead)                                                          |

---

# ✅ How they work together in real-time

### 🔹 **Aurora MySQL (Production)**

* **Parameter Groups:** Tune performance (`max_connections`, `binlog_format=ROW`)
* **Option Groups:** ❌ Not used (Aurora ignores them)
* **Extensions:** ❌ Not supported (MySQL doesn’t have extensions like Postgres)

---

### 🔹 **Aurora PostgreSQL (Production)**

* **Parameter Groups:** Tune performance (`shared_buffers`, `autovacuum`)
* **Option Groups:** ❌ Not used
* **Extensions:** ✅ Use `pg_stat_statements`, `uuid-ossp`, `pgcrypto`

---

### 🔹 **RDS MySQL (Production)**

* **Parameter Groups:** Tune engine params (`innodb_buffer_pool_size`, `max_connections`)
* **Option Groups:** ✅ Add `MARIADB_AUDIT` plugin for compliance + `S3_INTEGRATION` for backups
* **Extensions:** ❌ Not supported (MySQL doesn’t have them)

---

### 🔹 **RDS PostgreSQL (Production)**

* **Parameter Groups:** Tune (`work_mem`, `effective_cache_size`)
* **Option Groups:** ❌ Not supported
* **Extensions:** ✅ Enable `pg_stat_statements`, `postgis` for GIS apps

---

✅ **Quick Analogy**:

* **Parameter Groups = config file** (`my.cnf`, `postgresql.conf`)
* **Option Groups = plugins/add-ons** (audit plugin, S3 integration, encryption)
* **Extensions (Postgres only) = installable modules** inside the database itself

