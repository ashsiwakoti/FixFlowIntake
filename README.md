# FixFlow — Appliance Repair Dispatch Platform

A full-stack intake and dispatch system for appliance repair companies servicing property managers. Property managers get a custom portal URL to submit maintenance work orders, which are triaged by AI and routed for dispatch.

## Live Demo

[fixflow-intake.vercel.app](https://fixflow-intake.vercel.app)

> Each property manager gets a unique dispatch URL (e.g., `/dispatch/alpha-properties`)

[fixflow-intake.vercel.app](https://fixflow-intake.vercel.app/dispatch/alpha-properties)

## Features

- **Multi-Tenant Dispatch Portals** — Each property manager gets a branded, slug-based intake URL
- **Work Order Intake** — Captures tenant info, unit number, phone, and issue description with real-time validation
- **AI Triage** — Automatically analyzes issue descriptions using GPT-4o-mini to predict appliance type and likely failure points
- **Status Workflow** — Work orders flow through: Received → Scheduling → Dispatched → Completed → Invoiced
- **SMS & Email Notifications** — Tenant SMS updates (Twilio) and PM email alerts (Resend)
- **Stripe Billing** — Subscription-based billing for property managers

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Next.js 16 (App Router) |
| Language | TypeScript |
| Styling | Tailwind CSS 4 |
| Database | Supabase (PostgreSQL) |
| Auth | Supabase RLS + Service Role |
| AI | OpenAI GPT-4o-mini |
| SMS | Twilio |
| Email | Resend |
| Payments | Stripe |
| Hosting | Vercel |

## Architecture

```
┌─────────────────────────────────────────────────┐
│                   Vercel (Edge)                  │
│  ┌───────────────────────────────────────────┐   │
│  │           Next.js App Router              │   │
│  │  ┌─────────────┐  ┌───────────────────┐   │   │
│  │  │  /dispatch/  │  │  Server Actions   │   │   │
│  │  │  [slug]      │──│  (form submit)    │   │   │
│  │  └─────────────┘  └───────┬───────────┘   │   │
│  └────────────────────────────┼───────────────┘   │
└───────────────────────────────┼───────────────────┘
                                │
                 ┌──────────────┼──────────────┐
                 │              │              │
          ┌──────▼──────┐ ┌────▼────┐  ┌──────▼──────┐
          │  Supabase   │ │ OpenAI  │  │   Twilio /  │
          │ (Postgres)  │ │ (Triage)│  │   Resend    │
          └─────────────┘ └─────────┘  └─────────────┘
```

## Screenshots

<!-- Add screenshots here -->
![Intake Form](screenshots/intake-form.png)
![Success Page](screenshots/success.png)

## How It Works

1. **Property manager** is onboarded and receives a custom dispatch URL
2. **Tenant or PM** visits the URL and submits a work order describing the appliance issue
3. **AI triage** analyzes the description, predicts the appliance type and top failure points
4. **Notifications** are sent — SMS to the tenant, email to the PM
5. **Dispatch team** manages the work order through the status workflow

## Status

This is a production application. The source code is in a private repository.
