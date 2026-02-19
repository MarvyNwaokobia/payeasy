# PayEasy GitHub Issues (50 Total) - New Template Format

## Phase 1: Project Setup & Infrastructure (Issues 1-8)

---

## Issue 1: Setup ESLint & TypeScript Configuration for Web App

**Labels:** `setup`, `type:task`, `complexity:MEDIUM`  
**Points:** 150

**Description:**
The web app needs strict TypeScript configuration and ESLint rules to maintain code quality across the project. Currently, there's minimal linting configuration.

**What needs to be done:**
1. Create `.eslintrc.json` with recommended Next.js + TypeScript rules
2. Configure `tsconfig.json` with strict mode enabled
3. Add pre-commit hooks using Husky to enforce linting
4. Document linting standards in CONTRIBUTING.md
5. Ensure all existing code passes linting

**Resolution Tips:**
- Use Next.js ESLint config as base: `npm install --save-dev eslint-config-next`
- Enable `strict: true` in tsconfig.json for maximum type safety
- Install Husky with: `npm install husky --save-dev && npx husky install`
- Add pre-commit hook: `npx husky add .husky/pre-commit "npm run lint"`
- Reference: [Next.js ESLint Documentation](https://nextjs.org/docs/basic-features/eslint)

---

### Issue 2: Configure Prettier Code Formatter with Project Standards
**Labels:** `setup`, `type:task`  
**Complexity:** MEDIUM (150 points)

**Description:**
Establish consistent code formatting across the entire project for readability and developer experience.

**What needs to be done:**
1. Create `.prettierrc.json` with formatting standards
2. Create `.prettierignore` for build artifacts and dependencies
3. Configure Prettier with Tailwind CSS plugin support
4. Integrate Prettier with ESLint (using `eslint-config-prettier`)
5. Add format script to package.json

**Resolution Tips:**
- Config should include: `"printWidth": 100`, `"tabWidth": 2`, `"trailingComma": "es5"`
- Install: `npm install --save-dev prettier eslint-config-prettier prettier-plugin-tailwindcss`
- Add script: `"format": "prettier --write ."`
- Add to ESLint: `"extends": ["next/core-web-vitals", "prettier"]`
- Create `.prettierignore`: Add `node_modules/`, `build/`, `.next/`, `dist/`

---

### Issue 3: Setup PostgreSQL Database Schema with Supabase
**Labels:** `database`, `type:backend`, `HIGH`  
**Complexity:** HIGH (200 points)

**Description:**
Design and implement the complete PostgreSQL schema for user profiles, listings, messages, and payment records. This is foundational for all data persistence.

**What needs to be done:**
1. Create initial migration files for Supabase
2. Define tables: `users`, `listings`, `messages`, `payment_records`, `rent_agreements`
3. Setup proper indexes for frequently queried fields
4. Configure Row Level Security (RLS) policies
5. Create helper documentation for database schema

**Tables to create:**
```sql
users (id, public_key, username, email, avatar_url, bio, created_at, updated_at)
listings (id, landlord_id, title, description, address, rent_xlm, bedrooms, bathrooms, created_at, updated_at)
messages (id, sender_id, receiver_id, listing_id, content, created_at)
payment_records (id, user_id, listing_id, amount_paid, transaction_hash, status, created_at)
rent_agreements (id, listing_id, contract_id, status, created_at)
```

**Resolution Tips:**
- Use Supabase Dashboard or CLI to create migrations
- Set primary keys as UUID with `gen_random_uuid()`
- Create indexes: `CREATE INDEX idx_listings_landlord_id ON listings(landlord_id);`
- Use RLS policies: `ALTER TABLE users ENABLE ROW LEVEL SECURITY;`
- Test with: `supabase start` locally before pushing
- Reference: [Supabase Database Guide](https://supabase.com/docs/guides/database)

---

### Issue 4: Setup Environment Variables & Configuration Management
**Labels:** `setup`, `type:task`  
**Complexity:** MEDIUM (150 points)

**Description:**
Create secure environment configuration for both development and production environments.

**What needs to be done:**
1. Create `.env.local.example` with all required variables
2. Create `.env.example` for documentation
3. Setup environment validation utility
4. Document all environment variables
5. Create env loading in Next.js config if needed

**Required Environment Variables:**
```
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
NEXT_PUBLIC_STELLAR_NETWORK=testnet
NEXT_PUBLIC_FREIGHTER_NETWORK=testnet
DATABASE_URL=
NODE_ENV=development
```

**Resolution Tips:**
- Create `lib/env.ts` with Zod validation for environment variables
- Use `process.env` for server-side and `process.env.NEXT_PUBLIC_*` for client-side
- Add `.env.local` to `.gitignore`
- Install Zod: `npm install zod`
- Reference: [Next.js Environment Variables](https://nextjs.org/docs/basic-features/environment-variables)

---

### Issue 5: Setup Supabase Client & Connection Pool
**Labels:** `backend`, `type:setup`  
**Complexity:** MEDIUM (150 points)

**Description:**
Initialize Supabase client libraries for both server-side and client-side usage with proper error handling.

**What needs to be done:**
1. Install `@supabase/supabase-js` package
2. Create `lib/supabase/client.ts` for client-side usage
3. Create `lib/supabase/server.ts` for server-side usage (with service role key)
4. Add connection pool configuration
5. Create helper hooks for common Supabase operations

**Resolution Tips:**
- Install: `npm install @supabase/supabase-js`
- Create client singleton to avoid multiple connections
- Use environment variables for URL and keys
- Implement error handling wrapper around Supabase calls
- Example client:
```typescript
import { createClient } from '@supabase/supabase-js'
const supabase = createClient(process.env.NEXT_PUBLIC_SUPABASE_URL!, process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!)
```
- Reference: [Supabase JS Client](https://supabase.com/docs/reference/javascript/introduction)

---

### Issue 6: Setup TypeScript API Types for Database Models
**Labels:** `backend`, `type:task`  
**Complexity:** MEDIUM (150 points)

**Description:**
Create TypeScript type definitions for all database models to ensure type safety across the application.

**What needs to be done:**
1. Create `types/database.ts` with all table types
2. Define interfaces for User, Listing, Message, PaymentRecord, RentAgreement
3. Create helper types for API responses
4. Add strict validation types
5. Document type conventions

**Example Types:**
```typescript
export interface User {
  id: string
  public_key: string
  username: string
  email?: string
  avatar_url?: string
  bio?: string
  created_at: string
  updated_at: string
}

export interface Listing {
  id: string
  landlord_id: string
  title: string
  description: string
  address: string
  rent_xlm: number
  bedrooms: number
  bathrooms: number
  created_at: string
  updated_at: string
}
```

**Resolution Tips:**
- Use `Z.infer` with Zod for shared runtime + type validation
- Keep types co-located in feature folders when possible
- Use `Partial<T>` for update operations
- Use `Pick<T, K>` for restricted fields in responses
- Reference: [TypeScript Handbook - Types](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html)

---

### Issue 7: Setup API Route Structure & Middleware
**Labels:** `backend`, `type:task`  
**Complexity:** MEDIUM (150 points)

**Description:**
Create the API route structure with middleware for authentication, error handling, and logging.

**What needs to be done:**
1. Create `/api` directory structure with clear organization
2. Create middleware for request logging
3. Create error handling middleware
4. Create authentication middleware
5. Setup API response wrapper for consistency

**Directory Structure:**
```
app/api/
├── auth/
│   ├── login/
│   ├── logout/
│   └── verify/
├── users/
│   ├── [id]/
│   └── profile/
├── listings/
├── messages/
├── middleware.ts
└── utils/
    ├── response.ts
    ├── errors.ts
    └── auth.ts
```

**Resolution Tips:**
- Create middleware for Next.js: `middleware.ts` at root level
- Use consistent response format: `{ success: boolean, data: T, error?: string }`
- Implement error handling with custom error classes
- Add request ID for logging: `const requestId = crypto.randomUUID()`
- Reference: [Next.js API Routes](https://nextjs.org/docs/app/building-your-application/routing/route-handlers)

---

### Issue 8: Setup GitHub Actions for CI/CD Pipeline
**Labels:** `devops`, `type:task`  
**Complexity:** MEDIUM (150 points)

**Description:**
Create automated testing and deployment workflows to ensure code quality on every push and PR.

**What needs to be done:**
1. Create `.github/workflows/lint.yml` for linting
2. Create `.github/workflows/build.yml` for build verification
3. Create `.github/workflows/test.yml` for running tests
4. Setup branch protection rules
5. Document CI/CD process

**Resolution Tips:**
- Use GitHub Actions with Node.js setup action
- Run: `npm run lint`, `npm run build`, `npm run test`
- Require status checks to pass before merge
- Setup codecov for coverage reports
- Reference: [GitHub Actions Documentation](https://docs.github.com/en/actions/quickstart)

---

## Phase 2: Authentication & Wallet Integration (Issues 9-15)

---

### Issue 9: Implement Stellar Wallet Detection & Connection
**Labels:** `auth`, `blockchain`, `type:feature`  
**Complexity:** HIGH (200 points)

**Description:**
Implement wallet detection using Freighter browser extension as the primary authentication method. This is the entry point for all users.

**What needs to be done:**
1. Create `lib/stellar/freighter.ts` with connection logic
2. Implement wallet detection on app load
3. Create connection UI component
4. Handle wallet switching/disconnection
5. Store connected wallet state in context/provider

**Resolution Tips:**
- Install: `npm install @stellar/freighter-api`
- Check Freighter availability: 
```typescript
import { isConnected } from '@stellar/freighter-api'
const connected = await isConnected()
```
- Get user public key: `const publicKey = await getUserPublicKey()`
- Create React Context for wallet state to avoid prop drilling
- Test with Freighter testnet setup
- Reference: [Freighter API Documentation](https://developers.stellar.org/docs/tools-and-sdks/freighter-sdk/reference)

---

### Issue 10: Implement Wallet Signature Verification for Login
**Labels:** `auth`, `type:feature`, `security`  
**Complexity:** HIGH (200 points)

**Description:**
Create a secure login flow using Stellar signatures to verify wallet ownership without storing private keys.

**What needs to be done:**
1. Create login challenge endpoint (`/api/auth/login`)
2. Create verify signature endpoint
3. Generate JWT tokens on successful verification
4. Store JWT in secure HTTP-only cookies
5. Create login UI component with sign flow

**Challenge Flow:**
1. Frontend requests login challenge with public key
2. Backend generates random nonce + timestamp
3. Frontend signs message: "PayEasy Login: [nonce].[timestamp]"
4. Backend verifies signature matches public key
5. Backend issues JWT token if valid

**Resolution Tips:**
- Use `stellar-sdk`: `Keypair.master()` to verify signatures
- Generate nonce: `crypto.randomBytes(32).toString('hex')`
- Implement replay protection with timestamp (5-minute window)
- Use `jsonwebtoken` for JWT: `npm install jsonwebtoken`
- Set cookie: `Set-Cookie: auth-token=jwt; HttpOnly; Secure; SameSite=Strict`
- Reference: [Stellar Signature Verification](https://developers.stellar.org/docs/building-apps/smart-contracts/specialized-topics/signing-transactions)

---

### Issue 11: Create User Registration Flow with Wallet
**Labels:** `auth`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Implement user registration that creates a new user profile linked to their Stellar wallet public key.

**What needs to be done:**
1. Create registration endpoint (`/api/auth/register`)
2. Create registration form component
3. Validate unique username & email
4. Generate user profile in database
5. Handle existing wallet scenarios

**Registration Steps:**
1. User connects wallet via Freighter
2. User enters username and email
3. Frontend submits to `/api/auth/register` with public_key, username, email
4. Backend validates and creates user in database
5. Auto-login user after registration

**Resolution Tips:**
- Use unique constraint on public_key: `ALTER TABLE users ADD CONSTRAINT unique_public_key UNIQUE(public_key);`
- Validate email format with regex before submission
- Check username availability and format (alphanumeric, 3-20 chars)
- Return JWT token after successful registration
- Handle error when public_key already registered
- Reference: [Email validation best practices](https://www.rfc-editor.org/rfc/rfc5322)

---

### Issue 12: Create User Authentication Context/Provider
**Labels:** `auth`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Implement a central authentication context to manage user state and auth operations throughout the app.

**What needs to be done:**
1. Create `contexts/AuthContext.tsx`
2. Create `providers/AuthProvider.tsx`
3. Create `hooks/useAuth.ts` custom hook
4. Implement login, logout, register methods
5. Persist auth state with hydration

**Context Should Manage:**
```typescript
{
  user: User | null
  isLoading: boolean
  isAuthenticated: boolean
  publicKey: string | null
  login: (publicKey: string) => Promise<JWT>
  logout: () => void
  register: (data: RegisterData) => Promise<User>
}
```

**Resolution Tips:**
- Use React Context API (don't over-complicate with Redux)
- Check localStorage on mount for persistent JWT
- Validate JWT expiration on app load
- Wrap app in `<AuthProvider>` at `layout.tsx`
- Use `useAuth()` hook instead of `useContext(AuthContext)` for ergonomics
- Reference: [React Context API](https://react.dev/reference/react/useContext)

---

### Issue 13: Implement Protected Routes & Middleware
**Labels:** `auth`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Create route protection to ensure only authenticated users can access certain pages.

**What needs to be done:**
1. Create route protection middleware
2. Create protected route wrapper component
3. Implement redirects to login for unauthorized access
4. Setup auth checks before rendering protected pages
5. Add loading states during auth verification

**Protected Routes:**
- `/dashboard` (authenticated only)
- `/profile` (authenticated only)
- `/listings/create` (authenticated only)
- `/messages` (authenticated only)

**Resolution Tips:**
- Use Next.js middleware: `middleware.ts` at root
- Check JWT token validity in middleware
- Redirect to `/login` if unauthorized
- Use conditional rendering with `useAuth()` hook
- Set loading state during auth verification
- Reference: [Next.js Middleware](https://nextjs.org/docs/advanced-features/middleware)

---

### Issue 14: Add Logout & Session Cleanup
**Labels:** `auth`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Implement proper logout functionality with session cleanup and token invalidation.

**What needs to be done:**
1. Create logout endpoint (`/api/auth/logout`)
2. Implement logout button in UI
3. Clear JWT token from cookies
4. Clear user context state
5. Redirect to home page after logout

**Resolution Tips:**
- Set cookie with `Max-Age: 0` to delete: `Set-Cookie: auth-token=; Max-Age=0`
- Clear localStorage if using it for auth
- Reset AuthContext state to null
- Invalidate token on server-side (optional token blacklist)
- Redirect to homepage: `router.push('/')`
- Reference: [Cookie deletion](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie)

---

### Issue 15: Create Login & Registration Pages with Forms
**Labels:** `ui`, `auth`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Build beautiful and user-friendly login and registration pages with proper form validation and error handling.

**What needs to be done:**
1. Create `/app/(auth)/login/page.tsx`
2. Create `/app/(auth)/register/page.tsx`
3. Create reusable form components with validation
4. Implement error handling and display
5. Add loading states during submission
6. Add responsive design with Tailwind

**Form Features:**
- Wallet connection button that opens Freighter
- Username input with real-time validation
- Email input with validation
- Password optional (blockchain-based authentication suggested)
- Loading spinners during submission
- Clear error messages
- Success redirects

**Resolution Tips:**
- Use React Hook Form for better performance: `npm install react-hook-form`
- Create custom validation with Zod: `npm install zod`
- Use `lucide-react` for icons
- Add loading state to prevent multiple submissions
- Display inline validation errors
- Reference: [React Hook Form](https://react-hook-form.com/), [Zod Validation](https://zod.dev/)

---

## Phase 3: Database & User Profiles (Issues 16-22)

---

### Issue 16: Create User Profile Page Layout
**Labels:** `ui`, `profile`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Build the user profile page showing profile information, edit button, and user stats.

**What needs to be done:**
1. Create `/app/profile/page.tsx`
2. Fetch current user data from API
3. Display user info: avatar, username, bio, member since
4. Create edit profile button
5. Add responsive design
6. Handle loading and error states

**Profile Information to Display:**
- Avatar image
- Username
- Email (if verified)
- Bio/about section
- Listings count
- Member since date
- Edit Profile button
- Connected wallet (public key masked)

**Resolution Tips:**
- Use `useAuth()` hook to get current user
- Create `/api/users/[id]` endpoint to fetch user details
- Use Supabase Storage for avatar images
- Add image optimization with Next.js `Image` component
- Implement skeleton loading component
- Reference: [Next.js Image Component](https://nextjs.org/docs/app/api-reference/components/image)

---

### Issue 17: Create Edit Profile Form with Validation
**Labels:** `ui`, `profile`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Build an edit profile page allowing users to update their profile information.

**What needs to be done:**
1. Create `/app/profile/edit/page.tsx`
2. Create profile edit form component
3. Implement form validation
4. Create `/api/users/[id]` PUT endpoint
5. Handle image upload for avatar
6. Show success/error feedback

**Editable Fields:**
- Username (3-20 characters, alphanumeric)
- Email (valid email format)
- Bio (0-500 characters)
- Avatar image (JPG/PNG, max 5MB)

**Resolution Tips:**
- Use React Hook Form with Zod validation
- Implement avatar upload directly to Supabase Storage
- Preview image before upload
- Show upload progress indicator
- Validate on both client and server
- Display success toast after update
- Reference: [React Hook Form File Upload](https://react-hook-form.com/form-builder)

---

### Issue 18: Implement Avatar Image Upload to Supabase Storage
**Labels:** `backend`, `storage`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Create image upload functionality for user avatars with validation and optimization.

**What needs to be done:**
1. Create `/api/users/avatar/upload` endpoint
2. Validate file size (max 5MB)
3. Verify file type (JPEG, PNG only)
4. Upload to Supabase Storage
5. Return public URL
6. Delete old avatar if exists

**Resolution Tips:**
- Use Supabase Storage: `supabase.storage.from('avatars').upload()`
- Validate file size: `if (file.size > 5 * 1024 * 1024) throw Error`
- Check MIME type: `file.type.startsWith('image/')`
- Generate unique filename: `${userId}-${timestamp}.${ext}`
- Configure Supabase bucket as public
- Delete old file: `supabase.storage.from('avatars').remove()`
- Reference: [Supabase Storage](https://supabase.com/docs/guides/storage)

---

### Issue 19: Create Public User Profile View Page
**Labels:** `ui`, `profile`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Build public profile pages for other users so people can see their background before messaging them.

**What needs to be done:**
1. Create `/app/users/[id]/page.tsx`
2. Fetch user data by public key or username
3. Display public profile info
4. Show user's listings
5. Add "Message User" button
6. Display trust/rating info (future integration)

**Public Profile Info:**
- Avatar
- Username
- Bio
- Listings count
- Member since date
- Message button (if authenticated)
- List of their listings

**Resolution Tips:**
- Create `/api/users/[id]` GET endpoint
- Use `[id]` for dynamic routing
- Show only public information (no email)
- Add Message button link to chat
- Use `Image` component with fallback avatar
- Reference: [Next.js Dynamic Routes](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes)

---

### Issue 20: Create User Dashboard Landing Page
**Labels:** `ui`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Build the main dashboard page for authenticated users showing overview of their activity and quick actions.

**What needs to be done:**
1. Create `/app/dashboard/page.tsx`
2. Display user stats (listings, my payments, messages)
3. Create quick action buttons (create listing, browse, messages)
4. Show recent activity
5. Add responsive layout with cards
6. Implement loading states

**Dashboard Shows:**
- Welcome message
- User stats cards (listings count, payments, messages unread)
- Quick action buttons
- Recent activities feed
- Unread messages count

**Resolution Tips:**
- Fetch aggregated user data in single query or with SWR
- Use reusable stat card component
- Implement skeleton loaders
- Make responsive with Tailwind grid
- Add navigation to main features
- Reference: [React SWR for data fetching](https://swr.vercel.app/)

---

### Issue 21: Setup User Stats Aggregation Queries
**Labels:** `backend`, `database`, `type:task`  
**Complexity:** MEDIUM (150 points)

**Description:**
Create efficient database queries for aggregating user statistics used across the app.

**What needs to be done:**
1. Create query functions in `lib/queries/users.ts`
2. Implement efficient aggregation queries
3. Add caching layer if needed
4. Create `/api/users/[id]/stats` endpoint
5. Document query patterns

**Stats to Track:**
```
- Total listings created
- Total messages sent/received
- Total rent payments made
- Unread messages count
- Active agreements count
```

**Resolution Tips:**
- Use database views for complex aggregations
- Create indexes on frequently grouped columns
- Use Supabase RLS with `auth.uid()`
- Implement result caching with SWR or React Query
- Example query:
```sql
SELECT COUNT(*) as listings_count FROM listings WHERE landlord_id = $1
```
- Reference: [SQL Aggregation](https://www.postgresql.org/docs/current/functions-aggregate.html)

---

### Issue 22: Create User Notification System (Database Schema)
**Labels:** `backend`, `database`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Design and implement the database schema for a notification system to alert users of important events.

**What needs to be done:**
1. Create `notifications` table in database
2. Create notification types enum
3. Define triggers for notification creation
4. Create `/api/notifications` endpoint
5. Create notification preferences table

**Notification Table Schema:**
```
notifications:
  - id (UUID)
  - user_id (FK to users)
  - type (enum: message, payment, listing, agreement)
  - title (string)
  - content (string)
  - read (boolean)
  - data (JSONB) - contextual data
  - created_at
```

**Resolution Tips:**
- Use PostgreSQL ENUM type for notification types
- Create trigger to auto-insert notifications on message_insert
- Add index on (user_id, read) for quick fetches
- Create notification preferences: `enable_notifications`, `enable_emails`
- Reference: [PostgreSQL ENUM Types](https://www.postgresql.org/docs/current/datatype-enum.html)

---

## Phase 4: Listings Foundation (Issues 23-30)

---

### Issue 23: Create Listing Model & Database Schema
**Labels:** `database`, `type:backend`  
**Complexity:** MEDIUM (150 points)

**Description:**
Design the comprehensive database schema for apartment/room listings with all relevant fields.

**What needs to be done:**
1. Expand/refine `listings` table
2. Create `listing_amenities` junction table
3. Create `listing_images` table
4. Add fields: address, bedrooms, bathrooms, furnished, etc.
5. Add indexes for search queries
6. Define RLS policies

**Listing Table Fields:**
```
listings:
  - id, landlord_id, title, description, address
  - rent_xlm (monthly rent), bedrooms, bathrooms
  - furnished (boolean), pet_friendly (boolean)
  - move_in_date, lease_term_months
  - latitude, longitude (for maps)
  - status (active, inactive)
  - created_at, updated_at
```

**Resolution Tips:**
- Use PostGIS extension for location queries: `CREATE EXTENSION IF NOT EXISTS postgis;`
- Create amenities table for flexibility
- Add check constraints: `CHECK (bedrooms > 0 AND bathrooms > 0)`
- Add partial index: `CREATE INDEX idx_active_listings ON listings(landlord_id) WHERE status = 'active';`
- Reference: [PostgreSQL CHECK Constraints](https://www.postgresql.org/docs/current/ddl-constraints.html)

---

### Issue 24: Create Listing API Endpoint for Creating New Listings
**Labels:** `backend`, `api`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Implement the API endpoint for users to create new apartment listings with validation.

**What needs to be done:**
1. Create POST `/api/listings` endpoint
2. Validate all required fields
3. Require authentication
4. Create listing in database
5. Return created listing with ID
6. Handle errors appropriately

**Request Body:**
```json
{
  "title": "Cozy 2BR in Downtown",
  "description": "...",
  "address": "123 Main St, City, State",
  "rent_xlm": 1500,
  "bedrooms": 2,
  "bathrooms": 1,
  "furnished": true,
  "pet_friendly": false,
  "amenities": ["wifi", "washer", "dishwasher"]
}
```

**Resolution Tips:**
- Use Zod to validate request body
- Get landlord_id from authenticated JWT
- Return 401 if not authenticated
- Return 400 with validation errors
- Set initial status to "active"
- Reference: [API Response Best Practices](https://jsonapi.org/)

---

### Issue 25: Create Listing Edit & Delete Endpoints
**Labels:** `backend`, `api`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Implement endpoints for listing owners to modify and delete their listings.

**What needs to be done:**
1. Create PUT `/api/listings/[id]` endpoint
2. Create DELETE `/api/listings/[id]` endpoint
3. Verify user is listing owner
4. Validate update fields
5. Handle cascading deletes
6. Return updated/deleted listing

**Resolution Tips:**
- Check ownership: `SELECT landlord_id FROM listings WHERE id = $1`
- Return 403 if user is not owner
- Validate fields same as create endpoint
- Delete associated images and amenities
- Use transaction: `BEGIN; ... COMMIT;`
- Soft delete option: Set status to 'deleted' instead of hard delete
- Reference: [REST API Best Practices](https://restfulapi.net/http-methods/)

---

### Issue 26: Create Listing Detail Page with Full Information
**Labels:** `ui`, `listing`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Build the detailed listing view showing all listing information, images, and landlord details.

**What needs to be done:**
1. Create `/app/listings/[id]/page.tsx`
2. Fetch listing and landlord data
3. Display listing info with multiple images carousel
4. Show map with location
5. Display amenities
6. Add message landlord button
7. Show move-in date and lease terms

**Page Sections:**
- Image carousel
- Basic info (price, bedrooms, bathrooms)
- Description
- Amenities list
- Location map
- Landlord profile card with message button
- Apply/Inquire button

**Resolution Tips:**
- Use dynamic routing: `[id]` can be UUID
- Fetch from `/api/listings/[id]` endpoint
- Use next/image library for optimized images
- Use Google Maps or Mapbox for location display
- Implement image carousel with arrows/dots
- Add error page for invalid/deleted listings
- Reference: [Next.js Dynamic Routes](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes)

---

### Issue 27: Implement Listing Image Upload with Multiple Images
**Labels:** `backend`, `storage`, `type:feature`  
**Complexity:** HIGH (200 points)

**Description:**
Create image upload functionality for listings supporting multiple images with optimization and ordering.

**What needs to be done:**
1. Create POST `/api/listings/[id]/images/upload` endpoint
2. Handle multiple file uploads
3. Validate file sizes and types
4. Upload to Supabase Storage
5. Create image records in database with order
6. Implement image deletion
7. Optimize images on upload

**Resolution Tips:**
- Accept FormData with multiple files
- Validate each file (JPEG/PNG, max 10MB)
- Generate unique filenames: `${listingId}-${timestamp}-${index}.jpg`
- Compress images: use Sharp library `npm install sharp`
- Create thumbnail version (500x500)
- Store order in database for carousel
- Example compression:
```typescript
import sharp from 'sharp'
const buffer = await sharp(file).resize(1200, 900).webp({ quality: 80 }).toBuffer()
```
- Reference: [Sharp Image Library](https://sharp.pixelplumbing.com/)

---

### Issue 28: Create Listing Browse Page (List All Listings)
**Labels:** `ui`, `listing`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Build the main listings browsing page showing all available listings with pagination and filtering options.

**What needs to be done:**
1. Create `/app/listings/page.tsx`
2. Fetch listings from API with pagination
3. Display listings in grid layout
4. Implement pagination controls
5. Add loading states
6. Show listing cards with images and basic info
7. Add responsive design

**Listing Card Shows:**
- First image
- Title
- Price (XLM)
- Bedrooms/Bathrooms count
- Location
- Landlord name

**Resolution Tips:**
- Use `/api/listings?page=1&limit=12` endpoint
- Implement offset pagination: `LIMIT 12 OFFSET (page-1)*12`
- Use Tailwind grid for responsive layout
- Add skeleton loader while fetching
- Use Next.js Link for navigation
- Implement loading state with Suspense boundary
- Reference: [Pagination Best Practices](https://www.postgresql.org/docs/current/queries-limit.html)

---

### Issue 29: Create Listing Search API Endpoint
**Labels:** `backend`, `api`, `type:feature`, `search`  
**Complexity:** HIGH (200 points)

**Description:**
Implement advanced search endpoint for listings with filtering by price, location, bedrooms, etc.

**What needs to be done:**
1. Create GET `/api/listings/search` endpoint
2. Support price range filter
3. Support location-based search
4. Support bedrooms/bathrooms filter
5. Support amenities filter
6. Implement full-text search on title/description
7. Add sorting options

**Query Parameters:**
```
?minPrice=100&maxPrice=2000
&location=downtown&radius=5km
&bedrooms=2
&bathrooms=1
&amenities=wifi,washer
&sortBy=price&order=asc
&page=1&limit=20
```

**Resolution Tips:**
- Use ILIKE for case-insensitive text search: `WHERE title ILIKE $1 OR description ILIKE $1`
- Use BETWEEN for price ranges: `WHERE rent_xlm BETWEEN $1 AND $2`
- Use PostGIS for location proximity: `SELECT * FROM listings WHERE ST_Distance(location, point) < $1`
- Create full-text search index: `CREATE INDEX idx_listings_fts ON listings USING GIN(to_tsvector('english', title || ' ' || description));`
- Reference: [PostgreSQL Full Text Search](https://www.postgresql.org/docs/current/textsearch.html)

---

### Issue 30: Create Advanced Listing Filters Component
**Labels:** `ui`, `listing`, `type:feature`, `search`  
**Complexity:** MEDIUM (150 points)

**Description:**
Build a sidebar component with advanced filters for browse page allowing users to refine search results.

**What needs to be done:**
1. Create FilterSidebar component
2. Implement price range slider
3. Add checkbox filters (furnished, pet-friendly)
4. Add bedrooms/bathrooms dropdown
5. Add amenities multi-select
6. Implement location input with autocomplete
7. Add "Clear Filters" button
8. Wire up to search API

**Filter Options:**
- Price range (slider: 0-5000 XLM)
- Bedrooms (1-6+ options)
- Bathrooms (1-4+ options)
- Furnished (yes/no)
- Pet-friendly (yes/no)
- Amenities (wifi, washer, dishwasher, ac, parking, etc.)
- Location/city search

**Resolution Tips:**
- Use React Hooks for filter state
- Use URL query params to persist filters: `useRouter().push(queryString)`
- Implement debouncing for price slider
- Use checkbox inputs for boolean filters
- Use multi-select dropdown for amenities
- Add useCallback to prevent unnecessary re-renders
- Reference: [React-Select Library](https://react-select.com/)

---

## Phase 5: Search & Display Enhancements (Issues 31-35)

---

### Issue 31: Create Listing Map View Component
**Labels:** `ui`, `map`, `type:feature`  
**Complexity:** HIGH (200 points)

**Description:**
Implement an interactive map showing all listings with their locations for better spatial browsing.

**What needs to be done:**
1. Setup Mapbox or Google Maps integration
2. Create MapView component
3. Add map markers for each listing
4. Implement marker click to show listing preview
5. Add zoom and pan functionality
6. Add location-based filtering (show listings in viewport)
7. Persist map state in URL

**Resolution Tips:**
- Use Mapbox GL (free tier available): `npm install mapbox-gl`
- Or use Google Maps API: `npm install @react-google-maps/api`
- Fetch listings based on map bounds: `api/listings/search?bbox=...`
- Implement clustering for many markers
- Use custom marker styles for different listing types
- Geocode addresses to coordinates
- Reference: [Mapbox GL Documentation](https://docs.mapbox.com/mapbox-gl-js/)

---

### Issue 32: Add Listing Favorites/Bookmarks Feature
**Labels:** `feature`, `database`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Allow authenticated users to save favorite listings for later viewing.

**What needs to be done:**
1. Create `user_favorites` table
2. Create POST `/api/listings/[id]/favorite` endpoint
3. Create DELETE `/api/listings/[id]/favorite` endpoint
4. Create GET `/api/users/me/favorites` endpoint
5. Add favorite button to listing cards
6. Show favorite heart icon
7. Create favorites page at `/app/favorites`

**Resolution Tips:**
- Table: `user_favorites (user_id, listing_id, created_at)` with UNIQUE constraint
- Use `useOptimisticUpdate` for instant UI feedback
- Return 409 if already favorited
- Add heart icon toggle on listing card
- Aggregate count: `SELECT COUNT(*) FROM user_favorites WHERE listing_id = $1`
- Reference: [SQL One-to-Many Relationships](https://www.postgresql.org/docs/current/ddl-constraints.html)

---

### Issue 33: Create Listings Performance Optimization with Caching
**Labels:** `backend`, `performance`, `type:task`  
**Complexity:** MEDIUM (150 points)

**Description:**
Optimize listing queries with caching layer to improve page load performance.

**What needs to be done:**
1. Implement Redis caching for listings
2. Cache active listings list (15 min TTL)
3. Cache individual listing details (1 hour TTL)
4. Create cache invalidation on listing updates
5. Monitor cache hit rates
6. Document caching strategy

**Resolution Tips:**
- Use Upstash Redis (serverless): `npm install @upstash/redis`
- Cache key pattern: `listings:all:page:1` or `listings:detail:${listingId}`
- Invalidate on create/update/delete: `await redis.del('listings:all:*')`
- Add cache busting headers: `Cache-Control: public, max-age=900`
- Monitor with: `redis.info()`
- Reference: [Upstash Redis](https://upstash.com/docs/redis/overall/getstarted)

---

### Issue 34: Add Listing Sorting Options
**Labels:** `ui`, `feature`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Implement different sorting options for listings to help users find what they need.

**What needs to be done:**
1. Add sort dropdown to listings page
2. Implement backend sorting: price (asc/desc), newest, most popular
3. Persist sort selection in URL
4. Highlight current sort option
5. Re-fetch listings when sort changes
6. Add "Recommended" algorithm (optional)

**Sorting Options:**
- Newest first (default)
- Price: Low to High
- Price: High to Low
- Most Viewed
- Most Favorited

**Resolution Tips:**
- Add `sortBy` and `sortOrder` query params
- Update API: `ORDER BY ${sortBy} ${sortOrder}`
- Use `useRouter()` and query params
- Add active state styling to selected sort
- Preserve other filters when changing sort
- Reference: [SQL ORDER BY](https://www.postgresql.org/docs/current/sql-select.html)

---

### Issue 35: Create Recently Viewed Listings Feature
**Labels:** `feature`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Track and display recently viewed listings for better user experience and navigation.

**What needs to be done:**
1. Create `user_view_history` table
2. Log listing view on detail page load
3. Create GET `/api/users/me/recent` endpoint
4. Display recent listings on dashboard
5. Add "View History" page at `/app/history`
6. Limit to last 50 views
7. Add timestamp to each view

**Resolution Tips:**
- Table: `user_view_history (id, user_id, listing_id, viewed_at)`
- Insert view on mount: `POST /api/listings/[id]/view`
- Query: `SELECT DISTINCT ON (listing_id) * FROM user_view_history WHERE user_id = $1 ORDER BY listing_id DESC, viewed_at DESC LIMIT 50`
- Store in localStorage for anonymous users (pre-auth)
- Show with thumbnails and last viewed time
- Reference: [PostgreSQL DISTINCT ON](https://www.postgresql.org/docs/current/sql-select.html)

---

## Phase 6: Messaging/Chat Basics (Issues 36-40)

---

### Issue 36: Create Message Database Schema
**Labels:** `database`, `messaging`, `type:backend`  
**Complexity:** MEDIUM (150 points)

**Description:**
Design the database schema for user-to-user messaging with support for conversations and message history.

**What needs to be done:**
1. Create/refine `messages` table
2. Create `conversations` table
3. Add indexes for efficient queries
4. Add constraints and validations
5. Setup RLS policies for privacy
6. Create view for conversation threads

**Schema Design:**
```
conversations:
  - id, user1_id, user2_id, listing_id, created_at, updated_at

messages:
  - id, conversation_id, sender_id, content, read, created_at
```

**Resolution Tips:**
- Add constraint to ensure user1_id < user2_id for conversation uniqueness
- Create index: `CREATE INDEX idx_conversations_users ON conversations(user1_id, user2_id);`
- Add `read_at` timestamp for read receipts
- Use `CHECK` constraint: `CHECK (sender_id IN (user1_id, user2_id))`
- Add RLS: `CREATE POLICY "Users can only see their conversations"`
- Reference: [PostgreSQL Constraints](https://www.postgresql.org/docs/current/ddl-constraints.html)

---

### Issue 37: Create Messaging API Endpoints
**Labels:** `backend`, `api`, `messaging`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Implement API endpoints for sending, retrieving, and managing messages.

**What needs to be done:**
1. Create POST `/api/messages/send` endpoint
2. Create GET `/api/conversations` endpoint
3. Create GET `/api/conversations/[id]/messages` endpoint
4. Create PUT `/api/messages/[id]/read` endpoint
5. Implement message validation
6. Add pagination for message history
7. Handle error cases

**Endpoints:**
- POST `/api/messages/send` - Create new message
- GET `/api/conversations` - List user's conversations
- GET `/api/conversations/[id]/messages?page=1` - Message history
- PATCH `/api/messages/[id]/read` - Mark as read
- DELETE `/api/messages/[id]` - Delete message (optional)

**Resolution Tips:**
- Validate content length (1-5000 chars)
- Return 400 if recipient is same as sender
- Auto-create conversation if doesn't exist
- Return paginated results (newest first): `ORDER BY created_at DESC`
- Add pagination cursor support for better performance
- Reference: [Cursor-based Pagination](https://www.postgresql.org/docs/current/queries-limit.html)

---

### Issue 38: Create Messages Listing Page with Conversation View
**Labels:** `ui`, `messaging`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Build the messaging page showing all conversations with a preview of recent messages.

**What needs to be done:**
1. Create `/app/messages/page.tsx`
2. Fetch and display all conversations
3. Show last message preview
4. Display unread badge count
5. Show conversation participants
6. Implement responsive layout
7. Add loading states
8. Add timestamps

**Conversation List Shows:**
- User avatar
- Username
- Last message preview (truncated)
- Last message timestamp
- Unread count badge
- Click to open conversation

**Resolution Tips:**
- Fetch conversations: `GET /api/conversations?limit=50`
- Sort by last message: `ORDER BY (SELECT created_at FROM messages WHERE conversation_id = conversations.id ORDER BY created_at DESC LIMIT 1) DESC`
- Show unread count: `COUNT(*) FROM messages WHERE conversation_id = $1 AND read = false AND sender_id != current_user_id`
- Use skeleton loader while fetching
- Use relative timestamps: "2 hours ago"
- Reference: [React Relative Time](https://day.js.org/docs/en/plugin/relative-time)

---

### Issue 39: Create Individual Conversation/Chat Component
**Labels:** `ui`, `messaging`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Build the conversation detail page showing message history and input for new messages.

**What needs to be done:**
1. Create `/app/messages/[id]/page.tsx`
2. Fetch conversation and messages
3. Display message thread with scroll
4. Implement message input form
5. Send message on submit
6. Mark messages as read
7. Show typing indicator (optional)
8. Add responsive design

**Features:**
- Message history scrollable
- Current user messages on right (blue)
- Other user messages on left (gray)
- Timestamp on each message
- Input field at bottom
- Send button
- Loading state while sending
- Auto-scroll to latest message
- Read receipts (optional)

**Resolution Tips:**
- Fetch: `GET /api/conversations/[id]/messages?page=1`
- Mark as read: `PATCH /api/messages/read` batch endpoint
- Use React Ref for auto-scroll: `messageEndRef.current?.scrollIntoView()`
- Use `useOptimisticUpdate` for instant message send feedback
- Handle Enter key to send: `e.key === 'Enter' && send()`
- Reference: [React useRef for Auto-scroll](https://react.dev/reference/react/useRef)

---

### Issue 40: Add Real-Time Message Updates with Supabase Realtime
**Labels:** `messaging`, `realtime`, `type:feature`, `HIGH`  
**Complexity:** HIGH (200 points)

**Description:**
Implement real-time message updates using Supabase Realtime subscriptions for instant messaging experience.

**What needs to be done:**
1. Setup Supabase Realtime subscriptions
2. Subscribe to message changes in conversation
3. Auto-refresh message list on new messages
4. Show typing indicator (optional)
5. Handle connection errors
6. Unsubscribe on component unmount
7. Test with multiple browser tabs

**Resolution Tips:**
- Setup subscription:
```typescript
const subscription = supabase
  .on('postgres_changes', 
    { event: 'INSERT', schema: 'public', table: 'messages', 
      filter: `conversation_id=eq.${conversationId}` },
    (payload) => { addMessage(payload.new) }
  )
  .subscribe()
```
- Cleanup on unmount: `subscription.unsubscribe()`
- Handle connection state: `supabase.getChannels()[0].state`
- Add reconnection logic
- Test: `supabase.realtime.endSubscription(subscription)`
- Reference: [Supabase Realtime](https://supabase.com/docs/guides/realtime)

---

## Phase 7: Smart Contract Foundation (Issues 41-45)

---

### Issue 41: Design & Implement Core Rent Agreement Smart Contract
**Labels:** `blockchain`, `contract`, `type:feature`, `HIGH`  
**Complexity:** HIGH (200 points)

**Description:**
Implement the Soroban smart contract for rent escrow with deposits, withdrawals, and payment logic.

**What needs to be done:**
1. Replace skeleton contract with full RentAgreement logic
2. Implement storage for admin, landlord, tenants, rent_amount
3. Implement `initialize()` function
4. Implement `deposit()` for tenant payments
5. Implement `withdraw()` for landlord to claim rent
6. Implement `get_balance()` to check contract balance
7. Implement `get_status()` to check payment status
8. Add event emissions for transactions
9. Write comprehensive tests

**Contract Functions:**
```rust
pub fn initialize(env: Env, landlord: Address, tenants: Vec<Address>, rent_amount: i128)
pub fn deposit(env: Env, from: Address, amount: i128)
pub fn withdraw(env: Env) -> i128  // Only landlord can call
pub fn get_balance(env: Env) -> i128
pub fn get_status(env: Env) -> ContractStatus
pub fn get_rent_amount(env: Env) -> i128
```

**Resolution Tips:**
- Use Soroban SDK for state storage
- Store balances in map: `Map<Address, i128>`
- Use `Address` type for Stellar addresses
- Implement access control checks
- Emit events with `env.events().publish()`
- Write tests in `test.rs`
- Example event: `env.events().publish(("transfer", from, to), amount)`
- Reference: [Soroban Smart Contract Development](https://developers.stellar.org/docs/build/smart-contracts)

---

### Issue 42: Setup Contract Compilation and Testing Infrastructure
**Labels:** `blockchain`, `contract`, `type:task`  
**Complexity:** MEDIUM (150 points)

**Description:**
Setup proper build pipeline and testing framework for the Soroban smart contract.

**What needs to be done:**
1. Configure `Cargo.toml` with dependencies
2. Setup cargo_next for latest features
3. Create test helpers and utilities
4. Implement contract tests
5. Setup GitHub Action for contract tests
6. Add contract deployment script
7. Document testing guidelines

**Cargo.toml Dependencies:**
```toml
[dependencies]
soroban-sdk = { version = "20.0.0", features = ["testutils"] }
soroban-ledger-snapshot = "20.0.0"

[dev-dependencies]
soroban-sdk = { version = "20.0.0", features = ["testutils"] }
```

**Resolution Tips:**
- Run tests: `cargo test --features testutils`
- Test contract with mock environment
- Use `e` parameter for test environment
- Create contract instances for each test
- Setup fixtures for common test data
- Add CI: `.github/workflows/contract-tests.yml`
- Reference: [Soroban Testing](https://developers.stellar.org/docs/build/smart-contracts/testing)

---

### Issue 43: Create Contract Deployment Script & Infrastructure
**Labels:** `blockchain`, `devops`, `type:task`, `HIGH`  
**Complexity:** HIGH (200 points)

**Description:**
Implement scripts and utilities for deploying the smart contract to Stellar testnet and mainnet.

**What needs to be done:**
1. Create deployment script in TypeScript/JavaScript
2. Setup wallet loading for deployment
3. Implement contract installation on network
4. Add environment-specific deployment (testnet vs mainnet)
5. Store contract IDs in database
6. Create deployment verification script
7. Document deployment process

**Resolution Tips:**
- Use `@stellar/js-stellar-sdk` for deployment
- Create deploy account with test XLM
- Build WASM: `soroban contract build`
- Submit transaction on testnet first
- Store contract ID in Supabase after deployment
- Example deploy script:
```typescript
const contract = await server.submitTransaction(tx)
await supabase.from('contracts').insert({ contract_id: contract.id })
```
- Use Testnet before mainnet
- Reference: [Stellar Testnet](https://developers.stellar.org/docs/learn/fundamentals-and-concepts/testnet)

---

### Issue 44: Integrate Freighter Wallet for Contract Transactions
**Labels:** `blockchain`, `wallet`, `type:feature`, `HIGH`  
**Complexity:** HIGH (200 points)

**Description:**
Implement Freighter wallet integration for signing and submitting smart contract transactions.

**What needs to be done:**
1. Create transaction building utilities
2. Implement signing flow with Freighter
3. Create transaction submission handler
4. Add gas fee estimation
5. Implement transaction status tracking
6. Handle signing errors and cancellations
7. Create transaction history logging

**Resolution Tips:**
- Build transaction with stellar-sdk
- Sign with Freighter: `const signedXdr = await signTransaction(xdr)`
- Submit to network: `await submitTransaction(signedXdr)`
- Get transaction status: `server.transactions().transaction(id).call()`
- Estimate fee: Use Soroban's `soroban_estimateGas`
- Handle user cancellation gracefully
- Log all transactions to database
- Reference: [Freighter Transaction Signing](https://developers.stellar.org/docs/tools-and-sdks/freighter-sdk/interactive-signing)

---

### Issue 45: Create Contract Interaction Service Layer
**Labels:** `backend`, `blockchain`, `type:task`  
**Complexity:** MEDIUM (150 points)

**Description:**
Create a service layer that abstracts smart contract interactions for easier backend usage.

**What needs to be done:**
1. Create `lib/stellar/contract.ts` with contract utilities
2. Implement `RentContractService` class
3. Create helper functions for common operations
4. Implement error handling and retry logic
5. Add logging for debugging
6. Create mock service for testing
7. Document all service methods

**Service Methods:**
```typescript
class RentContractService {
  async deployContract(landlord, tenants, rentAmount): Promise<contractId>
  async depositRent(contractId, tenantAddress, amount): Promise<txHash>
  async withdrawRent(contractId): Promise<txHash>
  async getContractBalance(contractId): Promise<amount>
  async getContractStatus(contractId): Promise<status>
  async parseContractError(error): Promise<userFriendlyMessage>
}
```

**Resolution Tips:**
- Use singleton pattern for service
- Implement retry logic for network failures
- Use exponential backoff for retries
- Log all operations with timestamps
- Create mock service for testing frontend
- Handle network errors with user feedback
- Reference: [Service Layer Pattern](https://martinfowler.com/eaaCatalog/serviceLocator.html)

---

## Phase 8: Blockchain Integration Start (Issues 46-50)

---

### Issue 46: Create Payment Initiation Page for Rent Payment
**Labels:** `ui`, `payment`, `type:feature`, `HIGH`  
**Complexity:** HIGH (200 points)

**Description:**
Build the page where tenants initiate their rent payment through the Soroban smart contract.

**What needs to be done:**
1. Create `/app/listings/[id]/pay/page.tsx`
2. Fetch listing and contract details
3. Display payment information clearly
4. Show XLM amount and conversion rates
5. Implement payment confirmation dialog
6. Call contract deposit function
7. Handle signing with Freighter
8. Show transaction status tracking
9. Add success/error feedback

**Page Shows:**
- Listing details (for context)
- Your share of rent (XLM amount)
- Current payment status
- Deposit date
- Confirm Payment button
- Transaction hash after payment

**Resolution Tips:**
- Use RentContractService for deposit call
- Show clear confirmation dialog before signing
- Display Freighter signing prompt info
- Track transaction in database: `payment_records` table
- Use polling or event subscription for status updates
- Show loading state during transaction
- Handle user rejection gracefully
- Reference: [Payment Flow Best Practices](https://www.freecodecamp.org/news/blockchain-based-payments/)

---

### Issue 47: Create Payment Status Tracking & Webhooks
**Labels:** `backend`, `blockchain`, `type:feature`, `HIGH`  
**Complexity:** HIGH (200 points)

**Description:**
Implement payment status tracking and webhook handlers for monitoring contract transactions on blockchain.

**What needs to be done:**
1. Create payment status polling endpoint
2. Implement Stellar Webhook integration (optional)
3. Create database records for payment tracking
4. Update payment status as transactions confirm
5. Add background job for periodic polling
6. Emit notifications when payments received
7. Handle transaction failures gracefully
8. Create payment history view

**Flow:**
1. User initiates payment → create `payment_records` entry with status "pending"
2. Transaction submitted → store transaction_hash
3. Periodic check → query Stellar Horizon API
4. Transaction confirmed → update status to "confirmed"
5. Emit notification → user sees payment success

**Resolution Tips:**
- Use Stellar Horizon API: `fetch(`https://horizon.stellar.org/transactions/${txHash}`)`
- Create background job: `npm install node-cron`
- Poll every 30 seconds for pending transactions
- Store ledger_close_time when confirmed
- Create `/api/payments/[id]/status` endpoint
- Emit notifications: `POST /api/notifications`
- Reference: [Stellar Horizon API](https://developers.stellar.org/api/introduction/index/), [Node-Cron](https://github.com/kelektiv/node-cron)

---

### Issue 48: Add Transaction Fee Handling & Gas Estimation
**Labels:** `backend`, `payment`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Implement proper handling of Stellar transaction fees and gas estimation for rent payments.

**What needs to be done:**
1. Estimate contract invocation fees
2. Display fee information to user
3. Add fee to payment amount
4. Implement fee adjustment logic
5. Create `/api/payments/estimate-fee` endpoint
6. Handle fee spike scenarios
7. Provide fee breakdown in UI
8. Document fee structure

**Resolution Tips:**
- Base fee: 100 stroops (0.00001 XLM)
- Contract invocation fee: depends on complexity
- Use Soroban `soroban_estimateGas`
- Recommend fee: estimated + buffer (20%)
- Get fee stats: `server.feeStats()`
- Cache fee estimates (5-10 min TTL)
- Show breakdown: Base + Contract execution + Buffer
- Reference: [Stellar Transaction Fees](https://developers.stellar.org/docs/learn/fundamentals-and-concepts/fees-and-payments)

---

### Issue 49: Create Payment Receipt & History Page
**Labels:** `ui`, `payment`, `type:feature`  
**Complexity:** MEDIUM (150 points)

**Description:**
Build a page showing payment history and receipts for audit trail and transparency.

**What needs to be done:**
1. Create `/app/payments/history/page.tsx`
2. Fetch payment history from database
3. Display payment list with details
4. Create payment receipt view
5. Add filters (by listing, by date range)
6. Implement PDF export for receipts
7. Show transaction hash on blockchain
8. Add payment status badge

**Payment History Shows:**
- Listing name
- Amount paid (XLM)
- Payment date
- Status (completed, pending, failed)
- Transaction hash
- Receipt button
- View on Blockchain link

**Resolution Tips:**
- Query: `SELECT * FROM payment_records WHERE user_id = $1 ORDER BY created_at DESC`
- Create receipt component with printable layout
- Generate PDF: `npm install jspdf react-pdf`
- Show Stellar testnet/mainnet link based on network
- Format timestamps: `new Date(created_at).toLocaleDateString()`
- Add status badges with colors
- Reference: [React PDF](https://react-pdf.org/), [jsPDF](https://github.com/parallax/jsPDF)

---

### Issue 50: Create Admin Dashboard for Payment Verification & Dispute Resolution
**Labels:** `ui`, `admin`, `type:feature`, `HIGH`  
**Complexity:** HIGH (200 points)

**Description:**
Build an admin dashboard for platform maintainers to verify payments, handle disputes, and monitor contract activity.

**What needs to be done:**
1. Create `/app/admin/dashboard/page.tsx` (admin-only)
2. Implement admin authentication check
3. Display payment statistics and metrics
4. Show recent payments with verification
5. Create dispute/issue reporting UI
6. Add contract monitoring view
7. Implement admin actions (refund button, etc.)
8. Add activity audit log
9. Create admin notifications

**Admin Dashboard Shows:**
- Total payments processed
- Pending/failed transactions
- Recent payment list
- Contract status overview
- User reports/disputes
- System health metrics
- Activity log
- Manual intervention tools

**Resolution Tips:**
- Add admin role check: `if (user.role !== 'admin') redirect()`
- Use RLS with admin check: `CREATE POLICY "admins_can_see_all_payments"`
- Create admin reports view
- Implement payment dispute flow
- Add manual refund capability (with caution)
- Log all admin actions
- Send admin notifications for issues
- Reference: [Role-Based Access Control](https://en.wikipedia.org/wiki/Role-based_access_control)

---

## Summary

These 50 issues are organized to build the application strategically:

**Phases Built:**
- ✅ **Phase 1 (Issues 1-8):** Foundation - setup, config, database
- ✅ **Phase 2 (Issues 9-15):** Authentication - wallet connection, login, registration
- ✅ **Phase 3 (Issues 16-22):** User profiles - editing, avatars, dashboard
- ✅ **Phase 4 (Issues 23-30):** Listings - CRUD, images, browsing
- ✅ **Phase 5 (Issues 31-35):** Search/discovery - maps, favorites, sorting
- ✅ **Phase 6 (Issues 36-40):** Messaging - real-time chat
- ✅ **Phase 7 (Issues 41-45):** Smart contracts - foundation & integration
- ✅ **Phase 8 (Issues 46-50):** Blockchain payments - initiation through admin

**At these 50 issues, the app is approximately 50% complete** with all core features built, reaching a functional MVP with blockchain integration started.

---

## Point Distribution

- **MEDIUM (150 pts):** 36 issues
- **HIGH (200 pts):** 14 issues
- **Total Points Possible:** 5,400 points
- **Estimated Value per Issue:** $150-200 depending on difficulty

