# System & Tech

## Stack
- Framework: [e.g. Next.js 15.2]
- Language: [TypeScript 5.7]
- Database: [PostgreSQL 16 + Drizzle 0.38]
- Auth: [Clerk / NextAuth]
- Hosting: [Vercel]

## Architecture
- [High-level pattern, e.g. "Modular monolith with feature-based folders"]
- Data flow: [Brief description]

## Decisions (append-only, newest top)
| Date | Decision | Rationale |
|------|----------|-----------|
| 2026-08-14 | Use Zod over Yup | Better TypeScript inference, smaller bundle |
| 2026-08-10 | tRPC over REST | End-to-end typesafety for solo speed |

## Patterns
- [Pattern name]: [Where used, e.g. "Repository pattern in src/lib/db/"]