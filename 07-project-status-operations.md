# 7. Project Status & Operations

## Project Overview

**Organization**: Post-Eat
**Total Open Issues**: 49
**Repositories**: 6
**Team Members**: 6

---

## Open Issues Summary

### High Priority Issues (Production Blockers)

| Repository | Issue | Title | Assignee |
|------------|-------|-------|----------|
| laravel-api | [#123](https://github.com/Post-Eat/posteat-laravel-api/issues/123) | Create Seeder for Business Receipt Training Stats | sirdavis99 |
| laravel-api | [#122](https://github.com/Post-Eat/posteat-laravel-api/issues/122) | Create Receipt Processing Queue Job | sirdavis99 |
| vendor-web | [#46](https://github.com/Post-Eat/posteat-vendor-web/issues/46) | Implement Report Customer Dialog for Orders | sirdavis99 |
| vendor-web | [#45](https://github.com/Post-Eat/posteat-vendor-web/issues/45) | Disable Order Management & Add Report Functionality | sirdavis99 |
| vendor-web | [#44](https://github.com/Post-Eat/posteat-vendor-web/issues/44) | Implement Receipt Training Upload Flow | sirdavis99 |

### Medium Priority Issues

| Repository | Issue | Title | Assignee |
|------------|-------|-------|----------|
| vendor-web | [#43](https://github.com/Post-Eat/posteat-vendor-web/issues/43) | Update Reservation Flow - Disable CRUD Operations | sirdavis99 |
| admin-web | [#33](https://github.com/Post-Eat/posteat-react-web-admin/issues/33) | Verify User Reports Management for Order Flow | sirdavis99 |

### Issues by Repository

| Repository | Open Issues | Link |
|------------|-------------|------|
| posteat-laravel-api | 8 | [View](https://github.com/Post-Eat/posteat-laravel-api/issues) |
| posteat-vendor-web | 11 | [View](https://github.com/Post-Eat/posteat-vendor-web/issues) |
| posteat-react-web | 9 | [View](https://github.com/Post-Eat/posteat-react-web/issues) |
| posteat-react-web-admin | 6 | [View](https://github.com/Post-Eat/posteat-react-web-admin/issues) |
| posteat-react-native-mobile | 9 | [View](https://github.com/Post-Eat/posteat-react-native-mobile/issues) |
| posteat-django-api | 6 | [View](https://github.com/Post-Eat/posteat-django-api/issues) |

**Full Issue List**: [PROJECT_ISSUES.md](../PROJECT_ISSUES.md)

---

## Cross-Repository Features

### Receipt Training (3 repos)
1. **laravel-api #123** - Create Seeder
2. **laravel-api #122** - Create Queue Job
3. **vendor-web #44** - Upload Flow
4. **vendor-web #31** - Stats API Integration

### Order Reporting (3 repos)
1. **vendor-web #45** - Disable Order Management
2. **vendor-web #46** - Report Dialog
3. **admin-web #33** - Verify Reports

### Reservation Flow (2 repos)
1. **vendor-web #43** - Update Flow
2. **admin-web #32** - Disable Manual Creation

---

## Team Members

| Member | Focus | Repositories |
|--------|-------|-------------|
| [sirdavis99](https://github.com/sirdavis99) | Backend & Vendor Dashboard | laravel-api, vendor-web, admin-web |
| [kogneeto](https://github.com/kogneeto) | APIs | laravel-api |
| [tolubrianna](https://github.com/tolubrianna) | Consumer Web & Mobile | react-web, mobile |
| [TheElderlyChild](https://github.com/TheElderlyChild) | AI/ML | django-api |
| [curtizdev](https://github.com/curtizdev) | Mobile Infrastructure | mobile |
| [rokeeb-sh](https://github.com/rokeeb-sh) | Vendor Metrics | vendor-web |

---

## GitHub Project Management

### Creating a GitHub Project

1. Go to: https://github.com/orgs/Post-Eat/projects
2. Click **"New Project"**
3. Choose **"Table"** view
4. Name: **"PostEat Development Roadmap"**
5. Add all 6 repositories as sources

### Assigning Issues to Project

Use the provided script:
```bash
./assign-to-project.sh <project-number>
```

**Full Guide**: [PROJECT_SETUP_GUIDE.md](../PROJECT_SETUP_GUIDE.md)

---

## Platform Functionality Overview

### Consumer App (posteat-react-web & posteat-react-native-mobile)

**For End Users**:
1. **Restaurant Discovery** - Browse and search restaurants
2. **Business Pages** - View menus, hours, reviews
3. **Reservations** - Book tables at restaurants
4. **Orders** - Place food orders
5. **Reviews** - Leave ratings and reviews
6. **User Profile** - Manage account settings
7. **Social Login** - Google, Apple, Facebook authentication

### Vendor Dashboard (posteat-vendor-web)

**For Restaurant Owners/Staff**:
1. **Business Management** - Update restaurant info, hours, cuisines
2. **Menu Management** - Add/edit menu items
3. **Order Management** - View and manage orders
4. **Reservation Management** - Handle table bookings
5. **Staff Management** - Add employees with roles
6. **Analytics** - View business metrics and KPIs
7. **Gallery Management** - Upload business photos
8. **Receipt Training** - Upload receipts for ML training

### Admin Dashboard (posteat-react-web-admin)

**For Platform Administrators**:
1. **User Management** - View and manage all users
2. **Business Verification** - Approve/reject business registrations
3. **Content Moderation** - Review flagged content
4. **Report Management** - Handle user reports
5. **Role Management** - Configure admin roles
6. **Permission Management** - Set up permissions
7. **System Configuration** - Currencies, languages, cuisines
8. **Analytics** - Platform-wide metrics

### Backend APIs

**Laravel API**:
- JWT Authentication
- Business CRUD
- Reservation system
- Order processing
- Review system
- Push notifications (Expo)
- Multi-language support

**Django API**:
- ML/AI services
- Review authenticity scoring
- Receipt processing
- Recommendation engine

---

## Login Credentials Reference

> **Security Note**: Secure credential transfer should be done via Bitwarden, 1Password, or similar. Never share via chat/email.

| Service | Access Email | Purpose |
|---------|-------------|---------|
| GitHub | posteatsrl@gmail.com | Repository access |
| AWS | posteatsrl@gmail.com | Cloud infrastructure |
| Godaddy | difiorejacopo@gmail.com | Domain management |

### How to Get Access

1. **GitHub**: Contact current organization owner to be added to Post-Eat organization
2. **AWS**: Request IAM user access or use organization credentials
3. **Godaddy**: Request domain registrar access

---

## Production URLs

| Service | Production URL |
|---------|---------------|
| Web | https://posteat.co.uk |
| Admin | https://admin.posteat.co.uk |
| API | https://api.posteat.co.uk/v1 |

## Development URLs

| Service | Development URL |
|---------|-----------------|
| Web | https://dev.posteat.co.uk |
| Admin | https://admin-dev.posteat.co.uk |
| API | https://dev-api.posteat.co.uk |

---

## Mobile App

### Deep Link Scheme
`com.posteat.app://`

### Opening Deep Links (iOS)
```bash
npx uri-scheme open "com.posteat.app://homepage/resturants?uuid=UUID" --ios
```

### App Stores
- **iOS TestFlight**: Contact team for access
- **Android**: APK builds via EAS

---

## Related Documentation

- [PROJECT_ISSUES.md](../PROJECT_ISSUES.md) - Complete issue list
- [PROJECT_SETUP_GUIDE.md](../PROJECT_SETUP_GUIDE.md) - GitHub project setup
- [README.md](./README.md) - Main documentation index
- [Architecture & Business Logic](./02-architecture-business-logic.md)
