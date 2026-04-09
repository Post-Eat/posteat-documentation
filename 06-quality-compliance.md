# 6. Quality & Compliance

## Test Suite Documentation

### Laravel API Tests

**Location**: [../posteat-laravel-api/tests/](../posteat-laravel-api/tests/)

**Framework**: PHPUnit

**Running Tests**:
```bash
# Run all tests
php artisan test

# Run specific test class
php artisan test --filter=TestClassName

# Run specific test method
php artisan test --filter=testMethodName

# Run with coverage
php artisan test --coverage
```

**Test Structure**:
```
tests/
├── Feature/              # Integration tests
│   ├── AuthTest.php
│   ├── BusinessTest.php
│   └── ...
└── Unit/                # Unit tests
    ├── Services/
    └── Models/
```

**Key Test Files**:
- `Feature/AuthTest.php` - Authentication tests
- `Feature/BusinessTest.php` - Business CRUD tests
- `Feature/ReservationTest.php` - Reservation flow tests

### Django API Tests

**Location**: [../posteat-django-api/](../posteat-django-api/)

**Framework**: Django TestCase

**Running Tests**:
```bash
python manage.py test
python manage.py test app_name
```

### React Frontend Tests

**Status**: Test suite not fully configured for most React projects

**If tests exist**:
```bash
# Run all tests
yarn test

# Watch mode
yarn test --watch

# Single test file
yarn test -- -t "test name"
```

---

## Code Quality

### PHP/Laravel - Laravel Pint

**Formatter**: Laravel Pint (built on PHP-CS-Fixer)

```bash
# Format all files
./vendor/bin/pint

# Check without modifying
./vendor/bin/pint --test
```

### JavaScript/TypeScript - ESLint

**Configuration**: `.eslintrc` in each project

**Running Lint**:
```bash
# React projects
yarn lint

# Fix auto-fixable issues
yarn lint --fix
```

**Key Rules**:
- Strict TypeScript mode
- No implicit `any`
- Consistent import order

### Pre-commit Hooks

Some projects may have pre-commit hooks configured via Husky:

```bash
# Install hooks (if available)
yarn prepare
```

---

## Code Review Process

### Pull Request Requirements

1. **Description**: Clear explanation of changes
2. **Testing**: Must test locally before PR
3. **Lint**: Must pass ESLint/Pint
4. **CI**: All checks must pass

### Review Checklist

- [ ] Code follows project conventions
- [ ] No hardcoded secrets
- [ ] Proper error handling
- [ ] TypeScript types are explicit
- [ ] Tests added/updated if needed
- [ ] Documentation updated if needed

---

## GDPR & Privacy Documentation

### Data Storage

#### User Personal Data

| Data Type | Storage | Retention |
|-----------|---------|-----------|
| Email | MySQL `users` table | Until account deletion |
| Name | MySQL `users` table | Until account deletion |
| Phone | MySQL `users` table | Until account deletion |
| Profile Photo | AWS S3 | Until account deletion |
| Address | MySQL `users` table | Until account deletion |

#### Business Data

| Data Type | Storage | Retention |
|-----------|---------|-----------|
| Business Info | MySQL `businesses` table | Until business deletion |
| Bank Account | MySQL (encrypted) | Until business deletion |
| Documents | AWS S3 | Until business deletion |

### Data Processing

#### Laravel API

1. **Collection**: Data collected via API endpoints
2. **Storage**: Stored in MySQL database
3. **Processing**: Used for platform operations
4. **Retention**: Kept until user/business deletion
5. **Deletion**: Soft delete with hard delete after grace period

#### Third-Party Processors

| Service | Data Shared | Purpose |
|---------|-------------|---------|
| Firebase | Email, Name (social login) | Authentication |
| AWS S3 | Media files | Storage |
| OpenAI | Review text | ML processing |

### User Rights

Users can:
- **Access**: Request their data
- **Rectification**: Update their profile
- **Erasure**: Delete their account
- **Portability**: Export their data

### Implementation

```php
// User data export
public function exportUserData(User $user)
{
    return [
        'profile' => $user->toArray(),
        'orders' => $user->orders->toArray(),
        'reservations' => $user->reservations->toArray(),
        'reviews' => $user->reviews->toArray(),
    ];
}

// Soft delete user (GDPR)
public function deleteAccount(Request $request)
{
    $user->delete(); // Soft delete
    
    // Schedule hard delete after grace period
    dispatch(new HardDeleteUserJob($user))->delay(now()->addDays(30));
}
```

### Security Measures

- Passwords hashed with bcrypt
- API responses don't expose sensitive fields
- S3 URLs are signed/temporary
- Rate limiting on data access endpoints
- Audit logging for sensitive operations

---

## Compliance Checklist

### Technical

- [x] HTTPS on all endpoints
- [x] Password hashing (bcrypt)
- [x] JWT token expiration
- [x] Input validation
- [x] SQL injection prevention
- [x] XSS protection
- [x] CSRF tokens

### Operational

- [ ] Regular security audits
- [ ] Backup verification
- [ ] Incident response plan
- [ ] Data retention policy documentation

### Documentation

- [x] Privacy policy
- [x] Terms of service
- [ ] Data processing agreements
- [ ] Cookie policy

---

## Performance & Optimization

### Laravel API

**Caching**: Redis (optional)
**Queue**: Laravel Queue for background jobs
**Optimization**: Eager loading for relationships

### React Frontends

**Code Splitting**: Via Vite
**Lazy Loading**: React.lazy() for routes
**Memoization**: useMemo, useCallback

---

## Related Documentation

- [Architecture & Business Logic](./02-architecture-business-logic.md)
- [Credentials & Security](./04-credentials-security.md)
- [Laravel AGENTS.md](../posteat-laravel-api/AGENTS.md)
- [React Web AGENTS.md](../posteat-react-web/AGENTS.md)
