# AWS RDS Project (Using AWS Console)

This project documents the hands-on steps I followed to create and manage Amazon RDS (Relational Database Service) using the AWS Console. It covers database creation, connection using Sqlectron, replication setup, snapshots, and database migration.

---

## ✅ What I Learned

- Provisioning RDS instances with best practices
- Connecting to RDS using external clients (Sqlectron)
- Enabling Read Replication for performance scaling
- Creating and restoring manual DB snapshots
- Migrating DBs using snapshot-based restores

---

## 🔧 Steps Performed (with Key Insights)

### 1. **Create an RDS Instance**
- **Service**: Navigate to `RDS Console → Create Database`
- **Mode**: Use **Standard Create** for full control
- **Engine**: Choose MySQL/PostgreSQL (based on use case)
- **Instance Class**: Choose `db.t3.micro` for free tier or based on performance needs
- **Storage**: Start with General Purpose (SSD) - autoscaling optional
- **Public Access**: **Enable** if connecting from your machine (make sure security groups allow inbound traffic)
- **VPC**: Choose default or custom VPC with internet access
- **Backup**: Enable automatic backups for recovery
- **Multi-AZ** (Optional): Use for high availability setups

> ⚠️ **Important**: Ensure the **security group** attached allows **inbound access on port 3306** (MySQL) or 5432 (PostgreSQL) from your IP.

---

### 2. **Connect RDS to Sqlectron**
- Go to **RDS dashboard → DB instance → Connectivity**
- Copy:
  - **Endpoint**
  - **Port**
- In **Sqlectron**:
  - Select your DB engine
  - Fill in endpoint, port, username, and password
  - Test and save the connection

> 🔒 **Tip**: Ensure your local machine IP is **added to inbound rules** of the RDS security group. Otherwise, connection will fail.

---

### 3. **Create a Read Replica**
- In RDS, select your instance → **Actions → Create Read Replica**
- Choose:
  - **Instance class** for replica
  - **Multi-AZ** (optional)
  - **Storage type** (SSD or magnetic)
- Launch the replica

> 📌 **Use Case**: Offload read queries to this replica to reduce load on the primary DB. Useful for reporting, analytics, etc.

> 🔁 **Note**: Replication is **asynchronous**. There's a small lag between the primary and replica.

---

### 4. **Take Manual Snapshots**
- Go to your DB instance → **Actions → Take Snapshot**
- Enter a meaningful **snapshot name** (e.g., `before-schema-change-Apr2025`)
- Confirm and wait for snapshot creation

> 📦 **Manual snapshots** are retained until you delete them, unlike automated backups that expire after a set retention period.

> 📍 Best Practice: Take snapshots before any **major schema changes or migrations**.

---

### 5. **Restore DB from Snapshot (Migration)**
- Go to `Snapshots → Select Snapshot → Actions → Restore Snapshot`
- Choose:
  - New DB instance identifier (e.g., `rds-restored-db-1`)
  - Modify settings like instance class, public access, etc.
- Launch

> 🔄 **Use Case**: Cloning DBs for testing, staging, or recovery from failures.

> 🧠 **Important**: Restored DBs are **new standalone instances**. They do not retain the replication setup or original configurations unless manually applied.

---

## 📘 Tools Used
- **AWS Management Console**
- **Sqlectron** – open-source DB GUI client



