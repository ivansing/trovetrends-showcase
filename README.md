<p align="center">
  <img src="docs/logo/trovetrends-logo-color.svg" alt="TroveTrends" width="280" />
</p>

<h1 align="center">TroveTrends — Content Platform with Email Marketing & Admin Dashboard</h1>

[![Live](https://img.shields.io/badge/live-trovetrends.com-success.svg)](https://trovetrends.com)
[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](#)
[![Next.js](https://img.shields.io/badge/Next.js-14-black?logo=next.js&logoColor=white)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Tailwind](https://img.shields.io/badge/Tailwind-3-38BDF8?logo=tailwindcss&logoColor=white)](https://tailwindcss.com/)
[![Firebase](https://img.shields.io/badge/Firebase-Firestore-FFCA28?logo=firebase&logoColor=black)](https://firebase.google.com/)
[![SendGrid](https://img.shields.io/badge/SendGrid-Email-1A82E2?logo=sendgrid&logoColor=white)](https://sendgrid.com/)
[![License](https://img.shields.io/badge/license-Proprietary-red.svg)](LICENSE)

---

> **This is a product showcase repository.** It contains documentation and architecture for TroveTrends, a proprietary content platform developed by [ByteUp LLC](https://byteup.co). The **public site is live at [trovetrends.com](https://trovetrends.com)**. Source code is not included. For licensing inquiries, see [Licensing](#licensing) below.

## About

**TroveTrends** is a production-grade, SEO-optimized content platform for technology, software, health, and lifestyle audiences, with integrated email marketing, lead generation, and affiliate infrastructure.

It ships as **two applications** working in concert:

1. **TroveTrends Frontend** — the public-facing content site at [trovetrends.com](https://trovetrends.com). Next.js 13.5+ with App Router, SSR/ISR for SEO, Firebase Firestore for data, SendGrid for transactional email, full-text search, trending articles, and lead-capture flows.
2. **TroveTrends Admin** — the internal content management dashboard. Next.js 14 with Firebase Auth and RBAC, article CRUD with a rich content-block system, email campaign management, subscriber analytics, and SEO metadata control.

Live, deployed, and actively maintained on Vercel.

## Key Features

### Public Content Site

- **SEO-first architecture** — Server-side rendering and incremental static regeneration on every article and category page, structured data (JSON-LD) for rich snippets, dynamic sitemaps, RSS feeds
- **Full-text search** — Article search API with relevance ranking
- **Trending & Featured articles** — Dynamic trending content based on engagement metrics
- **Multi-category browsing** — Technology, software, health & lifestyle, finance & investing
- **Newsletter subscription** — Double opt-in flow with confirmation emails, SendGrid-backed
- **Contact form** — Rate-limited, Zod-validated, Firebase-stored with notification emails
- **Finance calculator** — Interactive finance calculator with custom React hooks
- **Interactive quiz** — Lead-magnet quiz with result-driven content recommendations
- **Reading tracker** — Client-side engagement tracking for personalization signals
- **Popup lead magnets** — Delayed-trigger newsletter popups (45s engagement threshold)
- **Maintenance mode** — Built-in middleware for scheduled site maintenance windows
- **Secure PDF downloads** — JWT-protected PDF delivery for gated content

### Admin Dashboard

- **Firebase Authentication** — Email/password and Google Sign-In with custom claims for RBAC
- **Role-Based Access Control** — Firebase custom claims gate admin vs. editor access
- **Article Management** — Full CRUD with a block-based content editor supporting:
  - Headings, paragraphs, quotes, lists
  - Images with captions and alt text
  - Call-to-action buttons with affiliate tracking
  - Tables, embedded videos
- **SEO metadata management** — Per-article meta title, description, Open Graph tags
- **Editorial workflow** — Draft → Published → Archived status transitions
- **Featured article designation** — Homepage hero and featured slots
- **Email Marketing** — Complete campaign management:
  - Email templates with brand-aligned design
  - Subscriber list management and segmentation
  - Campaign scheduling and analytics
- **Contact & lead management** — Subscriber and lead tracking with source attribution
- **Analytics dashboard** — Performance metrics, engagement statistics, conversion tracking
- **View count & affiliate click tracking** — Article-level engagement and revenue metrics

## Architecture

### Public Site

<p align="center">
  <img src="docs/diagrams/system-architecture-frontend.drawio.png" alt="TroveTrends Frontend Architecture" />
</p>

The frontend follows a clean four-layer architecture:

- **Client Layer** — React 18 components (SSR-hydrated), client-side state via React hooks, Google Analytics and Pinterest Pixel for engagement tracking
- **Application Layer** — Next.js 13.5 App Router with Server Components (default) and Client Components (for interactive UI), API route handlers under `app/api/*`, middleware for maintenance mode and cross-cutting concerns
- **Data Layer** — Firebase Firestore (articles, newsletter subscribers, contacts, system settings) with real-time synchronization and security rules enforced at the database level
- **Service Layer** — SendGrid for transactional emails (confirmation, welcome, unsubscribe), analytics providers for engagement tracking

### Admin Dashboard

<p align="center">
  <img src="docs/diagrams/system-architecture-admin.drawio.png" alt="TroveTrends Admin Architecture" />
</p>

The admin dashboard uses Next.js 14 with a separate Firebase project scoping, Firebase Auth with custom claims for RBAC, and the same Firestore collections as the frontend (read/write separation enforced by security rules).

### Rendering Strategy

| Page Type        | Rendering | Reason                         |
| ---------------- | --------- | ------------------------------ |
| Article pages    | SSR + ISR | SEO optimization, fast loads   |
| Category pages   | SSR + ISR | SEO optimization               |
| Finance calculator | CSR     | Heavy interactivity            |
| Interactive quiz | CSR       | Client-side state management   |
| API routes       | Server    | Data operations                |

**Why this mix:** Content pages are SSR for SEO (crawlers get fully-rendered HTML), interactive features are CSR (no point in SSR for a calculator), and API routes run server-only to keep Firebase Admin SDK calls off the client bundle.

## Tech Stack

### Frontend (trovetrends.com)

| Category           | Technology                                    |
| ------------------ | --------------------------------------------- |
| Framework          | Next.js 13.5+ (App Router, SSR/ISR)           |
| Language           | TypeScript 5.2 (strict mode)                  |
| Styling            | Tailwind CSS 3.3 + shadcn/ui (Radix primitives) |
| Icons              | Lucide React                                  |
| Database           | Firebase Firestore (Client SDK + Admin SDK)   |
| Email              | SendGrid (transactional)                      |
| Forms              | React Hook Form + Zod validation              |
| Data Visualization | Recharts, Embla Carousel                      |
| Testing            | Jest + ts-jest                                |
| Deployment         | Vercel (standalone output)                    |

### Admin Dashboard

| Category           | Technology                                       |
| ------------------ | ------------------------------------------------ |
| Framework          | Next.js 14 (App Router)                          |
| Language           | TypeScript 5                                     |
| Styling            | Tailwind CSS + shadcn/ui                         |
| Auth               | Firebase Authentication + custom claims (RBAC)   |
| Database           | Firebase Firestore (Admin SDK, server-only writes) |
| Email              | SendGrid (campaigns, templates, analytics)      |
| Forms              | React Hook Form + Zod                            |
| Testing            | Jest + React Testing Library                     |
| Deployment         | Vercel                                           |

## Engineering Highlights

### SEO-First Architecture

Every public page is either SSR or ISR so that search engine crawlers receive fully-rendered HTML. Dynamic sitemaps are generated server-side from Firestore content. Structured data (JSON-LD) is emitted for articles, ensuring rich snippets in search results. The site ranks on Google for target keywords in health, finance, and technology verticals.

### Rate-Limited API Routes

The newsletter signup, contact form, and search endpoints are rate-limited to prevent abuse. Implementation is middleware-based so new API routes inherit rate limiting by default without per-route code.

### Double Opt-In Newsletter Flow

Newsletter subscription uses a verified double opt-in pattern:

1. User submits email → server validation (format + duplicate check)
2. Subscriber created in Firestore with `pending_confirmation` status + confirmation token
3. Confirmation email dispatched via SendGrid with tokenized link
4. User clicks link → status updated to `confirmed`, welcome email sent

This ensures only real, owned email addresses enter the subscriber base — critical for deliverability and CAN-SPAM compliance.

### Content Block System (Admin)

Articles aren't stored as raw HTML or Markdown. Instead, each article is a structured sequence of **content blocks** (heading, paragraph, image, CTA, table, list, quote, embed) with typed metadata. This lets the admin dashboard render a rich editor per block type while keeping the data schema consistent and machine-readable.

**Why this matters:** A future headless API, mobile app, or AI summarizer can walk the block structure without parsing HTML.

### JWT-Protected PDF Downloads

Gated PDF content (lead magnets, ebooks) is served through a JWT-token-based download flow. The token is generated server-side with a short expiry, bound to the requesting user or form submission, and validated before serving the file. Prevents hotlinking and enforces gating.

### Firestore Security Rules

All write access to Firestore is restricted to server-side operations (Admin SDK). The client SDK has read-only access to public collections, enforced by Firestore security rules at the database layer. Even if the client bundle is tampered with, writes are impossible from the browser.

### Performance Optimizations

- **SWC minification** for faster builds and smaller bundles
- **`next/image`** with automatic format negotiation and responsive sizing
- **Standalone output** for containerized deployments
- **Server Components by default** to reduce client-side JavaScript
- **Dynamic imports** for heavy components (calculator, quiz, Recharts)
- **Static generation** where content is stable
- **Firestore real-time listeners** with local caching for dynamic data

## By the Numbers

| Metric                    | Value                                              |
| ------------------------- | -------------------------------------------------- |
| Apps                      | 2 (Public site + Admin dashboard)                  |
| API Route Handlers        | 12+ (frontend) + full admin API                    |
| Firestore Collections     | `articles`, `newsletter_subscribers`, `contacts`, `systemSettings`, `subscribers_campaigns` |
| Email Templates           | Confirmation, Welcome, Unsubscribe, Campaign templates |
| Article Block Types       | 8+ (heading, paragraph, image, CTA, table, list, quote, embed) |
| RBAC                      | Firebase custom claims (admin, editor roles)       |
| Deployment                | Vercel (production + preview environments)         |

## Documentation

Full architecture documentation is included as downloadable PDFs:

- [**TroveTrends Frontend Architecture**](docs/TroveTrends_Frontend_Architecture.pdf) — 9-page deep-dive covering technology stack, system architecture, directory structure, data flow, state management, rendering strategy, external integrations, and performance
- [**TroveTrends Admin Architecture**](docs/TroveTrends_Admin_Architecture.pdf) — Admin dashboard architecture, authentication flow, article management design, database schema, deployment guide

The internal documentation library also includes:

- Component reference
- API reference
- Security model
- Development guide
- Deployment guide
- Database schema
- Article management design

## Security

### Authentication (Admin)

- Firebase Authentication (email/password + Google OAuth)
- Firebase custom claims for role-based access
- JWT verification on every admin API endpoint
- Secure session management

### Data Protection

- **Firestore security rules** restrict all writes to server-side operations
- Client SDK is read-only for public collections
- Admin SDK runs exclusively on Next.js API routes
- Environment variables for all API keys (never in client bundles)

### API Hardening

- Rate limiting on public endpoints (newsletter, contact, search)
- Zod schema validation on every request body
- Input validation at client, API, and database layers
- CAN-SPAM compliant unsubscribe flow
- HTTPS-only production traffic via Vercel

## Skills Demonstrated

| Area                  | Details                                                                    |
| --------------------- | -------------------------------------------------------------------------- |
| Full-Stack Next.js    | App Router, SSR/ISR, Server Components, middleware, API routes             |
| SEO Engineering       | Structured data, sitemaps, RSS, meta tags, SSR-first architecture         |
| Firebase              | Firestore (Client + Admin SDK), Auth with custom claims, security rules   |
| Email Infrastructure  | SendGrid transactional, campaign management, double opt-in, unsubscribe   |
| RBAC                  | Firebase custom claims, JWT verification, role-based UI gating            |
| Form Handling         | React Hook Form + Zod, rate-limited submission, server validation         |
| Content Architecture  | Block-based editor, typed schemas, headless-API-ready data model          |
| Performance           | SSR + ISR, dynamic imports, image optimization, SWC, caching strategy     |
| DevOps                | Vercel deployment, standalone output, environment management              |
| Testing               | Jest, ts-jest, component testing                                          |
| Documentation         | Architecture PDFs, API references, component docs, security model         |

## About ByteUp

TroveTrends is a product of **[ByteUp LLC](https://byteup.co)**, a software studio building SaaS products for underserved markets and delivering contract development for clients who value clean architecture and reliable code.

### ByteUp products

| Product                   | Status             | Link                                                                                        |
| ------------------------- | ------------------ | ------------------------------------------------------------------------------------------- |
| **TroveTrends**           | Live               | You are here · [trovetrends.com](https://trovetrends.com)                                   |
| **FixDoc**                | Active development | [fixdoc-showcase](https://github.com/ivansing/fixdoc-showcase) — Multi-tenant doc verification SaaS |
| **Migratex**              | Production-ready   | [migratex-showcase](https://github.com/ivansing/migratex-showcase) — Excel → PostgreSQL ETL CLI |
| **Stripe Payment Links**  | Production-ready   | [stripe-payment-links-showcase](https://github.com/ivansing/stripe-payment-links-showcase) — FastAPI payment API |

### Author

Built and maintained by **Ivan Duarte**, Full-Stack Software Engineer and founder of ByteUp LLC. Based in Bogotá, Colombia.

- Company: [byteup.co](https://byteup.co) · contact@byteup.co
- GitHub: [@ivansing](https://github.com/ivansing)
- LinkedIn: [lance-dev](https://linkedin.com/in/lance-dev)

## Licensing

TroveTrends is proprietary software developed by ByteUp LLC. The source code of both the frontend and admin applications is not included in this repository and is not available under any open-source license. The live site at trovetrends.com is publicly accessible for content, but the underlying code and admin dashboard are not.

This repository contains documentation and marketing materials only. The content (README, architecture documentation, diagrams, PDFs) is provided for informational and portfolio purposes.

**For commercial licensing, white-label inquiries, or custom content-platform development, contact contact@byteup.co.**

---

*Document Version: 1.0 · Product: TroveTrends · Status: Live at [trovetrends.com](https://trovetrends.com)*
