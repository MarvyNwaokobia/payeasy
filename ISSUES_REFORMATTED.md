# PayEasy GitHub Issues (50 Total) - Reformatted Template

## Phase 1: Project Setup & Infrastructure (Issues 1-8)

---

## Issue 1: Setup ESLint & TypeScript Configuration for Web App

**Labels:** `setup`, `type:task`, `complexity:MEDIUM` | **Points:** 150

### 📝 Description
Establish strict TypeScript configuration and ESLint rules to maintain code quality across the project. A well-configured linter catches errors early and ensures consistent code patterns.

### Context
Without a linter, code quality degrades as the project grows. Team members write code inconsistently. TypeScript's strict mode prevents many classes of runtime errors. Together, these tools form the foundation of a maintainable codebase.

### Tech Stack
- ESLint 8+ with Next.js config
- TypeScript 5+ with strict mode
- Husky for git hooks
- Prettier for formatting

### ✅ Requirements
1. Create `.eslintrc.json` with Next.js + TypeScript rules
2. Configure `tsconfig.json` with strict mode
3. Setup Husky pre-commit hooks
4. Add `npm run lint` script
5. Document standards in CONTRIBUTING.md
6. Ensure all code passes linting
7. Configure IDE ESLint integration
8. Prevent commits with lint errors

### 🎯 Acceptance Criteria
- `npm run lint` runs with 0 errors
- TypeScript strict mode enforced
- Pre-commit hook blocks bad commits
- All existing code passes lint
- No `any` types allowed
- Configuration extends `next/core-web-vitals`
- Team members can use IDE auto-fix
- ESLint config is well-documented

### 📁 Expected Files to Change/Create
```
.eslintrc.json                  (new)
tsconfig.json                   (modify)
.husky/pre-commit               (new)
package.json                    (add script)
CONTRIBUTING.md                 (add section)
```

---

## Issue 2: Configure Prettier Code Formatter with Project Standards

**Labels:** `setup`, `type:task`, `complexity:MEDIUM` | **Points:** 150

### 📝 Description
Establish unified code formatting using Prettier. This eliminates bikeshedding over code style and ensures every file follows the same visual patterns.

### Context
Manual formatting wastes developer time. Prettier automates this with sensible defaults. When combined with ESLint, it eliminates tool conflicts. Developers focus on logic, not formatting.

### Tech Stack
- Prettier 3+
- prettier-plugin-tailwindcss
- eslint-config-prettier
- Tailwind CSS class ordering

### ✅ Requirements
1. Create `.prettierrc.json` with shared config
2. Create `.prettierignore` for exclusions
3. Add Tailwind plugin for class ordering
4. Integrate with ESLint
5. Add `npm run format` script
6. Setup VS Code auto-format
7. Add pre-commit prettier hook
8. Format all existing code

### 🎯 Acceptance Criteria
- All code is formatted uniformly
- `npm run format` works without errors
- Prettier and ESLint don't conflict
- Tailwind classes ordered consistently
- VS Code formats on save (optional)
- All files properly formatted
- `.env*`, `node_modules/`, `.next/` ignored
- No formatting conflicts in pre-commit

### 📁 Expected Files to Change/Create
```
.prettierrc.json                (new)
.prettierignore                 (new)
package.json                    (add script)
.husky/pre-commit               (update)
.vscode/settings.json           (configure)
```

---

## Issue 3: Setup PostgreSQL Database Schema with Supabase

**Labels:** `database`, `type:backend`, `complexity:HIGH` | **Points:** 200

### 📝 Description
Design and implement the complete PostgreSQL schema. This is foundational - a well-designed schema prevents data anomalies, enables complex queries, and supports growth.

### Context
The database stores:
- User profiles linked to Stellar wallets
- Apartment listings with rich metadata
- Real-time messages between users
- Payment records for blockchain transactions
- Rent agreements and contract tracking

Proper indexes, constraints, and RLS policies ensure data integrity and security at the database level.

### Tech Stack
- PostgreSQL 13+ (Supabase hosted)
- UUID for primary keys
- PostGIS for location queries
- Row Level Security (RLS)
- Foreign key constraints
- Database views

### ✅ Requirements
1. Create migration files for all tables
2. Define 7 tables: users, listings, messages, payment_records, rent_agreements, listing_amenities, listing_images
3. Add indexes on foreign keys and frequently queried fields
4. Configure RLS policies for data isolation
5. Add foreign key constraints with cascades
6. Create views for common query patterns
7. Add CHECK constraints for validation
8. Write schema documentation

### 🎯 Acceptance Criteria
- All tables created with correct schema
- Indexes on (landlord_id), (user_id), (conversation_id), etc.
- Foreign keys prevent data inconsistency
- RLS restricts users to own data
- Migrations run cleanly
- Schema documented
- Queries execute in <100ms
- No circular dependencies

### 📁 Expected Files to Change/Create
```
supabase/migrations/
  001_create_users.sql
  002_create_listings.sql
  003_create_messages.sql
  004_create_payments.sql
  005_create_agreements.sql
  006_create_indexes.sql
  007_setup_rls.sql
docs/DATABASE_SCHEMA.md
```

---

## Issue 4: Setup Environment Variables & Configuration Management

**Labels:** `setup`, `type:task`, `complexity:MEDIUM` | **Points:** 150

### 📝 Description
Create secure environment configuration for dev, staging, and production. Environment variables keep secrets out of source code while allowing per-environment customization.

### Context
Different environments need different settings:
- Database URLs
- API key and secrets
- Feature flags
- Stellar network (testnet vs mainnet)
- Debug settings

Using env vars keeps git clean and secrets safe.

### Tech Stack
- Next.js env system
- Zod for validation
- dotenv for local dev
- GitHub Secrets for CI/CD

### ✅ Requirements
1. Create `.env.local.example` (no real values)
2. Create `.env.example` for docs
3. Setup Zod validation utility
4. Document all variables
5. Configure dev/staging/prod separately
6. Ensure secrets aren't logged
7. Add `.env.local` to `.gitignore`
8. Validate at startup

### 🎯 Acceptance Criteria
- `.env.local` is git-ignored
- `.env.example` documents all vars
- Zod catches missing/invalid vars at startup
- Tests work with mock env vars
- No secrets in git history
- Error messages guide developers
- CI/CD properly configured
- Dev/staging/prod are distinct

### 📁 Expected Files to Change/Create
```
.env.local                      (git-ignored)
.env.example                    (committed)
lib/env.ts                      (Zod validation)
docs/ENVIRONMENT.md
```

---

## Issue 5: Setup Supabase Client & Connection Pool

**Labels:** `backend`, `type:setup`, `complexity:MEDIUM` | **Points:** 150

### 📝 Description
Initialize Supabase client libraries for server and client usage. The Supabase client is the gateway to the database with proper isolation and error handling.

### Context
We need:
- Client-side access (with RLS enforcement)
- Server-side access (with service role)
- Singleton pattern (avoid multiple connections)
- Error handling for network failures
- Proper type safety

### Tech Stack
- @supabase/supabase-js
- Supabase CLI
- Connection pooling
- TypeScript types

### ✅ Requirements
1. Install `@supabase/supabase-js`
2. Create `lib/supabase/client.ts` (browser)
3. Create `lib/supabase/server.ts` (server)
4. Implement singleton pattern
5. Add error handling wrappers
6. Setup retry logic
7. Create helper hooks
8. Add TypeScript types

### 🎯 Acceptance Criteria
- Client enforces RLS properly
- Server client has elevated permissions
- Connections are reused (singleton)
- Errors handled gracefully
- Types provided for all operations
- Testing mockable
- Realtime subscriptions work
- No memory leaks from subscriptions

### 📁 Expected Files to Change/Create
```
lib/supabase/client.ts
lib/supabase/server.ts
lib/supabase/types.ts
hooks/useSupabase.ts
hooks/useAuth.ts
```

---

## Issue 6: Setup TypeScript API Types for Database Models

**Labels:** `backend`, `type:task`, `complexity:MEDIUM` | **Points:** 150

### 📝 Description
Create TypeScript types for all database models. Types catch bugs at compile time, improve IDE support, and serve as documentation.

### Context
Without types, API responses are loosely typed. TypeScript types:
- Catch errors at compile time
- Provide IDE autocomplete
- Document data structure
- Enable validation with Zod

### Tech Stack
- TypeScript 5+
- Zod for validation
- Type inference
- Utility types (Pick, Partial, Omit)

### ✅ Requirements
1. Create `types/database.ts` with model types
2. Define User, Listing, Message, Payment, Agreement types
3. Create base and extended types
4. Add Zod validation schemas
5. Create create/update operation types
6. Document type conventions
7. Type all database columns
8. Export for app-wide use

### 🎯 Acceptance Criteria
- All models have types
- No `any` types in definitions
- Zod validates API payloads
- Types are reusable
- IDE autocomplete works
- Type errors prevent bugs
- Nested data properly typed
- Update operations distinct from create

### 📁 Expected Files to Change/Create
```
types/database.ts
types/api.ts
types/validation.ts
lib/validation/schemas.ts
```

---

## Issue 7: Setup API Route Structure & Middleware

**Labels:** `backend`, `type:task`, `complexity:MEDIUM` | **Points:** 150

### 📝 Description
Create organized API route structure with middleware. Clean API organization makes the codebase maintainable and scalable.

### Context
Well-organized routes:
- Group by domain (auth, listings, messages)
- Centralized error handling
- Request logging with IDs
- Auth verification
- Consistent response format

### Tech Stack
- Next.js 14 App Router
- Middleware for processing
- Custom error classes
- Zod validation
- Winston logging

### ✅ Requirements
1. Create `/app/api` directory structure
2. Create request logging middleware
3. Create error handling middleware
4. Create auth verification middleware
5. Setup response wrapper format
6. Create error boundary
7. Add rate limiting
8. Document conventions

### 🎯 Acceptance Criteria
- Routes follow conventions
- Requests have unique IDs
- Errors standardized
- Auth verified in protected routes
- Logging works
- Rate limiting prevents abuse
- Errors helpful for developers
- Routes testable with mocks

### 📁 Expected Files to Change/Create
```
app/api/middleware.ts
app/api/utils/response.ts
app/api/utils/errors.ts
app/api/utils/auth.ts
app/api/utils/logger.ts
```

---

## Issue 8: Setup GitHub Actions for CI/CD Pipeline

**Labels:** `devops`, `type:task`, `complexity:MEDIUM` | **Points:** 150

### 📝 Description
Create automated CI/CD workflows. CI/CD catches errors early and enforces quality before code reaches main branch.

### Context
Automated workflows:
- Lint on every PR
- Build verification
- Run tests
- Deploy automatically
- Provide feedback

### Tech Stack
- GitHub Actions
- Node.js runtime
- ESLint
- Jest/testing
- Codecov

### ✅ Requirements
1. Create `.github/workflows/lint.yml`
2. Create `.github/workflows/build.yml`
3. Create `.github/workflows/test.yml`
4. Create `.github/workflows/deploy.yml` (optional)
5. Setup branch protection rules
6. Configure required status checks
7. Add codecov for tracking
8. Document process

### 🎯 Acceptance Criteria
- Lint workflow passes
- Build successful
- Tests run and report coverage
- Status checks required before merge
- Workflows complete in <15 minutes
- Deployment works
- Failures notify team
- Coverage tracked

### 📁 Expected Files to Change/Create
```
.github/workflows/lint.yml
.github/workflows/build.yml
.github/workflows/test.yml
.github/workflows/deploy.yml
```

---

## Phase 2: Authentication & Wallet Integration (Issues 9-15)

---

## Issue 9: Implement Stellar Wallet Detection & Connection

**Labels:** `auth`, `blockchain`, `type:feature`, `complexity:HIGH` | **Points:** 200

### 📝 Description
Implement Freighter wallet detection and connection. This is the entry point for blockchain authentication.

### Context
Users authenticate with Stellar wallets via Freighter. We need to:
- Detect if Freighter is installed
- Request wallet connection
- Get public key
- Handle errors
- Persist state

### Tech Stack
- @stellar/freighter-api
- React Context
- localStorage
- TypeScript

### ✅ Requirements
1. Create `lib/stellar/freighter.ts`
2. Detect wallet on app load
3. Create connection UI component
4. Handle wallet switching
5. Store state in Context
6. Persist in localStorage
7. Handle Freighter not installed error
8. Add retry logic

### 🎯 Acceptance Criteria
- Freighter detection works
- Users can connect wallet
- Connection persists after refresh
- Users can switch wallets
- Clear error if Freighter missing
- Public key retrieved correctly
- State available app-wide
- Works testnet & mainnet

### 📁 Expected Files to Change/Create
```
lib/stellar/freighter.ts
lib/stellar/types.ts
components/WalletConnectButton.tsx
components/WalletStatus.tsx
```

---

## Issue 10: Implement Wallet Signature Verification for Login

**Labels:** `auth`, `type:feature`, `security`, `complexity:HIGH` | **Points:** 200

### 📝 Description
Create signature-based login using wallet ownership verification. Non-custodial authentication proves wallet ownership without storing keys.

### Context
Login flow:
1. Request challenge with public key
2. Server generates nonce
3. User signs with wallet
4. Server verifies signature
5. Issue JWT token

Proves ownership without key sharing.

### Tech Stack
- stellar-sdk for verification
- jsonwebtoken for JWTs
- Freighter for signing
- HTTP-only cookies
- Timestamp protection

### ✅ Requirements
1. Create `/api/auth/login` endpoint
2. Create signature verification endpoint
3. Generate cryptographic nonce
4. Implement timestamp validation (5-min window)
5. Generate JWT on success
6. Store JWT in secure HTTP-only cookie
7. Create login UI component
8. Handle signing cancellation

### 🎯 Acceptance Criteria
- Users login by signing challenge
- JWT issued on success
- Token in HTTP-only Secure cookie
- Replay attacks prevented
- Clear error on invalid signature
- Verification server-side
- Cannot forge signatures
- Login < 30 seconds

### 📁 Expected Files to Change/Create
```
app/api/auth/login/route.ts
app/api/auth/verify/route.ts
lib/auth/jwt.ts
lib/auth/signatures.ts
pages/(auth)/login/page.tsx
```

---

*(Continue with Issues 11-50 following same format...)*

## Issue 11: Create User Registration Flow with Wallet

**Labels:** `auth`, `type:feature`, `complexity:MEDIUM` | **Points:** 150

### 📝 Description
Implement user registration creating profiles linked to Stellar wallets. New users must create a profile before accessing the platform.

### Context
Registration:
1. Connect wallet
2. Enter username/email/bio
3. Submit to API
4. Create in database
5. Auto-login

### Tech Stack
- React Hook Form
- Zod validation
- Supabase
- Next.js API routes

### ✅ Requirements
1. Create registration endpoint
2. Create form component
3. Validate unique username (3-20 chars, alphanumeric)
4. Validate unique email
5. Create user profile
6. Auto-login after registration
7. Handle existing wallet
8. Send verification email (optional)

### 🎯 Acceptance Criteria
- Users can register
- Duplicate check works
- Form validation clear
- User logged in after registration
- Data stored correctly
- Registration < 10 seconds
- Mobile responsive
- Accessibility met

### 📁 Expected Files to Change/Create
```
app/api/auth/register/route.ts
pages/(auth)/register/page.tsx
components/RegistrationForm.tsx
```

---

## [Continuing Issues 12-50...]

### Issue 12-15: Auth Context, Protected Routes, Logout, UI Pages
### Issue 16-22: Profiles, Edit, Avatars, Dashboard, Stats, Notifications
### Issue 23-30: Listings, CRUD, Images, Browse, Search, Filters
### Issue 31-35: Maps, Favorites, Caching, Sorting, History
### Issue 36-40: Messages, Chat, Realtime
### Issue 41-45: Smart Contracts, Testing, Deployment, Integration
### Issue 46-50: Payments, Status Tracking, Fees, Receipts, Admin

---

## Summary

**50 Issues Total**
- **Phase 1 (8):** Infrastructure & Setup
- **Phase 2 (7):** Authentication
- **Phase 3 (7):** User Profiles  
- **Phase 4 (8):** Listings
- **Phase 5 (5):** Search & Discovery
- **Phase 6 (5):** Messaging
- **Phase 7 (5):** Smart Contracts
- **Phase 8 (5):** Payments

**Point Distribution**
- MEDIUM (150 pts): 36 issues
- HIGH (200 pts): 14 issues
- **Total: 5,400 points**

---

## Next Steps

1. Review all issues and adjust as needed
2. Run the issue creation script: `python scripts/create-issues.py`
3. Issues will be created on GitHub with proper formatting
4. Create GitHub Project board to track progress
5. Assign issues to contributors based on complexity
