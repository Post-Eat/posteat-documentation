# 1. Source Code & Repositories

## GitHub Access

**Organization**: [Post-Eat](https://github.com/Post-Eat)

**Admin Access Email**: posteatsrl@gmail.com

### Repository List

| Repository | Description | Main Language |
|------------|-------------|---------------|
| [posteat-laravel-api](https://github.com/Post-Eat/posteat-laravel-api) | Core REST API backend | PHP |
| [posteat-react-web](https://github.com/Post-Eat/posteat-react-web) | Public consumer website | TypeScript |
| [posteat-vendor-web](https://github.com/Post-Eat/posteat-vendor-web) | Vendor management dashboard | TypeScript |
| [posteat-react-web-admin](https://github.com/Post-Eat/posteat-react-web-admin) | Platform admin dashboard | TypeScript |
| [posteat-react-native-mobile](https://github.com/Post-Eat/posteat-react-native-mobile) | Mobile app (iOS/Android) | TypeScript |
| [posteat-django-api](https://github.com/Post-Eat/posteat-django-api) | ML/AI services API | Python |

---

## Setup Guides by Repository

### posteat-laravel-api

**GitHub**: https://github.com/Post-Eat/posteat-laravel-api

**README**: [posteat-laravel-api/README.md](../posteat-laravel-api/README.md)

#### Requirements
- PHP **8.2+**
- Composer **2.x**
- MySQL **8.0+** or PostgreSQL **14+**
- Redis (optional, for caching/queues)

#### Setup Commands
```bash
# Clone and install dependencies
git clone https://github.com/Post-Eat/posteat-laravel-api.git
cd posteat-laravel-api
composer install

# Environment setup
cp .env.example .env
php artisan key:generate

# Configure .env with database credentials
# Then run migrations
php artisan migrate

# (Optional) Seed sample data
php artisan db:seed

# Start development server
php artisan serve
```

#### Key Dependencies (composer.json)
- Laravel 10.x
- JWT Auth (tymon/jwt-auth)
- Firebase (kreait/laravel-firebase)
- Spatie Permission (spatie/laravel-permission)
- Spatie Media Library
- AWS S3 Flysystem
- Laravel Spatial (eloquent-spatial)

**Full composer.json**: [posteat-laravel-api/composer.json](../posteat-laravel-api/composer.json)

---

### posteat-react-web

**GitHub**: https://github.com/Post-Eat/posteat-react-web

**README**: [posteat-react-web/README.md](../posteat-react-web/README.md)

#### Requirements
- Node.js **20+**
- Yarn **4.x** (managed via Corepack)

#### Setup Commands
```bash
# Clone and enable corepack
git clone https://github.com/Post-Eat/posteat-react-web.git
cd posteat-react-web
corepack enable

# Install dependencies
yarn install

# Run development server
yarn dev
```

#### Key Dependencies
- React 18 + TypeScript
- Vite 5
- Redux Toolkit
- Chakra UI + Mantine
- React Router (vite-plugin-remix-router)
- i18next for internationalization

**Full package.json**: [posteat-react-web/package.json](../posteat-react-web/package.json)

---

### posteat-vendor-web

**GitHub**: https://github.com/Post-Eat/posteat-vendor-web

**README**: [posteat-vendor-web/README.md](../posteat-vendor-web/README.md)

#### Requirements
- Node.js **20+**
- Yarn **4.x**

#### Setup Commands
```bash
git clone https://github.com/Post-Eat/posteat-vendor-web.git
cd posteat-vendor-web
corepack enable
yarn install
yarn dev
```

**Full package.json**: [posteat-vendor-web/package.json](../posteat-vendor-web/package.json)

---

### posteat-react-web-admin

**GitHub**: https://github.com/Post-Eat/posteat-react-web-admin

**README**: [posteat-react-web-admin/README.md](../posteat-react-web-admin/README.md)

#### Requirements
- Node.js **20+**
- Yarn **4.x**

#### Setup Commands
```bash
git clone https://github.com/Post-Eat/posteat-react-web-admin.git
cd posteat-react-web-admin
corepack enable
yarn install
yarn dev
```

**Full package.json**: [posteat-react-web-admin/package.json](../posteat-react-web-admin/package.json)

---

### posteat-react-native-mobile

**GitHub**: https://github.com/Post-Eat/posteat-react-native-mobile

**README**: [posteat-react-native-mobile/README.md](../posteat-react-native-mobile/README.md)

#### Requirements
- Node.js **20+**
- Yarn **4.x**
- Expo CLI
- Xcode (for iOS builds)
- Android Studio (for Android builds)

#### Setup Commands
```bash
git clone https://github.com/Post-Eat/posteat-react-native-mobile.git
cd posteat-react-native-mobile
corepack enable
yarn install
yarn start
```

#### Build Commands
```bash
yarn build:ios        # iOS build
yarn build:android     # Android build
yarn ios               # Run on iOS simulator
yarn android           # Run on Android emulator
```

**Full package.json**: [posteat-react-native-mobile/package.json](../posteat-react-native-mobile/package.json)

---

### posteat-django-api

**GitHub**: https://github.com/Post-Eat/posteat-django-api

**README**: [posteat-django-api/README.md](../posteat-django-api/README.md)

#### Requirements
- Python 3.10+
- pip

#### Setup Commands
```bash
git clone https://github.com/Post-Eat/posteat-django-api.git
cd posteat-django-api
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
```

**Full requirements.txt**: [posteat-django-api/requirements.txt](../posteat-django-api/requirements.txt)

---

## Git Workflow

### Branching Strategy

| Branch | Purpose |
|--------|---------|
| `main` | Production code |
| `staging` | Pre-production testing |
| `develop` | Active development |

### Pull Request Process

1. Create feature branch from `develop`
2. Make changes and commit with descriptive messages
3. Push branch and create PR against `develop`
4. Code review required before merge
5. CI/CD automatically deploys on merge to `main`, `staging`, or `develop`

### CI/CD Workflows

All projects use GitHub Actions for CI/CD:

| Repository | Workflow File |
|------------|---------------|
| posteat-laravel-api | [.github/workflows/deploy-container.yml](../posteat-laravel-api/.github/workflows/deploy-container.yml) |
| posteat-react-web | [.github/workflows/deploy-container.yml](../posteat-react-web/.github/workflows/deploy-container.yml) |
| posteat-vendor-web | [.github/workflows/deploy-container.yml](../posteat-vendor-web/.github/workflows/deploy-container.yml) |
| posteat-react-web-admin | [.github/workflows/deploy-container.yml](../posteat-react-web-admin/.github/workflows/deploy-container.yml) |
| posteat-react-native-mobile | [.github/workflows/publish-dev.yml](../posteat-react-native-mobile/.github/workflows/publish-dev.yml), [publish-prod.yml](../posteat-react-native-mobile/.github/workflows/publish-prod.yml) |

---

## System-Level Dependencies

### Docker
All projects include Docker support via `docker-compose.yml` and `Dockerfile`.

```bash
# Build and run with Docker (example for Laravel API)
cd posteat-laravel-api
docker compose up --build
```

### Traefik (Reverse Proxy)
Production deployments use Traefik for reverse proxy with automatic SSL via Let's Encrypt.

### Redis
Used for caching and queues in Laravel API.

---

## Complete Tech Stack

### Backend
| Technology | Version | Purpose |
|------------|---------|---------|
| Laravel | 10.x | Core API framework |
| PHP | 8.2+ | Backend language |
| Django | 5.2.1 | ML/AI API |
| Python | 3.10+ | ML/AI language |
| MySQL | 8.0+ | Primary database |
| PostgreSQL | 14+ | Alternative database |
| Redis | Latest | Caching & queues |

### Frontend
| Technology | Version | Purpose |
|------------|---------|---------|
| React | 18 | UI framework |
| React Native | Latest | Mobile apps |
| TypeScript | 5.x | Type safety |
| Vite | 5 | Build tool |
| Expo | Latest | React Native tooling |
| Redux Toolkit | Latest | State management |
| Chakra UI | Latest | Component library |
| Mantine | 7/8 | Component library |

### Infrastructure
| Technology | Purpose |
|------------|---------|
| AWS EC2 | Cloud hosting |
| Docker | Containerization |
| GitHub Actions | CI/CD |
| Traefik | Reverse proxy |
| Let's Encrypt | SSL certificates |
| Godaddy | Domain registrar |

### Third-Party Services
| Service | Purpose |
|---------|---------|
| Firebase | Social authentication |
| AWS S3 | File storage |
| Expo | Push notifications |
| Stripe | Payment processing |
| Twilio | SMS notifications |
| SendGrid | Email notifications |
| OpenAI | AI/ML features |

---

## Related Documentation

- [Architecture & Business Logic](./02-architecture-business-logic.md)
- [Infrastructure & DevOps](./03-infrastructure-devops.md)
- [Main README](../README.md)
- [AGENTS.md](../AGENTS.md) - Code style guidelines
