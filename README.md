# AIMI Recruitment System — Applicant Portal

A web-based recruitment and onboarding platform developed for Arvin International Marketing Inc. (AIMI). The system guides an applicant through the complete hiring process — from application submission to onboarding — while providing HR with a separate dashboard to manage the pipeline.

---

## Overview

The AIMI Recruitment System allows applicants to track their hiring progress in real time, from submitting an application, through background verification, receiving and responding to a job offer, submitting employment requirements, and completing onboarding.

On the administrative side, HR staff are provided with a separate portal to post job openings, review applicants, manage each stage of the hiring process, and maintain employee records.

This document covers the **applicant-facing portal**.

---

## Features

### Applicant Portal

- Online application form with document upload (Personal Data Sheet, resume)
- Real-time hiring progress tracker
- Background check status updates
- Job offer review with Accept / Decline options
- Document requirements upload, with resubmission if a document is rejected by HR
- Onboarding details (start date, address, requirements to bring)
- Notifications

### HR / Admin Portal

- Management of job postings and applicants
- Review of applications and interview scheduling
- Progression of applicants through hiring stages
- Management of employee records

---

## Hiring Pipeline

The applicant progresses through the following stages:

Application → Background Check → Job Offer → Requirements → Onboarding → Hired

Each stage unlocks only once the previous stage has been completed. For example, the Requirements stage is not accessible until the applicant has responded to the job offer.

---

## Technology Stack

| Layer          | Technology                                        |
| -------------- | -------------------------------------------------- |
| Framework      | Next.js 15 (App Router), React 19                  |
| Styling        | Tailwind CSS                                       |
| Authentication | Auth.js v5 (NextAuth)                              |
| Database ORM   | Prisma (@prisma/client, @prisma/adapter-pg)        |
| Database       | PostgreSQL, hosted on Supabase                     |
| File Storage   | Supabase Storage (resumes, requirement documents)  |
| Validation     | Zod, React Hook Form                               |
| PDF Generation | @react-pdf/renderer                                |

---

## Prerequisites

Before setting up the project locally, ensure the following are installed:

- Node.js version 18.18 or later
- npm (included with Node.js)
- Git
- Access to the project's Supabase instance (database connection string and API keys)

---

## Local Setup Instructions

### 1. Clone the Repository

```
git clone https://github.com/YasminIlagan/Application--Website--AIMI.git
cd Application--Website--AIMI
```

### 2. Install Dependencies

```
npm install
```

### 3. Configure Environment Variables

Create a `.env.local` file in the project root directory. Required values can be obtained from the Supabase project under Project Settings → API and Project Settings → Database.

```
# PostgreSQL connection string
# (Supabase → Project Settings → Database → Connection String → URI)
DATABASE_URL="postgresql://postgres:[YOUR-PASSWORD]@[YOUR-HOST]:5432/postgres"

# Supabase project URL and keys
# (Supabase → Project Settings → API)
NEXT_PUBLIC_SUPABASE_URL="https://xxxxxxxx.supabase.co"
NEXT_PUBLIC_SUPABASE_ANON_KEY="your-anon-public-key"
SUPABASE_SERVICE_ROLE_KEY="your-service-role-key"

# Authentication secret
# Generate with: openssl rand -base64 32
NEXTAUTH_SECRET="your-random-secret"
```

**Note:** The `.env.local` file must never be committed to version control. It is already excluded via `.gitignore`.

### 4. Synchronize the Database Schema

This project connects to an existing, shared Supabase database. Since the schema already exists, the recommended step is to retrieve the current schema rather than push local changes:

```
npx prisma generate
npx prisma db pull
```

**Caution:** The `db:push` script should only be used when intentionally introducing new schema changes to the shared database. Coordinate with team members before running this command, as it directly affects the live, shared database.

To seed the database with sample data (optional):

```
npm run db:seed
```

### 5. Run the Development Server

```
npm run dev
```

The application will be available at `http://localhost:3000`.

---

## Available Scripts

| Command           | Description                                                                  |
| ----------------- | ------------------------------------------------------------------------------ |
| `npm run dev`     | Starts the local development server                                          |
| `npm run build`   | Builds the application for production                                        |
| `npm run start`   | Runs the production build locally                                            |
| `npm run lint`    | Runs ESLint                                                                   |
| `npm run db:push` | Pushes local Prisma schema changes to the shared database (use with caution)  |
| `npm run db:seed` | Seeds the database with sample data                                          |

---

## Project Structure

```
src/
├─ app/
│  ├─ (auth)/login/       Login page
│  ├─ apply/              Public job application form
│  ├─ application/        Application status page
│  ├─ dashboard/          Applicant dashboard (hiring progress tracker)
│  ├─ admin/              HR/admin panel (applicants, job postings, employees)
│  ├─ notifications/      Notifications page
│  └─ api/                API routes (applications, requirements,
│                          background-check, PDS, notifications, etc.)
├─ components/            Shared UI and dashboard/admin components
├─ lib/                   Prisma client, Supabase clients, helper functions
└─ types/                 Shared TypeScript type definitions
prisma/
└─ schema.prisma          Database schema
```

---

## Deployment

The recommended deployment platform is Vercel:

1. Push the repository to GitHub.
2. Import the project into Vercel.
3. Add the same environment variables from `.env.local` under Vercel's Project Settings → Environment Variables.
4. Deploy. Vercel will automatically rebuild the application on every push to the `main` branch.

---

## Troubleshooting

| Issue                                        | Resolution                                                                                          |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Prisma errors regarding missing tables or columns | Run `npx prisma db pull` to synchronize the local schema with the live database, then `npx prisma generate`. |
| File uploads failing                          | Verify that `SUPABASE_SERVICE_ROLE_KEY` is correctly configured and that the relevant storage bucket exists in Supabase. |
| Authentication not functioning correctly      | Confirm that `NEXTAUTH_SECRET` is set in `.env.local`.                                              |
