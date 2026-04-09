# 4. Credentials & Security

## Access Credentials

### Primary Accounts

| Service | Access Email | Notes |
|---------|-------------|-------|
| GitHub | posteatsrl@gmail.com | Organization owner |
| AWS | posteatsrl@gmail.com | EC2, S3, etc. |
| Godaddy | difiorejacopo@gmail.com | Domain management |

### Credential Transfer

> **⚠️ Security Warning**: Never transfer credentials via chat or email. Use a secure password manager.

**Recommended**:
- Bitwarden
- 1Password

---

## Third-Party Service Access

### Firebase

**Purpose**: Social authentication (Google, Apple, Facebook)

**Configuration**: `FIREBASE_CREDENTIALS` in Laravel `.env`

**How to set up**:
1. Go to Firebase Console: https://console.firebase.google.com
2. Create/select project
3. Generate service account credentials JSON
4. Download and reference in `FIREBASE_CREDENTIALS`

### AWS S3

**Purpose**: File storage (images, receipts, etc.)

**Required Environment Variables**:
```env
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key
AWS_DEFAULT_REGION=eu-west-2
AWS_BUCKET=posteat-uploads
```

### Stripe

**Purpose**: Payment processing

**Status**: Integration pending in some repositories

**Configuration**: Stripe API keys in environment

### Twilio

**Purpose**: SMS notifications

**Status**: Not currently active

### SendGrid

**Purpose**: Email notifications

**Status**: Not currently active

### OpenAI

**Purpose**: AI/ML features (Django API)

**Configuration**: `OPENAI_API_KEY` in Django `.env`

---

## Environment Variables Structure

### Laravel API (.env)

```env
# Application
APP_NAME=PostEat
APP_ENV=local
APP_KEY=
APP_URL=https://dev-api.posteat.co.uk
APP_DEBUG=true

# Database
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=posteat
DB_USERNAME=root
DB_PASSWORD=

# Authentication
JWT_SECRET=
JWT_TTL=60
FIREBASE_CREDENTIALS=/path/to/firebase-credentials.json

# AWS
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=eu-west-2
AWS_BUCKET=posteat-uploads
AWS_ENDPOINT=

# Redis
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

# Mail
MAIL_MAILER=smtp
MAIL_HOST=
MAIL_PORT=
MAIL_USERNAME=
MAIL_PASSWORD=
MAIL_ENCRYPTION=
MAIL_FROM_ADDRESS=

# Expo Push Notifications
EXPO_ACCESS_TOKEN=
```

### React Frontends (.env)

```env
# API Configuration
VITE_APP_API_URL=https://dev-api.posteat.co.uk/v1
VITE_APP_VENDOR_URL=https://dev-vendor.posteat.co.uk
VITE_APP_ADMIN_URL=https://admin-dev.posteat.co.uk
VITE_APP_CLIENT_URL=https://dev.posteat.co.uk

# Mobile Deep Links
VITE_APP_MOBILE_SCHEME=posteat
```

### Django API (.env)

```env
DEBUG=True
SECRET_KEY=
ALLOWED_HOSTS=*

# Database
DATABASE_URL=sqlite:///db.sqlite3

# OpenAI
OPENAI_API_KEY=

# Hugging Face
HF_TOKEN=
```

---

## Security Configuration

### JWT Authentication (Laravel)

```php
// config/jwt.php
'ttl' => env('JWT_TTL', 60),
'refresh_ttl' => env('JWT_REFRESH_TTL', 20160),
```

**Security Measures**:
- Token expiration: 60 minutes
- Refresh token: 2 weeks
- Blacklisting enabled

### CORS Configuration (Laravel)

```php
// config/cors.php
'paths' => ['api/*'],
'allowed_methods' => ['*'],
'allowed_origins' => ['https://posteat.co.uk'],
'allowed_origins_patterns' => [],
'allowed_headers' => ['*'],
'exposed_headers' => [],
'max_age' => 0,
'supports_credentials' => true,
```

### API Rate Limiting

```php
// app/Http/Middleware/ThrottleRequests.php
'throttle' => '60,1', // 60 requests per minute
```

### Spatie Permissions

Three-tier permission registry:
- `AdminPermission` - Platform admin permissions
- `BusinessPermission` - Business/vendor permissions
- `UserPermission` - End user permissions

---

## Encryption & Keys

### Application Key (Laravel)

```bash
php artisan key:generate
```

This generates a 32-character random string for `APP_KEY`.

### JWT Secret

```bash
php artisan jwt:secret
```

Or manually set `JWT_SECRET` in `.env`.

### Database Encryption

MySQL/PostgreSQL encryption at rest depends on:
- AWS RDS encryption (if using RDS)
- Server-side encryption on S3

---

## File Upload Security

### Media Library (Spatie)

- File type validation
- Max file size limits
- Image optimization
- S3 secure URLs

### Upload Flow

1. Client uploads to Laravel API
2. Validation of file type/size
3. Processing (resize, optimize)
4. Storage to S3 with secure URL
5. CDN delivery (optional)

---

## Password Security

### User Passwords

- Hashed using bcrypt (Laravel default)
- Minimum 8 characters
- No plain-text storage

### Service Accounts

- Rotated regularly
- Stored in password manager
- Never committed to repository

---

## GitHub Secrets

All sensitive data stored as GitHub Actions secrets:

| Secret Name | Purpose |
|-------------|---------|
| `DEV_ENV_B64` | Dev environment variables (base64) |
| `STAGING_ENV_B64` | Staging environment variables |
| `PROD_ENV_B64` | Production environment variables |
| `EC2_HOST_DNS` | EC2 instance hostname |
| `SSH_USERNAME` | SSH username for deployment |
| `SSH_PRIVATE_KEY` | SSH private key |

### Adding Secrets

1. Go to repository **Settings** → **Secrets and variables** → **Actions**
2. Click **New repository secret**
3. Enter name and value
4. Reference in workflow: `${{ secrets.SECRET_NAME }}`

---

## Security Best Practices

### Code Security

1. **Never commit secrets** - Use `.gitignore` for `.env` files
2. **Validate input** - All user input sanitized
3. **Prepared statements** - SQL injection prevention
4. **CSRF protection** - Laravel handles automatically

### Deployment Security

1. **SSH keys** - Use key-based authentication
2. **Firewall** - Only necessary ports open
3. **SSL/TLS** - All traffic encrypted
4. **Regular updates** - Keep dependencies updated

### Monitoring

- Failed login attempts logged
- Rate limiting on auth endpoints
- Anomaly detection (if enabled)

---

## Related Documentation

- [Source Code & Repositories](./01-source-code-repositories.md)
- [Infrastructure & DevOps](./03-infrastructure-devops.md)
- [Project Status & Operations](./07-project-status-operations.md)
- [Laravel .env.example](../posteat-laravel-api/.env.example)
