# 2. Architecture & Business Logic

## System Architecture Overview

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              CLIENTS                                        │
├─────────────┬─────────────┬─────────────┬─────────────┬───────────────────┤
│  Mobile     │  Web        │  Vendor     │  Admin      │  External         │
│  App        │  (Consumer) │  Dashboard  │  Dashboard  │  Services         │
│  (Expo)     │  (React)    │  (React)    │  (React)    │                   │
└──────┬──────┴──────┬──────┴──────┬──────┴──────┬──────┴────────┬────────┘
       │              │              │              │               │
       └──────────────┴──────────────┴──────────────┴───────────────┘
                                     │
                              ┌──────▼──────┐
                              │   TRAEFIK   │
                              │ Reverse     │
                              │ Proxy + SSL │
                              └──────┬──────┘
                                     │
       ┌─────────────────────────────┼─────────────────────────────┐
       │                             │                             │
┌──────▼──────┐              ┌───────▼───────┐              ┌──────▼──────┐
│  Laravel    │              │   Django      │              │  AWS S3     │
│  API        │              │   API         │              │  Storage    │
│  (Primary)  │              │   (ML/AI)     │              │             │
└──────┬──────┘              └───────────────┘              └─────────────┘
       │
       │
┌──────▼──────┐
│  MySQL      │
│  Database    │
└─────────────┘
       │
┌──────▼──────┐
│  Redis      │
│  Cache/Queue│
└─────────────┘
```

---

## Repository Architecture

### posteat-laravel-api (Primary Backend)

**Purpose**: Core REST API powering all frontend applications

**Location**: [../posteat-laravel-api](../posteat-laravel-api)

**Architecture**:
```
posteat-laravel-api/
├── app/
│   ├── Http/
│   │   ├── Controllers/     # 74+ API controllers
│   │   └── Requests/        # Form requests & validation
│   ├── Models/              # 81+ Eloquent models
│   ├── Services/            # Business logic services
│   ├── Jobs/                # Background queue jobs
│   ├── Observers/           # Model observers
│   ├── Validations/         # Custom validation classes
│   └── Traits/              # Reusable traits
├── database/
│   ├── migrations/          # Database schema
│   └── seeders/             # Sample data seeders
├── routes/
│   ├── api.php              # API routes
│   └── web.php              # Web routes
└── config/                  # Laravel configuration
```

**Key Controllers**:
- `AuthController` - JWT authentication, social login
- `BusinessController` - Restaurant CRUD, galleries, cuisines
- `ReservationController` - Table booking management
- `ReviewController` - Review and rating system
- `OrderController` - Order management and tracking
- `UserController` - User profile management
- `NotificationController` - Push notification management

---

### posteat-react-web (Consumer Website)

**Purpose**: Public-facing website for restaurant discovery

**Location**: [../posteat-react-web](../posteat-react-web)

**Architecture**:
```
posteat-react-web/
├── src/
│   ├── pages/                # File-based routes (vite-plugin-remix-router)
│   │   ├── landing/          # Landing page
│   │   ├── business/         # Business public pages
│   │   └── auth/             # Authentication flows
│   ├── components/           # Reusable UI components
│   ├── stores/               # Redux Toolkit slices
│   ├── utils/                # Helpers (including redirect utilities)
│   ├── themes/               # Theme configuration
│   └── styles/              # Global styles
```

**Key Features**:
- Public marketing and landing pages
- Business pages with galleries, cuisines, reviews
- Auth/session flows with redirect utilities
- Deep links to mobile app

**Documentation**:
- [REDIRECT_UTILITIES_README.md](../posteat-react-web/REDIRECT_UTILITIES_README.md)
- [FIXES_SUMMARY.md](../posteat-react-web/FIXES_SUMMARY.md)

---

### posteat-vendor-web (Vendor Dashboard)

**Purpose**: Restaurant management for vendors

**Location**: [../posteat-vendor-web](../posteat-vendor-web)

**Architecture**:
```
posteat-vendor-web/
├── src/
│   ├── pages/
│   │   ├── b/               # Business-scoped pages
│   │   └── business/        # Business management
│   ├── components/          # Reusable UI components
│   ├── stores/               # Redux Toolkit slices
│   └── services/             # API service layer
```

**Key Features**:
- Order management and tracking
- Menu and inventory management
- Business analytics and reporting
- Staff management with role-based permissions
- Reservation management
- Gallery and cuisine configuration

---

### posteat-react-web-admin (Admin Dashboard)

**Purpose**: Platform-wide administration

**Location**: [../posteat-react-web-admin](../posteat-react-web-admin)

**Architecture**:
```
posteat-react-web-admin/
├── src/
│   ├── pages/               # File-based routes
│   │   ├── dashboard/       # Dashboard pages
│   │   ├── users/           # User management
│   │   ├── business/        # Business management
│   │   ├── administrators/   # Admin management
│   │   ├── roles/           # Role management
│   │   ├── permissions/     # Permission management
│   │   ├── cuisines/        # Cuisine management
│   │   ├── currencies/      # Currency management
│   │   ├── languages/       # Language management
│   │   ├── reviews/         # Review moderation
│   │   ├── reports/         # Report management
│   │   └── verifications/   # Verification management
│   ├── components/          # Reusable UI components
│   └── stores/              # Redux Toolkit slices
```

**Key Features**:
- User management (customers, vendors, admins)
- Business verification and management
- Content moderation (reviews, reports)
- System configuration (currencies, languages, cuisines)
- Analytics and reporting
- Permission and role management

---

### posteat-react-native-mobile (Mobile App)

**Purpose**: Customer-facing mobile application

**Location**: [../posteat-react-native-mobile](../posteat-react-native-mobile)

**Architecture**:
```
posteat-react-native-mobile/
├── app/                     # Expo app configuration
├── components/              # Reusable UI components
├── forms/                   # Form schemas
├── hooks/                   # Custom hooks
├── providers/               # Context providers
├── stores/                  # Redux Toolkit slices
├── themes/                  # Theme configuration
├── translations/            # i18n translations
├── utils/                   # Utilities
├── assets/                  # Images, fonts
└── languages/               # Language files
```

**Key Features**:
- Restaurant discovery
- Order placement
- Review submission
- Push notifications
- Multi-language support

---

### posteat-django-api (ML/AI Services)

**Purpose**: Machine learning and AI services

**Location**: [../posteat-django-api](../posteat-django-api)

**Architecture**:
```
posteat-django-api/
├── apps/
│   └── posteat_django_api/  # Main Django app
│       ├── ml/              # ML models and services
│       └── api/             # REST API endpoints
├── media/                   # File uploads
├── manage.py                # Django management script
└── requirements.txt         # Python dependencies
```

**Key Features**:
- Review authenticity detection
- Receipt processing
- AI-powered recommendations

---

## API Documentation

### Postman Collections

**Primary API (Laravel)**:
- **Collection URL**: https://documenter.getpostman.com/view/35257821/2sAY4sk5Mb
- **Postman Workspace**: Contains all endpoints for:
  - Authentication
  - Business management
  - Reservations
  - Orders
  - Reviews
  - Users
  - Notifications

**Mobile API (Separate)**:
- **Collection URL**: https://documenter.getpostman.com/view/41814866/2sAYX6p22q
- **Alternative**: https://documenter.getpostman.com/view/14257206/2sAYX3q3QN

### API Base URLs

| Environment | Base URL |
|-------------|----------|
| Production | `https://api.posteat.co.uk/v1` |
| Development | `https://dev-api.posteat.co.uk` |

### Key API Endpoints

#### Authentication
- `POST /auth/login` - JWT login
- `POST /auth/register` - User registration
- `POST /auth/social` - Firebase social login
- `POST /auth/refresh` - Token refresh
- `POST /auth/logout` - Logout

#### Businesses
- `GET /businesses` - List businesses
- `GET /businesses/{uuid}` - Get business details
- `POST /businesses` - Create business
- `PUT /businesses/{uuid}` - Update business
- `DELETE /businesses/{uuid}` - Delete business

#### Reservations
- `GET /reservations` - List reservations
- `POST /reservations` - Create reservation
- `PUT /reservations/{uuid}` - Update reservation
- `DELETE /reservations/{uuid}` - Cancel reservation

#### Orders
- `GET /orders` - List orders
- `GET /orders/{uuid}` - Get order details
- `POST /orders` - Create order
- `PUT /orders/{uuid}` - Update order status

#### Reviews
- `GET /reviews` - List reviews
- `POST /reviews` - Submit review
- `PUT /reviews/{uuid}` - Update review
- `POST /reviews/{uuid}/report` - Report review

---

## Database Schema

### Main Database (Laravel API - MySQL/PostgreSQL)

**Key Tables**:

| Table | Purpose |
|-------|---------|
| `users` | User accounts with Firebase auth |
| `businesses` | Restaurant/business profiles |
| `business_hours` | Operating hours |
| `business_cuisines` | Cuisine associations |
| `tables` | Restaurant tables |
| `reservations` | Booking records |
| `orders` | Order records |
| `order_items` | Items within orders |
| `reviews` | User reviews and ratings |
| `media` | File uploads (Spatie Media Library) |
| `notifications` | Push notification records |
| `permissions` | Spatie permissions |
| `roles` | Spatie roles |
| `model_has_permissions` | Permission assignments |
| `model_has_roles` | Role assignments |

### Django API Database

Uses SQLite by default for ML-related data:
- `Review` - Processed reviews
- `Receipt` - Receipt data for training
- ML model cache tables

---

## Core Business Logic

### KPIs (Key Performance Indicators)

Tracked in the Laravel API:
- **Business Metrics**: Orders per day, revenue, average order value
- **User Metrics**: Signups, retention, engagement
- **Review Metrics**: Average ratings, review count

### Recommendation Logic

The Django API provides ML-powered recommendations:
1. User location-based suggestions
2. Cuisine preference matching
3. Review authenticity scoring
4. Similar business recommendations

### Sustainability Metrics

- Food waste tracking via receipt uploads
- Business sustainability scores
- Environmental impact estimates

### Expiry Logic

- Order expiration: Orders auto-expire if not confirmed within configured time
- Review window: Users can leave reviews within X days of order completion
- Reservation expiry: Unconfirmed reservations released after grace period

### Receipt Processing (AI)

The Django API handles:
1. Receipt image upload
2. OCR text extraction
3. AI-based data categorization
4. Business receipt training stats

### Content Moderation

Reviews go through moderation:
1. AI authenticity scoring
2. Flagged content review
3. Manual approval for low-score reviews

---

## State Management

### Redux Toolkit Structure

All React projects follow this pattern:

```
stores/
├── entity_name/
│   ├── index.ts        # Store configuration
│   ├── types.ts        # TypeScript types
│   ├── slice.ts        # Redux slice
│   └── hooks.ts        # Custom hooks
└── api/                # RTK Query endpoints
```

### API Hook Pattern

```typescript
export function useEntityApi(config?: Config) {
  const [readState, setReadState] = useState(initialState);
  return {
    readState,
    _CreateEntity,
    _ReadEntity,
    _UpdateEntity,
    _DeleteEntity,
  };
}
```

---

## Error Handling

### Frontend
- `resolveApiError` utility for API errors
- `Promise.allSettled` for sequential calls
- User-friendly error messages

### Backend
- `ValidationException` for validation errors
- `try/catch` with database transactions
- Structured JSON error responses

---

## Related Documentation

- [Source Code & Repositories](./01-source-code-repositories.md)
- [Infrastructure & DevOps](./03-infrastructure-devops.md)
- [Laravel API README](../posteat-laravel-api/README.md)
- [posteat-laravel-api/AGENTS.md](../posteat-laravel-api/AGENTS.md)
- [posteat-react-web/AGENTS.md](../posteat-react-web/AGENTS.md)
