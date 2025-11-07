# Session Align - Quick Setup Guide

## 🚀 From Zero to Running in 30 Minutes

### Prerequisites

1. **PostgreSQL Database** (Choose one):

   - [Neon](https://neon.tech) - Free tier, serverless PostgreSQL
   - [Supabase](https://supabase.com) - Free PostgreSQL with extras
   - [Railway](https://railway.app) - Simple PostgreSQL hosting
   - Local PostgreSQL for development

2. **Clerk Account** (Authentication):

   - Sign up at [clerk.com](https://clerk.com)
   - Create a new application
   - Copy your API keys

3. **Storage** (Choose one):
   - AWS S3 - Traditional choice
   - Cloudflare R2 - Cheaper, S3-compatible
   - Uploadthing - Simpler for small files

## Step 1: Database Setup

### Option A: Neon (Recommended - Fastest)

```bash
# 1. Sign up at neon.tech
# 2. Create a new database
# 3. Copy the connection string
# 4. Update .env.local:
DATABASE_URL="postgresql://user:pass@host/dbname?sslmode=require"
```

### Option B: Local Development

```bash
# Using Docker
docker run --name session-align-db \
  -e POSTGRES_DB=session_align \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=postgres \
  -p 5432:5432 \
  -d postgres:15

# Update .env.local:
DATABASE_URL="postgresql://postgres:postgres@localhost:5432/session_align"
```

## Step 2: Clerk Authentication Setup

1. **Create Clerk Application:**

   - Go to [dashboard.clerk.com](https://dashboard.clerk.com)
   - Click "Add application"
   - Name: "Session Align"
   - Choose sign-in methods (Email, Google, etc.)

2. **Get API Keys:**

   - Go to API Keys section
   - Copy to `.env.local`:

   ```env
   NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_...
   CLERK_SECRET_KEY=sk_test_...
   ```

3. **Configure User Metadata:**
   - Go to Users & Authentication > User Metadata
   - Define the following fields in the public metadata schema:
     - `role`: string (`CREATOR`, `PUBLISHER`, or `ADMIN`)
     - `tenantId`: string
     - `onboarded`: boolean

## Step 3: Generate & Run Migrations

```bash
# Generate migration files from schema
npm run db:generate

# Apply migrations to database
npm run db:migrate

# Optional: Open Drizzle Studio to view database
npm run db:studio
```

## Step 4: Configure Storage (For Audio Files)

### Option A: Cloudflare R2 (Recommended - Cheaper)

```bash
# 1. Sign up at cloudflare.com
# 2. Go to R2 Storage
# 3. Create a bucket: "session-align-media"
# 4. Generate API tokens
# 5. Update .env.local with R2 credentials
```

### Option B: AWS S3

```bash
# 1. Create S3 bucket in AWS Console
# 2. Set up IAM user with S3 access
# 3. Update .env.local with AWS credentials
```

## Step 5: Run the Application

```bash
# Install dependencies (if not done)
npm install

# Run development server
npm run dev

# Open http://localhost:3000
```

## Step 6: Initial Setup Tasks

1. **Sign up as first user** - Will become admin
2. **Create organization** - Your music label/publisher
3. **Configure organization settings** in dashboard

## 🎯 What's Working Now

After setup, you immediately have:

- ✅ User authentication (sign up, login, SSO)
- ✅ Multi-tenant organizations
- ✅ Role-based access (Creator, Publisher, Admin)
- ✅ User dashboard
- ✅ Billing/subscription ready (Stripe)
- ✅ Database with Session Align schema
- ✅ File upload infrastructure
- ✅ Production-ready security

## 📦 Next Steps: Add Music Features

Now you can focus on building Session Align specific features:

### 1. Creator Profile Component

```typescript
// src/features/creators/ProfileForm.tsx
import { useUser } from '@clerk/nextjs';

import { creatorSchema } from '@/models/Schema';

export function CreatorProfileForm() {
  // Build your creator profile form
}
```

### 2. Availability Calendar

```typescript
// src/features/availability/Calendar.tsx
import { availabilitySchema } from '@/models/Schema';

export function AvailabilityCalendar() {
  // Build calendar component
}
```

### 3. Session Matching

```typescript
// src/features/matching/MatchingEngine.ts
export async function findMatches(creatorId: string) {
  // Implement matching algorithm
}
```

### 4. Audio Upload

```typescript
// src/features/upload/AudioUpload.tsx
import { uploadToS3 } from '@/libs/s3';

export function AudioUpload() {
  // Handle audio file uploads
}
```

## 🚢 Deployment (When Ready)

### Deploy to Vercel (Easiest)

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel

# Follow prompts, add environment variables
```

### Deploy to Railway

```bash
# Connect GitHub repo
# Railway will auto-deploy on push
# Add environment variables in dashboard
```

## 🔧 Troubleshooting

### Database Connection Issues

- Ensure DATABASE_URL has `?sslmode=require` for production databases
- Check if database is accessible from your IP

### Clerk Not Working

- Verify API keys are correct
- Check if URLs in .env.local match your setup
- Ensure webhook endpoint is configured for production

### File Upload Issues

- Check S3/R2 bucket permissions
- Verify CORS settings on bucket
- Test with small files first

## 📚 Resources

- [Drizzle ORM Docs](https://orm.drizzle.team)
- [Clerk Docs](https://clerk.com/docs)
- [Shadcn UI Components](https://ui.shadcn.com)
- [Next.js App Router](https://nextjs.org/docs/app)

## 💡 Pro Tips

1. **Use Neon for database** - Free tier is generous, serverless scales automatically
2. **Use Cloudflare R2 for storage** - 10GB free, way cheaper than S3
3. **Start with Clerk's free tier** - 5,000 monthly active users free
4. **Deploy to Vercel** - Generous free tier, automatic previews

---

## 🎉 You're Ready!

With this setup, you've eliminated weeks of infrastructure work and can focus on building the unique Session Align features. The boilerplate handles all the boring stuff - auth, multi-tenancy, security, database, file uploads, payments.

Now go build the music industry collaboration platform!
