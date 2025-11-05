# 🚀 Pet Clinic Deployment Guide - FREE Hosting Setup

## Overview
Complete guide for deploying Pet Clinic Management System using 100% FREE services.

## 🎯 Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│                     Cloudflare Pages                      │
│                  (Frontend + API Routes)                  │
│                     Next.js 14 App                        │
└─────────────────────────────────────────────────────────┘
                            │
                            ├── API Calls
                            ├── Auth
                            ├── Realtime
                            │
┌─────────────────────────────────────────────────────────┐
│                        Supabase                           │
│  ┌──────────────────────────────────────────────────┐    │
│  │  PostgreSQL  │  Auth  │  Storage  │  Realtime   │    │
│  └──────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────┘
```

## 💯 Free Services Stack

| Service | Purpose | Free Tier Limits |
|---------|---------|-----------------|
| **Cloudflare Pages** | Frontend Hosting + Edge Functions | Unlimited bandwidth, 500 builds/month |
| **Supabase** | Database + Auth + Storage | 500MB DB, 1GB Storage, 50K MAUs |
| **GitHub** | Source Code | Unlimited public repos |
| **Resend/SendGrid** | Email | 100 emails/day (Resend) or 100/day (SendGrid) |
| **Uploadthing** | Image Uploads (Alternative) | 2GB storage |

## 📦 Option 1: Cloudflare Pages (RECOMMENDED)

### Why Cloudflare Pages?
- ✅ Supports Next.js SSR/API Routes
- ✅ Edge Functions for serverless backend
- ✅ Unlimited bandwidth
- ✅ Global CDN
- ✅ Custom domains free
- ✅ Automatic HTTPS

### Setup Steps

#### 1. Prepare Next.js for Cloudflare Pages

```bash
# Install adapter
npm install @cloudflare/next-on-pages
```

#### 2. Update `next.config.js`
```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    runtime: 'edge', // For Cloudflare Workers
  },
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: '*.supabase.co',
      },
    ],
  },
}

module.exports = nextConfig
```

#### 3. Create `wrangler.toml`
```toml
name = "pet-clinic"
compatibility_date = "2024-01-01"

[site]
bucket = ".vercel/output/static"

[build]
command = "npm run build"

[build.upload]
format = "service-worker"

[[kv_namespaces]]
binding = "CACHE"
id = "your-kv-namespace-id"

[vars]
NEXT_PUBLIC_SUPABASE_URL = "https://your-project.supabase.co"
NEXT_PUBLIC_SUPABASE_ANON_KEY = "your-anon-key"
```

#### 4. Deploy to Cloudflare Pages

**Via Dashboard:**
1. Go to [Cloudflare Pages](https://pages.cloudflare.com/)
2. Create new project
3. Connect GitHub repository
4. Build settings:
   ```
   Framework preset: Next.js
   Build command: npx @cloudflare/next-on-pages@1
   Build output directory: .vercel/output/static
   ```
5. Environment variables (add all from `.env.local`)

**Via CLI:**
```bash
# Install Wrangler
npm install -g wrangler

# Login
wrangler login

# Deploy
npx @cloudflare/next-on-pages
wrangler pages publish .vercel/output/static
```

## 📦 Option 2: GitHub Pages (Static Only)

### Limitations
⚠️ No server-side rendering
⚠️ No API routes (must use external APIs)
⚠️ Client-side only

### Setup for Static Export

#### 1. Configure Next.js for Static Export
```javascript
// next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: 'export',
  images: {
    unoptimized: true,
  },
  basePath: '/pet-clinic', // If using github.io/pet-clinic
}

module.exports = nextConfig
```

#### 2. GitHub Actions Workflow
Create `.github/workflows/deploy.yml`:
```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: "20"
          cache: "npm"
          
      - name: Install dependencies
        run: npm ci
        
      - name: Build
        env:
          NEXT_PUBLIC_SUPABASE_URL: ${{ secrets.NEXT_PUBLIC_SUPABASE_URL }}
          NEXT_PUBLIC_SUPABASE_ANON_KEY: ${{ secrets.NEXT_PUBLIC_SUPABASE_ANON_KEY }}
        run: npm run build
        
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./out

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

## 🗄️ Supabase Setup

### 1. Create Supabase Project
1. Go to [supabase.com](https://supabase.com)
2. Create new project (FREE)
3. Save credentials:
   - Project URL
   - Anon Key
   - Service Role Key (keep secret!)

### 2. Database Schema Setup

Run in Supabase SQL Editor:

```sql
-- Enable UUID extension
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- Create custom types
CREATE TYPE user_role AS ENUM ('admin', 'veterinarian', 'receptionist', 'accountant');
CREATE TYPE appointment_status AS ENUM ('scheduled', 'confirmed', 'in-progress', 'completed', 'cancelled', 'no-show');
CREATE TYPE payment_status AS ENUM ('pending', 'partial', 'paid', 'overdue', 'cancelled');

-- Users table (extends Supabase auth.users)
CREATE TABLE public.profiles (
  id UUID REFERENCES auth.users(id) PRIMARY KEY,
  email TEXT UNIQUE NOT NULL,
  role user_role DEFAULT 'receptionist',
  first_name TEXT,
  last_name TEXT,
  phone TEXT,
  avatar_url TEXT,
  is_active BOOLEAN DEFAULT true,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Clients table
CREATE TABLE public.clients (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  first_name TEXT NOT NULL,
  last_name TEXT NOT NULL,
  email TEXT,
  phone TEXT NOT NULL,
  alternate_phone TEXT,
  address TEXT,
  city TEXT,
  postal_code TEXT,
  emergency_contact_name TEXT,
  emergency_contact_phone TEXT,
  preferred_communication TEXT CHECK (preferred_communication IN ('email', 'sms', 'call')),
  notes TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Pets table
CREATE TABLE public.pets (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  client_id UUID REFERENCES clients(id) ON DELETE CASCADE,
  name TEXT NOT NULL,
  species TEXT NOT NULL,
  breed TEXT,
  color TEXT,
  gender TEXT CHECK (gender IN ('male', 'female')),
  birth_date DATE,
  weight DECIMAL(10,2),
  microchip_id TEXT,
  photo_url TEXT,
  allergies TEXT,
  special_needs TEXT,
  behavioral_notes TEXT,
  is_active BOOLEAN DEFAULT true,
  deceased_date DATE,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Continue with other tables...
-- (See SUPABASE_SETUP.md for complete schema)

-- Enable Row Level Security
ALTER TABLE profiles ENABLE ROW LEVEL SECURITY;
ALTER TABLE clients ENABLE ROW LEVEL SECURITY;
ALTER TABLE pets ENABLE ROW LEVEL SECURITY;

-- Create RLS Policies
CREATE POLICY "Public profiles are viewable by everyone"
  ON profiles FOR SELECT
  USING (true);

CREATE POLICY "Users can update own profile"
  ON profiles FOR UPDATE
  USING (auth.uid() = id);

-- Create indexes for better performance
CREATE INDEX idx_pets_client_id ON pets(client_id);
CREATE INDEX idx_appointments_pet_id ON appointments(pet_id);
CREATE INDEX idx_appointments_scheduled_date ON appointments(scheduled_date);

-- Create functions and triggers
CREATE OR REPLACE FUNCTION handle_updated_at()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Apply trigger to all tables with updated_at
CREATE TRIGGER set_updated_at
  BEFORE UPDATE ON profiles
  FOR EACH ROW
  EXECUTE FUNCTION handle_updated_at();
```

### 3. Storage Buckets Setup

```sql
-- Create storage buckets for pet photos
INSERT INTO storage.buckets (id, name, public)
VALUES 
  ('pet-photos', 'pet-photos', true),
  ('medical-documents', 'medical-documents', false);
```

### 4. Authentication Setup

Enable authentication providers in Supabase dashboard:
- Email/Password ✅
- Google OAuth (optional)
- Magic Link (optional)

## 🔧 Environment Variables

Create `.env.local`:
```bash
# Supabase
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key-here
SUPABASE_SERVICE_ROLE_KEY=your-service-key-here

# App
NEXT_PUBLIC_APP_URL=https://your-app.pages.dev

# Optional: Email (Resend)
RESEND_API_KEY=re_xxxxxxxxxxxx

# Optional: Uploadthing (alternative to Supabase Storage)
UPLOADTHING_SECRET=sk_live_xxxxxxxxxxxx
UPLOADTHING_APP_ID=xxxxxxxxxxxx
```

## 📱 Next.js App Configuration

### 1. Install Dependencies
```bash
npm install @supabase/supabase-js @supabase/auth-helpers-nextjs
npm install @tanstack/react-query zustand
npm install react-hook-form @hookform/resolvers zod
npm install daisyui tailwindcss
```

### 2. Supabase Client Setup
```typescript
// lib/supabase/client.ts
import { createBrowserClient } from '@supabase/ssr'

export function createClient() {
  return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
  )
}
```

### 3. Server-side Client
```typescript
// lib/supabase/server.ts
import { createServerClient, type CookieOptions } from '@supabase/ssr'
import { cookies } from 'next/headers'

export function createClient() {
  const cookieStore = cookies()
  
  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        get(name: string) {
          return cookieStore.get(name)?.value
        },
        set(name: string, value: string, options: CookieOptions) {
          cookieStore.set({ name, value, ...options })
        },
        remove(name: string, options: CookieOptions) {
          cookieStore.set({ name, value: '', ...options })
        },
      },
    }
  )
}
```

## 🚀 Deployment Checklist

### Pre-deployment
- [ ] Test locally with production build: `npm run build && npm run start`
- [ ] Set up Supabase project and database
- [ ] Configure environment variables
- [ ] Test authentication flow
- [ ] Test image uploads
- [ ] Run Lighthouse audit

### Cloudflare Pages
- [ ] Connect GitHub repository
- [ ] Configure build settings
- [ ] Add environment variables
- [ ] Set up custom domain (optional)
- [ ] Enable Web Analytics
- [ ] Configure Page Rules for caching

### Post-deployment
- [ ] Test production site
- [ ] Set up monitoring (Sentry - free tier)
- [ ] Configure backup strategy
- [ ] Set up Supabase database backups
- [ ] Test email notifications
- [ ] Monitor Supabase usage (stay within free tier)

## 📊 Free Tier Optimization Tips

### Supabase Optimization
1. **Database Size**: Use JSONB for flexible fields instead of many columns
2. **Storage**: Compress images before upload (WebP format)
3. **Auth**: Use Supabase Auth instead of custom JWT
4. **Realtime**: Only subscribe to necessary tables
5. **Functions**: Use Edge Functions sparingly (free tier: 500K invocations)

### Cloudflare Optimization
1. **Caching**: Set proper cache headers
2. **Images**: Use Cloudflare Image Optimization
3. **Workers**: Use KV storage for caching
4. **Analytics**: Use Cloudflare Web Analytics (free)

### Code Optimization
```javascript
// Use dynamic imports for large components
const HeavyComponent = dynamic(() => import('./HeavyComponent'), {
  loading: () => <Skeleton />,
  ssr: false
})

// Optimize images
<Image 
  src={petPhoto} 
  alt="Pet"
  width={400}
  height={300}
  loading="lazy"
  quality={75}
/>

// Use React Query for caching
const { data } = useQuery({
  queryKey: ['pets', clientId],
  queryFn: fetchPets,
  staleTime: 5 * 60 * 1000, // 5 minutes
  cacheTime: 10 * 60 * 1000, // 10 minutes
})
```

## 🔍 Monitoring & Analytics (Free)

### 1. Cloudflare Analytics
- Automatic with Cloudflare Pages
- Real User Monitoring
- Core Web Vitals

### 2. Vercel Analytics (Alternative)
```bash
npm install @vercel/analytics
```

### 3. Sentry (Error Tracking)
```bash
npm install @sentry/nextjs
```

Free tier: 5K errors/month

### 4. Supabase Dashboard
- Built-in metrics
- Query performance
- Auth analytics
- Storage usage

## 🆘 Troubleshooting

### Common Issues

#### 1. Build Fails on Cloudflare
```bash
# Use Node 18 or 20
# Add to build command:
NODE_VERSION=20 npx @cloudflare/next-on-pages@1
```

#### 2. Supabase Connection Issues
```javascript
// Add timeout and retry
const supabase = createClient()
supabase.auth.setSession({
  access_token: token,
  refresh_token: refreshToken,
})
```

#### 3. Image Upload Issues
```javascript
// Use Supabase Storage with size limits
const { data, error } = await supabase.storage
  .from('pet-photos')
  .upload(fileName, file, {
    cacheControl: '3600',
    upsert: false,
    contentType: 'image/webp'
  })
```

## 📚 Resources

- [Cloudflare Pages Docs](https://developers.cloudflare.com/pages/)
- [Supabase Docs](https://supabase.com/docs)
- [Next.js on Cloudflare](https://developers.cloudflare.com/pages/framework-guides/nextjs/)
- [DaisyUI Components](https://daisyui.com/components/)
- [Supabase Free Tier](https://supabase.com/pricing)