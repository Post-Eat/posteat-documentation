# PostEat Platform - Technical Handover Documentation

This repository contains the comprehensive technical handover documentation for the PostEat platform.

## Table of Contents

### 1. [Source Code & Repositories](./01-source-code-repositories.md)
- GitHub access and ownership
- Repository structure and setup guides
- Git workflow documentation
- System dependencies
- Complete tech stack listing

### 2. [Architecture & Business Logic](./02-architecture-business-logic.md)
- Complete architecture diagrams
- API documentation (Postman collections)
- Database schema overview
- Core business logic (KPIs, recommendation logic, sustainability metrics, expiry logic)

### 3. [Infrastructure & DevOps](./03-infrastructure-devops.md)
- Cloud access (AWS EC2)
- CI/CD pipeline documentation
- Domain management & SSL
- Deployment processes

### 4. [Credentials & Security](./04-credentials-security.md)
- Third-party service access
- .env structure and environment variables
- Security configurations

### 5. [Data & Backup](./05-data-backup.md)
- Database dumps and restore procedures
- Backup strategies

### 6. [Quality & Compliance](./06-quality-compliance.md)
- Test suite documentation
- GDPR/privacy documentation

### 7. [Project Status & Operations](./07-project-status-operations.md)
- Open issues and project boards
- Admin and user manual overview
- Login credentials reference

---

## Platform Overview

**PostEat** is a multi-application food ordering platform consisting of:

| Repository | Technology | Description |
|------------|------------|-------------|
| `posteat-laravel-api` | Laravel 10 (PHP 8.2) | Core REST API backend |
| `posteat-react-web` | React 18 + TypeScript | Public consumer website |
| `posteat-vendor-web` | React 18 + TypeScript | Vendor management dashboard |
| `posteat-react-web-admin` | React 18 + TypeScript | Platform admin dashboard |
| `posteat-react-native-mobile` | Expo + React Native | Mobile app (iOS/Android) |
| `posteat-django-api` | Django 5.2 (Python) | ML/AI services API |

---

## Quick Access Links

### Repositories (GitHub)
- **Organization**: [Post-Eat](https://github.com/Post-Eat)
- [posteat-laravel-api](https://github.com/Post-Eat/posteat-laravel-api)
- [posteat-react-web](https://github.com/Post-Eat/posteat-react-web)
- [posteat-vendor-web](https://github.com/Post-Eat/posteat-vendor-web)
- [posteat-react-web-admin](https://github.com/Post-Eat/posteat-react-web-admin)
- [posteat-react-native-mobile](https://github.com/Post-Eat/posteat-react-native-mobile)
- [posteat-django-api](https://github.com/Post-Eat/posteat-django-api)

### API Documentation
- **Postman Collection**: https://documenter.getpostman.com/view/35257821/2sAY4sk5Mb
- **Mobile API**: https://documenter.getpostman.com/view/41814866/2sAYX6p22q

### Production URLs
| Service | Production URL |
|---------|---------------|
| Web | https://posteat.co.uk |
| Admin | https://admin.posteat.co.uk |
| API | https://api.posteat.co.uk/v1 |

### Development URLs
| Service | Development URL |
|---------|-----------------|
| Web | https://dev.posteat.co.uk |
| Admin | https://admin-dev.posteat.co.uk |
| API | https://dev-api.posteat.co.uk |

---

## Login Credentials

> **Note**: Secure credential transfer should be done via a password manager (Bitwarden, 1Password). Contact posteatsrl@gmail.com for access.

| Service | Access Email |
|---------|-------------|
| GitHub | posteatsrl@gmail.com |
| AWS | posteatsrl@gmail.com |
| Godaddy Domain | difiorejacopo@gmail.com |

---

## Team Members

| Member | Focus | Repositories |
|--------|-------|-------------|
| sirdavis99 | Backend & Vendor Dashboard | laravel-api, vendor-web, admin-web |
| kogneeto | APIs | laravel-api |
| tolubrianna | Consumer Web & Mobile | react-web, mobile |
| TheElderlyChild | AI/ML | django-api |
| curtizdev | Mobile Infrastructure | mobile |
| rokeeb-sh | Vendor Metrics | vendor-web |

---

## Related Documentation

- [Main README](../README.md) - Platform overview and project management
- [AGENTS.md](../AGENTS.md) - Agent conventions and code style guidelines
- [Project Issues](../PROJECT_ISSUES.md) - 49 open issues across 6 repositories
- [Project Setup Guide](../PROJECT_SETUP_GUIDE.md) - GitHub project management
- [GEMINI.md](../GEMINI.md) - Gemini project overview
