# 3. Infrastructure & DevOps

## Cloud Infrastructure

### AWS Access

**Account Email**: posteatsrl@gmail.com

**Access Required**:
- AWS Console: https://aws.amazon.com
- EC2 instances for container hosting
- S3 for file storage

### Server Details

Production servers are hosted on AWS EC2 with Traefik reverse proxy.

**Key Configuration** (from deploy workflows):
- Base domain: `posteat.co.uk`
- Container port for API: `8000`
- Container port for Web: `3000`
- SSL managed via Let's Encrypt (Traefik ACME)

---

## CI/CD Pipelines

All projects use GitHub Actions with custom workflows from `uncoverthefuture-org/actions`.

### Laravel API Deployment

**Workflow File**: [../posteat-laravel-api/.github/workflows/deploy-container.yml](../posteat-laravel-api/.github/workflows/deploy-container.yml)

```yaml
name: CI CD Build and Deploy Container App

on:
  push:
    branches: [ main, staging, develop ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    environment:
      name: ${{ github.ref == 'refs/heads/main' && 'production' || '' }}
```

**Deployment Process**:
1. Triggered on push to `main`, `staging`, or `develop`
2. Build and push container image
3. Deploy to EC2 via SSH
4. Traefik automatically handles SSL and routing

**Secrets Required**:
- `DEV_ENV_B64` - Development environment variables (base64)
- `STAGING_ENV_B64` - Staging environment variables (base64)
- `PROD_ENV_B64` - Production environment variables (base64)
- `EC2_HOST_DNS` - EC2 instance hostname
- `SSH_USERNAME` - SSH username
- `SSH_PRIVATE_KEY` - SSH private key

### React Web Deployment

**Workflow File**: [../posteat-react-web/.github/workflows/deploy-container.yml](../posteat-react-web/.github/workflows/deploy-container.yml)

**Process**: Same as Laravel API with web-specific configuration.

### Vendor Web Deployment

**Workflow File**: [../posteat-vendor-web/.github/workflows/deploy-container.yml](../posteat-vendor-web/.github/workflows/deploy-container.yml)

### Admin Web Deployment

**Workflow File**: [../posteat-react-web-admin/.github/workflows/deploy-container.yml](../posteat-react-web-admin/.github/workflows/deploy-container.yml)

### Mobile App Deployment

**Workflow Files**:
- [../posteat-react-native-mobile/.github/workflows/publish-dev.yml](../posteat-react-native-mobile/.github/workflows/publish-dev.yml) - Dev builds
- [../posteat-react-native-mobile/.github/workflows/publish-prod.yml](../posteat-react-native-mobile/.github/workflows/publish-prod.yml) - Production builds

**Process**: Uses EAS (Expo Application Services) for building.

---

## Deployment Process

### Standard Deployment (GitHub Actions)

1. **Code Push** - Developer pushes to branch
2. **CI Build** - GitHub Actions builds container
3. **Image Push** - Container image pushed to registry
4. **SSH Deploy** - Actions SSH to EC2 and pull new image
5. **Traefik Routing** - Automatic routing via labels

### Manual Deployment

```bash
# SSH to server
ssh user@ec2-host-dns

# Pull latest container
docker compose pull

# Restart services
docker compose up -d

# View logs
docker compose logs -f
```

### Docker Compose Configuration

All projects include `docker-compose.yml`:

```yaml
# Example from posteat-laravel-api
version: '3.8'
services:
  app:
    build: .
    ports:
      - "8000:8000"
    environment:
      - APP_ENV=production
    volumes:
      - ./storage:/app/storage
```

---

## Domain Management

### Domain Registrar

**Godaddy**: difiorejacopo@gmail.com

**Domain**: posteat.co.uk

### DNS Configuration

DNS is configured in Godaddy to point to AWS EC2 instance:
- A record: `@` → EC2 IP
- CNAME: `www` → EC2 hostname
- Subdomains for each service:
  - `api.posteat.co.uk`
  - `dev-api.posteat.co.uk`
  - `admin.posteat.co.uk`
  - `admin-dev.posteat.co.uk`

### SSL Certificates

**Provider**: Let's Encrypt (via Traefik)

**How it works**:
1. Traefik automatically requests certificates
2. ACME protocol handles validation
3. Certificates auto-renew before expiry
4. No manual certificate management needed

---

## Monitoring & Logging

### Application Logging

**Laravel**: Built-in logging to `storage/logs/`

**Django**: Logging to console/stdout

### Container Logs

```bash
# View all logs
docker compose logs -f

# View specific service
docker compose logs -f app

# View last N lines
docker compose logs --tail=100
```

### Health Checks

Traefik performs health checks on containers:
- API health: `GET /health` or similar
- Web health: HTTP response check

---

## Environment Configuration

### Environment Variables (All Projects)

#### Laravel API (.env)

```env
APP_URL=https://dev-api.posteat.co.uk
APP_ENV=local

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=posteat
DB_USERNAME=root
DB_PASSWORD=

JWT_SECRET=your-jwt-secret
FIREBASE_CREDENTIALS=path/to/credentials.json

# AWS S3
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=eu-west-2
AWS_BUCKET=posteat-uploads

# Redis
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379
```

#### React Frontends (.env)

```env
VITE_APP_API_URL=https://dev-api.posteat.co.uk/v1
VITE_APP_VENDOR_URL=https://dev-vendor.posteat.co.uk
VITE_APP_ADMIN_URL=https://admin-dev.posteat.co.uk
VITE_APP_MOBILE_SCHEME=posteat
```

### Secret Management

Secrets are stored as GitHub Actions secrets:
1. Encoded as base64 in workflow files
2. Decoded during deployment
3. Injected as environment variables

---

## Backup Procedures

### Database Backups

**Laravel API** includes MySQL database.

**Backup Command**:
```bash
mysqldump -u root -p posteat > backup_$(date +%Y%m%d).sql
```

### Container Volumes

Docker volumes persist:
- Database data
- Uploaded media files
- Application storage

**Backup Command**:
```bash
docker compose down
docker run --rm -v posteat_storage:/data -v $(pwd):/backup alpine tar czf /backup/storage_backup.tar.gz /data
```

---

## Infrastructure Diagram

```
Internet
    │
    ▼
┌─────────────────┐
│   Godaddy DNS   │
│  posteat.co.uk  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  AWS EC2        │
│  (Traefik)      │
│                 │
│  ┌───────────┐  │
│  │ Laravel   │  │ :8000
│  │ API       │  │
│  └───────────┘  │
│                 │
│  ┌───────────┐  │
│  │ React Web │  │ :3000
│  └───────────┘  │
│                 │
│  ┌───────────┐  │
│  │ Vendor    │  │ :3000
│  │ Dashboard │  │
│  └───────────┘  │
│                 │
│  ┌───────────┐  │
│  │ Admin     │  │ :3000
│  │ Dashboard │  │
│  └───────────┘  │
│                 │
│  ┌───────────┐  │
│  │ MySQL     │  │
│  └───────────┘  │
│                 │
│  ┌───────────┐  │
│  │ Redis     │  │
│  └───────────┘  │
└─────────────────┘
         │
         ▼
┌─────────────────┐
│    AWS S3       │
│  (File Storage) │
└─────────────────┘
```

---

## Related Documentation

- [Source Code & Repositories](./01-source-code-repositories.md)
- [Architecture & Business Logic](./02-architecture-business-logic.md)
- [Credentials & Security](./04-credentials-security.md)
- [Data & Backup](./05-data-backup.md)
