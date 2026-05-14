# 🗳️ Wapenamanda AI Voter Tracking System

This is a secure, cloud-native implementation of the voter tracking system for Wapenamanda District.

## 🚀 Cloud Deployment Guide

Since the local environment has disk limitations, this project is designed to be deployed directly to the cloud.

### 1. Database Setup (Neon.tech or Supabase)
1. Create a free account at [Neon.tech](https://neon.tech) or [Supabase](https://supabase.com).
2. Create a new PostgreSQL project.
3. Copy the **Connection String** (e.g., `postgres://user:password@host/dbname`).
4. Create a `.env` file in the root of the project:
   ```env
   DATABASE_URL="your_connection_string_here"
   NEXTAUTH_SECRET="any-random-long-string"
   NEXTAUTH_URL="https://your-app-name.vercel.app"
   ```

### 2. Deploy to Vercel
1. Push this code to a GitHub repository.
2. Connect your GitHub account to [Vercel](https://vercel.com).
3. Import the `voter-tracker` repository.
4. Add the **Environment Variables** defined in Step 1 to the Vercel project settings.
5. Click **Deploy**.

### 3. Database Migration
Once deployed, you need to push the Prisma schema to your cloud database:
Run this command in your local terminal (if possible) or via a GitHub Action:
```bash
npx prisma migrate deploy
```
*Alternatively, you can use the Prisma Studio in the cloud to create the tables manually based on the `schema.prisma` file.*

## 🛠️ Architecture Summary

### Consoles
- **People Console** (`/people`): Registration and public census viewing.
- **Politician Console** (`/politician`): Private analytics and follower lists.
- **NCT Console** (`/nct`): Central authority for population and candidate management.

### Security
- **RBAC**: Role-Based Access Control ensures that only NCT staff can update populations and only Politicians can see their own voters.
- **Confidentiality**: All aggregation calculations are performed server-side in `src/lib/stats.ts` to prevent leaking numbers via the browser console.
- **Audit Trail**: Every change in the NCT console is logged in the `AuditLog` table.

## 📊 Data Hierarchy
`Ward` $\rightarrow$ `LLG` $\rightarrow$ `District`
Population totals roll up automatically via server-side aggregation.
