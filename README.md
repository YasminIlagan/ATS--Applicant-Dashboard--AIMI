<div align="center">

# 🧭 AIMI — Applicant & Recruitment Management System

**A recruitment and onboarding platform that takes an applicant from
"Applied" to "Hired" — and gives HR one dashboard to run the whole pipeline.**

</div>


---

## 🔎 Overview

AIMI lets applicants track their entire hiring journey in real time — from
submitting an application, through background verification, receiving and
responding to a job offer, submitting employment requirements, and finally
onboarding — all from a single applicant dashboard.

On the other side, HR gets an admin portal to post jobs, review applicants,
move them through each hiring stage, and manage employee records.

---

## ✨ Features

### 👤 Applicant Portal
- 📝 Online application form (PDS/resume upload)
- 📊 Real-time hiring progress tracker with step-by-step status
- 🔍 Background check status updates
- 💼 Job offer viewing with **Accept / Decline**
- 📎 Document requirements upload — with resubmission flow if a document
  is rejected by HR
- 🏁 Onboarding details (start date, address, what to bring)
- 🔔 Notifications

### 🛡️ HR / Admin Portal
- 📌 Manage job postings and applicants
- 🗂️ Review applications, schedule interviews
- ➡️ Move applicants through hiring stages
- 👥 Manage employee records and archives

---

## 🪜 Hiring Pipeline

This is the journey an applicant moves through on their dashboard:

```
 Applied → Background Check → Job Offer → Requirements → Onboarding → Hired
   📝            🔍               💼            📎              🏁         ✅
```

Each step only unlocks once the previous one is genuinely complete —
for example, the **Job Offer** step won't skip ahead to **Requirements**
until the applicant has actually clicked Accept or Decline.

---

## 🧱 Tech Stack

| Layer          | Tech |
|----------------|------|
| Framework      | Next.js 15 (App Router), React 19 |
| Styling        | Tailwind CSS |
| Auth           | NextAuth v5 |
| Database ORM   | Prisma (`@prisma/client`, `@prisma/adapter-pg`) |
| Database       | PostgreSQL (hosted on Supabase) |
| File Storage   | Supabase Storage (resumes, requirement documents) |
| Validation     | Zod, React Hook Form |
| PDF generation | `@react-pdf/renderer` |

---

## ✅ Prerequisites

Before you start, make sure you have:

- **Node.js** 18.18 or later — [download here](https://nodejs.org/)
- **npm** (comes with Node) — or yarn/pnpm/bun if you prefer
- A **Supabase project** (free tier is fine) — you'll need its database
  connection string and API keys
- **Git**

---

## 🚀 Getting Started (Local Setup)

### 1️⃣ Clone the repo

```bash
git clone https://github.com/YasminIlagan/Application--Website--AIMI.git
cd Application--Website--AIMI
```

### 2️⃣ Install dependencies

```bash
npm install
```

### 3️⃣ Set up environment variables

Create a `.env` file in the project root and fill in the values below (get
these from your Supabase project → **Project Settings → API** and
**Project Settings → Database**):

```env
# Postgres connection string
# (Supabase → Project Settings → Database → Connection string → URI)
DATABASE_URL="postgresql://postgres:[YOUR-PASSWORD]@[YOUR-HOST]:5432/postgres"

# Supabase project URL and keys
# (Supabase → Project Settings → API)
NEXT_PUBLIC_SUPABASE_URL="https://xxxxxxxx.supabase.co"
NEXT_PUBLIC_SUPABASE_ANON_KEY="your-anon-public-key"
SUPABASE_SERVICE_ROLE_KEY="your-service-role-key"

# NextAuth — generate one with: openssl rand -base64 32
NEXTAUTH_SECRET="your-random-secret"
```

> ⚠️ **Never commit your `.env` file.** It's already covered by `.gitignore`.

### 4️⃣ Sync the database schema

This project uses Prisma with an existing Supabase database, so generate the
client and make sure your schema is in sync:

```bash
npx prisma generate
npm run db:push
```

Optional — seed the database with sample data:

```bash
npm run db:seed
```

### 5️⃣ Run the development server

```bash
npm run dev
```

Open **[http://localhost:3000](http://localhost:3000)** in your browser.
The app hot-reloads as you edit files. 🎉

---

## 📜 Available Scripts

| Command           | Description                            |
|-------------------|-----------------------------------------|
| `npm run dev`     | Start the local development server      |
| `npm run build`   | Build the app for production            |
| `npm run start`   | Run the production build locally        |
| `npm run lint`    | Run ESLint                               |
| `npm run db:push` | Push the Prisma schema to the database  |
| `npm run db:seed` | Seed the database with sample data      |

---

## 📁 Project Structure

```
src/
├─ app/
│  ├─ (auth)/login/       # Login page
│  ├─ apply/               # Public job application form
│  ├─ application/         # Application status page
│  ├─ dashboard/           # Applicant dashboard (hiring progress tracker)
│  ├─ admin/               # HR/admin panel (applicants, job postings, employees)
│  ├─ notifications/       # Notifications page
│  └─ api/                 # API routes (applications, requirements,
│                             background-check, pds, notifications, etc.)
├─ components/             # Shared UI + dashboard/admin components
├─ lib/                    # Prisma client, Supabase clients, helpers
└─ types/                  # Shared TypeScript types
prisma/
└─ schema.prisma           # Database schema
```

---

## 🚢 Deployment

The easiest way to deploy is with [Vercel](https://vercel.com/new):

1. Push this repo to GitHub.
2. Import the project into Vercel.
3. Add the same environment variables from `.env` in Vercel's
   **Project Settings → Environment Variables**.
4. Deploy — Vercel automatically rebuilds on every push to `main`.

---

## 🛠️ Troubleshooting

| Problem | Fix |
|---|---|
| Prisma errors about missing tables/columns | Run `npx prisma generate` again after any schema change, and confirm `DATABASE_URL` points to the right database. |
| File uploads failing | Check that `SUPABASE_SERVICE_ROLE_KEY` is set correctly and that the relevant storage bucket exists in Supabase. |
| Login not working | Confirm `NEXTAUTH_SECRET` is set — without it, NextAuth sessions won't work correctly. |

---

<div align="center">

</div>