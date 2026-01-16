## 1 Backup a **single database** (most common)

### Using `pg_dump`

This creates a logical backup (SQL or custom format).

#### SQL file (easy to restore & read)

```bash
pg_dump -U postgres -d cardlabs > cardlabs_backup.sql
```

Restore:

```bash
psql -U postgres -d cardlabs < cardlabs_backup.sql
```

---

#### Custom format (recommended for production)

Smaller, faster, flexible restore.

```bash
pg_dump -U postgres -Fc cardlabs > cardlabs_backup.dump
```

Restore:

```bash
createdb mydatabase
pg_restore -U postgres -d cardlabs cardlabs_backup.dump
```


## 2️ Backup **all databases** on the server

### Using `pg_dumpall`

```bash
pg_dumpall -U postgres > full_backup.sql
```

Restore:

```bash
psql -U postgres < full_backup.sql
```

Includes roles and permissions
Slower, large files

---

## 3️ Backup from a **remote server (EC2, VPS, etc.)**

Run this on **your local machine**:

```bash
pg_dump -h SERVER_IP -U postgres -d mydatabase > mydatabase.sql
```

Example:

```bash
pg_dump -h 54.xxx.xxx.xxx -U myuser -d school_db > school_db.sql
```

---

## 4️ Backup **with password (non-interactive)**

Use environment variable (safe for scripts):

```bash
export PGPASSWORD="mypassword"
pg_dump -U postgres -d mydatabase > mydatabase.sql
```

Windows (PowerShell):

```powershell
setx PGPASSWORD "mypassword"
pg_dump -U postgres -d mydatabase > mydatabase.sql
```

---

## 5️ Automated daily backups (Linux cron)

```bash
crontab -e
```

Add:

```bash
0 2 * * * pg_dump -U postgres mydatabase > /backups/mydatabase_$(date +\%F).sql
```

---

## 6️ Physical backup (entire cluster)

Used for **disaster recovery** (advanced).

```bash
pg_basebackup -U replication_user -D /backup/location -Fp -Xs -P
```

Exact copy of data directory
Requires replication setup

---

## Best Practice (Recommended)

For most use cases:

* ✅ `pg_dump -Fc`
* ✅ Daily automated backups
* ✅ Store off-server (S3, Google Drive, USB)