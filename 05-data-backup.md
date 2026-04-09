# 5. Data & Backup

## Database Overview

### Laravel API Database

**Type**: MySQL (primary) / PostgreSQL (supported)

**Location**: AWS EC2 (colocated) or AWS RDS (if migrated)

**Tables**: 80+ including:
- `users` - User accounts
- `businesses` - Restaurant profiles
- `reservations` - Booking records
- `orders` - Order records
- `reviews` - User reviews
- `media` - File attachments
- Spatie permission tables

### Django API Database

**Type**: SQLite (development) / PostgreSQL (production recommended)

**Location**: Within Django project

**Tables**:
- `Review` - Processed reviews for ML
- `Receipt` - Receipt data for training

---

## Database Dumps

### Creating a Database Dump

#### MySQL

```bash
# Full database dump
mysqldump -u root -p posteat > posteat_dump_$(date +%Y%m%d).sql

# Compressed dump
mysqldump -u root -p posteat | gzip > posteat_dump_$(date +%Y%m%d).sql.gz

# Dump with drop table statements (for clean restore)
mysqldump -u root -p --add-drop-table posteat > posteat_dump_$(date +%Y%m%d).sql
```

#### PostgreSQL

```bash
# Full database dump
pg_dump -U postgres posteat > posteat_dump_$(date +%Y%m%d).sql

# Compressed dump
pg_dump -U postgres posteat | gzip > posteat_dump_$(date +%Y%m%d).sql.gz
```

### Staging/Test Database

For staging without sensitive data:

```bash
# Dump production
mysqldump -u root -p posteat > production_dump.sql

# Create staging database
mysql -u root -p -e "CREATE DATABASE posteat_staging"

# Restore to staging (users will need password redaction)
mysql -u root -p posteat_staging < production_dump.sql

# Redact sensitive data
mysql -u root -p posteat_staging < redact_sensitive_data.sql
```

### Selective Dump (Specific Tables)

```bash
# Dump only users and businesses tables
mysqldump -u root -p posteat users businesses > users_businesses_dump.sql
```

---

## Database Restore Procedures

### Full Restore

#### MySQL

```bash
# Create database if not exists
mysql -u root -p -e "CREATE DATABASE posteat"

# Restore from dump
mysql -u root -p posteat < posteat_dump_20240115.sql

# With compressed file
gunzip < posteat_dump_20240115.sql.gz | mysql -u root -p posteat
```

#### PostgreSQL

```bash
# Drop and recreate
psql -U postgres -c "DROP DATABASE posteat;"
psql -U postgres -c "CREATE DATABASE posteat;"

# Restore
psql -U postgres posteat < posteat_dump_20240115.sql
```

### Point-in-Time Recovery

For MySQL with binary logs enabled:

```bash
# Restore to specific point in time
mysqlbinlog --stop-datetime="2024-01-15 10:00:00" binlog.* | mysql -u root -p posteat
```

---

## Backup Strategies

### Automated Backups

#### Option 1: Cron-based Backup

```bash
# /etc/cron.daily/posteat-backup
#!/bin/bash
DATE=$(date +%Y%m%d)
mysqldump -u root -p'password' posteat | gzip > /backups/posteat_$DATE.sql.gz
# Keep last 30 days
find /backups -name "posteat_*.sql.gz" -mtime +30 -delete
```

#### Option 2: Docker Volume Backup

```bash
#!/bin/bash
DATE=$(date +%Y%m%d)
docker run --rm -v posteat_mysql_data:/data -v $(pwd):/backup alpine tar czf /backup/mysql_backup_$DATE.tar.gz /data
```

### Backup Locations

| Type | Location | Retention |
|------|----------|-----------|
| Daily | `/backups/` on EC2 | 7 days |
| Weekly | S3 bucket | 4 weeks |
| Monthly | S3 bucket | 12 months |

---

## File System Backups

### Media Files (S3)

**Already backed up by S3**:
- Automatic versioning enabled
- Cross-region replication (if configured)

**Manual sync**:
```bash
# Sync to local backup
aws s3 sync s3://posteat-uploads ./backups/media/ --region eu-west-2

# Sync with deletion tracking
aws s3 sync s3://posteat-uploads ./backups/media/ --delete
```

### Application Storage

```bash
# Backup Laravel storage
tar czf storage_backup_$(date +%Y%m%d).tar.gz -C /path/to/app storage/

# Backup only important directories
tar czf storage_backup_$(date +%Y%m%d).tar.gz \
  /path/to/app/storage/app \
  /path/to/app/storage/logs \
  /path/to/app/bootstrap/cache
```

---

## Backup Verification

### Restore Test Procedure

1. **Create test environment**
   ```bash
   mysql -u root -p -e "CREATE DATABASE posteat_test"
   ```

2. **Restore to test**
   ```bash
   mysql -u root -p posteat_test < latest_backup.sql
   ```

3. **Verify integrity**
   ```sql
   USE posteat_test;
   SELECT COUNT(*) FROM users;
   SELECT COUNT(*) FROM businesses;
   ```

4. **Check application**
   - Start application with test database
   - Verify key operations work

### Monitoring Backup Success

```bash
# Check backup exists and is recent
if [ -f /backups/posteat_$(date +%Y%m%d).sql.gz ]; then
    echo "Backup successful"
else
    echo "Backup FAILED" | mail -s "Backup Alert" admin@posteat.co.uk
fi
```

---

## Disaster Recovery

### Complete System Failure

1. **Provision new EC2 instance**
2. **Restore from latest backup**
   ```bash
   # Install Docker
   # Clone repository
   # Restore database
   mysql -u root -p posteat < latest_backup.sql
   # Restore S3 files
   aws s3 sync s3://posteat-uploads ./storage --region eu-west-2
   ```
3. **Start services**
   ```bash
   docker compose up -d
   ```

### Database-Only Failure

1. **Stop application**
   ```bash
   docker compose stop app
   ```

2. **Drop and recreate database**
   ```bash
   mysql -u root -p -e "DROP DATABASE posteat; CREATE DATABASE posteat;"
   ```

3. **Restore**
   ```bash
   mysql -u root -p posteat < latest_backup.sql
   ```

4. **Restart application**
   ```bash
   docker compose start app
   ```

---

## Data Redaction for Testing

When creating test/staging databases:

```sql
-- Redact user passwords (keep hashes for testing)
UPDATE users SET password = '$2y$10$dummy_hash_for_testing';

-- Redact personal data
UPDATE users SET 
  email = CONCAT('user_', id, '@test.local'),
  phone = '0000000000',
  name = CONCAT('Test User ', id);

-- Clear sensitive business data
UPDATE businesses SET 
  bank_account = NULL,
  tax_id = NULL;
```

---

## Related Documentation

- [Infrastructure & DevOps](./03-infrastructure-devops.md)
- [Credentials & Security](./04-credentials-security.md)
- [Laravel Database Migrations](../posteat-laravel-api/database/migrations/)
