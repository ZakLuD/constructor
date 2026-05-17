# Nova Builder

Privacy-first full-stack Website Builder, CMS, and SaaS admin platform.

The project is a production-ready MVP scaffold for a Webflow + Framer + Google Sites + Shopify Admin + Notion style builder. Public visitors only receive published site content. Builder, CMS, database, integrations, API tools, roles, logs, and admin workflows live in a separate admin mode gated by approved accounts and RBAC.

## Stack

- Frontend: Next.js, React, TypeScript, TailwindCSS, Framer Motion
- Backend: Express, Node.js, JWT auth, WebSocket realtime channel
- Database: PostgreSQL, Prisma ORM
- Platform: Docker Compose, PWA, plugin SDK, REST API
- Privacy: no analytics cookies, no telemetry, no fingerprinting, no device tracking

## Apps

```txt
apps/web      Next.js public site, login/apply pages, admin GUI, PWA shell
apps/api      Express API, auth, RBAC, CMS, builder, integrations, workflows
packages/shared      Role, permission, block, and integration definitions
packages/plugin-sdk  Typed plugin manifest and runtime contract
prisma        PostgreSQL schema and demo seed
docs          Architecture, API examples, deployment notes
```

## Quick Start

```bash
cp .env.example .env
npm install
npm run prisma:generate
npm run prisma:migrate
npm run prisma:seed
npm run dev
```

Open:

- Public site: http://localhost:3000
- Admin login: http://localhost:3000/login
- API health: http://localhost:4000/health

Demo account after seed:

```txt
login: superadmin
password: ChangeMe!2026
```

Set `SEED_ADMIN_PASSWORD` before seeding if you want a different initial password.

## Docker

```bash
cp .env.example .env
docker compose up --build
```

## Free Public Deployment

For a free hosted setup without Render, use:

- Vercel Hobby for `apps/web` and `/api/*` serverless API
- Neon Free Postgres for `DATABASE_URL`

The project includes:

```txt
vercel.json
docs/FREE_DEPLOYMENT_RU.md
```

Follow [docs/FREE_DEPLOYMENT_RU.md](docs/FREE_DEPLOYMENT_RU.md) to publish the site for other users.

Then run migrations and seed inside the API container or from the host:

```bash
npm run prisma:deploy
npm run prisma:seed
```

## Public Mode

`/` renders only published site data from:

```txt
GET /api/public/site/main
```

No builder controls, admin navigation, editor panels, API manager, logs, or CMS widgets are rendered in public mode. The public page also excludes visible admin-entry UI.

## Admin Mode

Admin routes live under `/admin/*` and are wrapped by the admin shell plus route-level `AdminGate` permission checks. API access is enforced server-side with `requireAuth` and `requirePermission`.

Included admin GUI:

- Dashboard widgets
- Visual drag-and-drop builder
- CMS content manager
- Component system
- Menu builder
- Popup builder
- Database editor
- API manager and request tester
- Google integrations
- Automation builder
- Privacy-safe analytics
- Theme editor
- Media and file managers
- User and role managers
- Activity logs
- Notification center
- AI assistant surface
- Plugin marketplace
- Backup/export manager
- SEO manager
- Form builder
- Email builder
- Realtime collaboration surface
- Settings, localization, RTL, PWA controls

## Auth Model

The platform intentionally avoids personal data collection.

Accounts use:

- Login
- Password
- Approval status
- Role

No email, phone, analytics cookies, device IDs, IP address audit metadata, user-agent logs, telemetry, or fingerprinting are required by the app code.

Access flow:

1. User opens `/apply`.
2. User submits login and password.
3. Account is created as `PENDING`.
4. Admin approves the account and assigns a role.
5. Approved account can sign in at `/login`.

If there are no users yet, the first application is bootstrapped as `SUPER_ADMIN`.

## RBAC

Roles:

- Super Admin
- Admin
- Moderator
- Editor
- Viewer
- User

Permission groups cover dashboard, builder, CMS, database, API manager, integrations, automations, theme, media, users, roles, logs, settings, plugins, backups, notifications, and AI.

Definitions live in:

```txt
packages/shared/src/index.ts
```

## Google Integrations

The integrations page and API support GUI configuration for:

- Google Sheets
- Google Apps Script
- Google Drive
- Google Forms
- Gmail API
- Google Calendar
- Google Maps
- Firebase
- Google OAuth

Live OAuth requires:

```txt
GOOGLE_CLIENT_ID
GOOGLE_CLIENT_SECRET
GOOGLE_REDIRECT_URI
```

Secrets should be stored by reference (`secretRef`) instead of raw values in project records.

## Plugin SDK

Plugins are typed with:

```ts
import { definePlugin } from "@cms/plugin-sdk";
```

Plugin manifests can declare placements such as sidebar, builder panel, automation node, API connector, and theme token.

## Privacy Guarantees

The codebase is designed around these defaults:

- No analytics package is installed.
- No tracking script is included.
- No telemetry is sent.
- No fingerprinting APIs are used.
- Admin audit logs store action metadata only.
- CORS does not use credentialed analytics cookies.
- PWA cache stores shell assets, not personal analytics.

## Useful Commands

```bash
npm run dev
npm run build
npm run typecheck
npm run prisma:migrate
npm run prisma:seed
```
