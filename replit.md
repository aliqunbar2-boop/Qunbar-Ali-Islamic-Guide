# Islamic Masail Finder

A premium Islamic knowledge platform where Muslims can search Islamic questions (masail) and find authoritative answers. Features a searchable database, AI assistant, and admin panel.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 8080, served at /api)
- `pnpm --filter @workspace/islamic-masail run dev` — run the frontend (served at /)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Frontend: React + Vite, Tailwind CSS, Framer Motion, wouter routing
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (zod/v4), drizzle-zod
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `lib/api-spec/openapi.yaml` — OpenAPI spec (source of truth for all API contracts)
- `lib/db/src/schema/categories.ts` — Categories table schema
- `lib/db/src/schema/masail.ts` — Masail table schema
- `artifacts/api-server/src/routes/` — Express route handlers (categories, masail, ai)
- `artifacts/islamic-masail/src/pages/` — Frontend pages (public/ and admin/)
- `artifacts/islamic-masail/src/index.css` — Islamic theme (green/gold palette)

## Features

- **Homepage**: Bismillah hero, search bar, stats banner (36 masail, 12 categories), latest masail
- **12 Categories**: Roza, Zakat, Namaz, Wuzu, Taharat, Quran, Hadith, Nikah, Talaq, Hajj, Business Masail, Daily Life Masail
- **Search**: Full-text search across English and Urdu questions and answers
- **AI Assistant**: Searches database first, shows related masail; if not found, politely refers to a scholar
- **Admin Panel**: Password-protected (admin123), full CRUD for masail and categories

## Architecture decisions

- OpenAPI-first: all API contracts in `lib/api-spec/openapi.yaml`, frontend uses generated hooks
- AI assistant is database-search-based (no external LLM required) — searches by keyword and full-text match
- Urdu text uses Noto Nastaliq Urdu font (Google Fonts), rendered RTL
- Admin auth is localStorage-based (password: admin123) — suitable for a single-operator setup
- Routes: `/api/*` → API server, `/` → React frontend

## Product

Islamic knowledge platform with 36 sample masail across 12 categories. Users can browse categories, search in English or Urdu, view full masail with references, and ask the AI assistant. Admins can add/edit/delete masail and categories.

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

- After OpenAPI spec changes, always run codegen before building the frontend
- Urdu text columns in DB: `question_urdu`, `answer_urdu`, `name_urdu` (snake_case in DB, camelCase in TypeScript)
- The AI route is at `/api/ai/ask` (POST)

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
