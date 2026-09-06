# Backend & Data

Databases, ORMs, authentication, caching, object storage, and search. Most
projects pick: one primary database, one data-access layer, one auth approach,
and add caching/search/storage only when a requirement calls for it.

---

## Databases & backend platforms

### PostgreSQL
- **Type:** reference (the database itself; consumed via a host or container)
- **Category:** Relational database
- **What it is:** A mature, standards-strong open-source relational database with rich types (JSONB, arrays, full-text search, geospatial via PostGIS).
- **Problem it solves:** Reliable transactional storage for structured data with room to grow into search, JSON, and geospatial without switching engines.
- **When to use:** The default database for most applications.
- **When NOT to use:** Tiny embedded/local-first apps (SQLite is simpler); pure document workloads at very large scale with no relational needs (consider a document store); key-value caching (Redis).
- **Works with:** Every ORM below; hosts include Neon, Supabase, Railway, Fly.io, RDS, self-hosted.
- **Cost / licensing:** Open-source (PostgreSQL License). Cost comes from hosting.
- **Maintenance / status:** Stable, actively maintained, long track record.
- **Alternatives:** SQLite/libSQL (embedded, local-first), MySQL (also solid; PlanetScale for scale/sharding), MongoDB (document model).
- **Docs:** https://www.postgresql.org/docs
- **Notes for chef:** Default to Postgres unless the project is clearly local-first/embedded. Choose the host based on the deploy target (see below and `services.md`).

### SQLite / libSQL
- **Type:** reference (embedded database)
- **Category:** Relational database (embedded / edge)
- **What it is:** A zero-configuration file-based SQL database. libSQL is an open fork (used by Turso) that adds a server/replica model.
- **Problem it solves:** Simple, fast local storage with no separate database server; edge deployments with read replicas close to users.
- **When to use:** CLIs, desktop apps, local-first apps, small sites, prototypes; edge apps via Turso's embedded replicas; test databases.
- **When NOT to use:** High-write-concurrency multi-user apps on a single file; when you need Postgres-specific features.
- **Works with:** Drizzle, Prisma; Turso for hosted/edge.
- **Cost / licensing:** SQLite is public domain; libSQL is open-source (MIT).
- **Maintenance / status:** Stable (SQLite is famously well-tested); libSQL actively maintained.
- **Alternatives:** Postgres (multi-user server), DuckDB (analytical queries).
- **Docs:** https://www.sqlite.org/docs.html · https://docs.turso.tech
- **Notes for chef:** Great for "small and simple" and for tests. If the app will clearly become a multi-user web service, start on Postgres to avoid a migration later.

### Supabase
- **Type:** service
- **Category:** Backend platform (Postgres + auth + storage + realtime)
- **What it is:** A hosted Postgres database bundled with authentication, row-level-security authorization, auto-generated APIs, realtime subscriptions, object storage, and edge functions.
- **Problem it solves:** Standing up a full backend quickly without building auth, file storage, and an API layer separately.
- **When to use:** MVPs and products that want an integrated backend; apps that benefit from realtime; teams comfortable writing SQL and RLS policies.
- **When NOT to use:** When you only need a database (a plain Postgres host is cheaper/simpler); when you want auth decoupled from your database vendor; workloads needing scale-to-zero compute (Supabase Postgres is generally always-on on paid plans).
- **Works with:** Any frontend framework; Drizzle/Prisma against the Postgres connection; its own JS client for auth/realtime/storage.
- **Cost / licensing:** Open-source core; hosted plans have a free tier then paid tiers (compute + usage). Self-hostable.
- **Maintenance / status:** Stable, actively maintained, large community.
- **Alternatives:** Firebase (document model, Google, less SQL), Neon (Postgres only, serverless/branching), Appwrite (self-host-friendly), Convex (reactive backend, different model), Pocketbase (single-binary, small apps).
- **Docs:** https://supabase.com/docs
- **Notes for chef:** Good default "backend in a box" for small teams. If the project only needs a database, prefer a pure Postgres host and pick auth separately.

### Neon
- **Type:** service
- **Category:** Serverless Postgres
- **What it is:** Managed Postgres that separates storage from compute, enabling instant database branching (copy-on-write), scale-to-zero on idle, and fast provisioning.
- **Problem it solves:** Postgres that fits serverless/preview-deploy workflows — a database branch per pull request, no paying for idle compute.
- **When to use:** Serverless and edge apps; teams that want a DB branch per preview environment; Vercel-hosted projects.
- **When NOT to use:** Apps needing bundled auth/storage/realtime (use Supabase); very latency-sensitive workloads sensitive to cold starts on the free tier.
- **Works with:** Any Postgres client/ORM; strong Vercel integration.
- **Cost / licensing:** Managed service; free tier then usage-based paid plans.
- **Maintenance / status:** Stable, actively maintained (owned by Databricks).
- **Alternatives:** Supabase, Railway/Fly Postgres (always-on), RDS/Cloud SQL (traditional managed).
- **Docs:** https://neon.tech/docs
- **Notes for chef:** Pick when the deploy model is serverless and per-environment branching is valuable. Otherwise a simple always-on Postgres host is fine.

## ORMs / data access

### Drizzle ORM
- **Type:** library
- **Category:** TypeScript ORM / query builder
- **What it is:** A TypeScript-first, SQL-like ORM. Schemas are TS files; queries read like typed SQL; tiny bundle; no separate engine binary.
- **Problem it solves:** Type-safe database access that stays close to SQL and runs anywhere, including edge runtimes.
- **When to use:** TypeScript projects where the team knows SQL; edge/serverless deployments with bundle-size limits; when you want migrations as plain SQL you can read.
- **When NOT to use:** Teams that want a fully abstracted, higher-level API and a visual data browser (Prisma); non-TS stacks.
- **Works with:** Postgres, MySQL, SQLite/libSQL; Next.js, Hono, React Router, etc.; Zod via `drizzle-zod`.
- **Cost / licensing:** Free, open-source (Apache-2.0).
- **Maintenance / status:** Stable, actively maintained, rapidly growing adoption.
- **Alternatives:** Prisma (higher-level, Studio GUI, larger ecosystem), Kysely (pure typed query builder, no schema layer), raw SQL with a driver.
- **Docs:** https://orm.drizzle.team/docs
- **Notes for chef:** Good default for new TypeScript apps, especially edge/serverless. Choose Prisma instead when the team wants maximum abstraction, tutorials, and tooling.

### Prisma
- **Type:** library
- **Category:** TypeScript/Node ORM
- **What it is:** A schema-first ORM: define models in a `schema.prisma` file, generate a typed client, get migrations and a data browser (Studio).
- **Problem it solves:** Approachable, well-documented database access for teams that would rather not write SQL, with strong tooling.
- **When to use:** Teams newer to SQL; projects that value the huge tutorial/answer base and Studio; standard Node server deployments.
- **When NOT to use:** Extreme bundle-size constraints on the edge (Drizzle is far smaller, though Prisma has improved); when you want migrations as hand-written SQL.
- **Works with:** Postgres, MySQL, SQLite, SQL Server, MongoDB; most Node frameworks.
- **Cost / licensing:** ORM is free, open-source (Apache-2.0). Optional paid Prisma platform products (Accelerate, etc.) are not required.
- **Maintenance / status:** Stable, very actively maintained, large ecosystem.
- **Alternatives:** Drizzle (SQL-close, tiny), TypeORM (older, decorator-based), Kysely.
- **Docs:** https://www.prisma.io/docs
- **Notes for chef:** Safe, well-trodden default. Pair the schema with Zod (via a generator or by hand) so validation and DB types stay aligned.

## Authentication

### Better Auth
- **Type:** library
- **Category:** Authentication (self-hosted, TypeScript)
- **What it is:** A framework-agnostic TypeScript auth library. Email/password, OAuth, magic links, passkeys, 2FA, organizations, and RBAC, stored in your own database via an adapter.
- **Problem it solves:** A complete, self-owned auth system without renting identity from a third-party service or hand-rolling sessions.
- **When to use:** TypeScript apps that want to own their user data and auth logic; when Lucia would previously have been chosen.
- **When NOT to use:** Teams that would rather outsource auth UI, compliance, and edge cases entirely (use Clerk/Auth0); non-TS stacks.
- **Works with:** Next.js, React Router, SvelteKit, Hono, etc.; Drizzle/Prisma/Kysely adapters; any SQL database.
- **Cost / licensing:** Free, open-source (MIT). You pay only for your own database.
- **Maintenance / status:** Actively maintained, rising fast as the modern default for self-hosted TS auth.
- **Alternatives:** Auth.js / NextAuth (established, many providers, Next-centric), Clerk (hosted, best DX, paid), Lucia (deprecated — now a learn-to-build-it resource, do not add as a dependency), Supabase Auth (if already on Supabase).
- **Docs:** https://www.better-auth.com/docs
- **Notes for chef:** Default when the requirement is "own our auth in TypeScript". Do not propose Lucia as a library — it was deprecated in 2025; point to it only as a reference for building sessions from scratch.

### Clerk
- **Type:** service
- **Category:** Authentication (hosted / managed)
- **What it is:** A managed authentication and user-management service with prebuilt, customizable UI components (sign-in, sign-up, user profile, organizations).
- **Problem it solves:** Production-grade auth — including MFA, device management, organizations, and compliance surface — with almost no code.
- **When to use:** Teams that want auth done for them; B2B apps needing organizations/teams quickly; fast MVPs where auth is not the differentiator.
- **When NOT to use:** Cost-sensitive projects at scale (per-MAU pricing adds up); when user data must stay in your own database/vendor-independent; offline/self-hosted requirements.
- **Works with:** Next.js (first-class), React, React Router, Expo, and more; backend SDKs for verification.
- **Cost / licensing:** Free tier up to a monthly-active-user limit, then per-MAU paid plans; some features gated to higher tiers.
- **Maintenance / status:** Stable, actively maintained.
- **Alternatives:** Auth0 (enterprise, mature, pricier), Supabase Auth (bundled, cheaper), Better Auth (self-hosted), WorkOS (enterprise SSO/SCIM focus).
- **Docs:** https://clerk.com/docs
- **Notes for chef:** Flag the per-MAU cost model to the user up front. Great for getting to launch; model the bill at expected user counts before committing.

## Caching & queues

### Redis (and Valkey)
- **Type:** service / reference
- **Category:** In-memory data store (cache, queue, rate limiting, sessions)
- **What it is:** An in-memory key-value store used for caching, session storage, rate limiting, leaderboards, pub/sub, and simple queues. Valkey is the community open-source fork.
- **Problem it solves:** Fast ephemeral state and cross-instance coordination that a primary database handles poorly.
- **When to use:** Caching expensive queries/responses; rate limiting; session or job state; anything needing sub-millisecond reads shared across instances.
- **When NOT to use:** As a primary source of truth for durable data; when the app is small enough that in-process caching suffices.
- **Works with:** Every backend language; hosted via Upstash (serverless, HTTP, pay-per-request), Redis Cloud, or self-host.
- **Cost / licensing:** Redis licensing changed in recent years — check current terms; Valkey is BSD-licensed and open. Hosting: Upstash has a free tier then usage-based.
- **Maintenance / status:** Stable, ubiquitous.
- **Alternatives:** Upstash (serverless Redis-compatible), in-memory LRU cache (single instance), a database table for simple queues, dedicated queues (BullMQ on Redis, SQS, Inngest for event workflows).
- **Docs:** https://redis.io/docs · https://upstash.com/docs
- **Notes for chef:** For serverless/edge apps, Upstash's HTTP API avoids connection-pool problems. Do not add Redis until a concrete need (cache, rate limit, queue) exists.

## Object storage

### Amazon S3 / Cloudflare R2
- **Type:** service
- **Category:** Object (file) storage
- **What it is:** Scalable storage for user uploads, generated files, backups, and static assets, accessed over an S3-compatible API. R2 is Cloudflare's S3-compatible store with no egress fees.
- **Problem it solves:** Storing and serving arbitrary files without putting blobs in your database or on a single server's disk.
- **When to use:** Any app with user uploads, exports, media, or large static assets.
- **When NOT to use:** Small numbers of static assets that can just ship with the build/CDN; when a bundled solution (Supabase Storage, UploadThing) is simpler for the scale.
- **Works with:** Any backend via S3 SDKs; presigned URLs for direct browser uploads; CDN in front for delivery.
- **Cost / licensing:** Usage-based (storage + requests + egress). R2 charges no egress fees, which often makes it cheaper for read-heavy media.
- **Maintenance / status:** Stable, industry standard.
- **Alternatives:** Cloudflare R2 (no egress), Backblaze B2 (cheap), Supabase Storage (bundled), UploadThing (developer-friendly upload flow on top of S3), Google Cloud Storage / Azure Blob.
- **Docs:** https://docs.aws.amazon.com/s3 · https://developers.cloudflare.com/r2
- **Notes for chef:** Prefer R2 when media will be served publicly at volume (egress savings). Use presigned URLs so uploads skip your server. For quick MVPs, UploadThing or Supabase Storage reduces setup.

## Search

### Meilisearch / Typesense
- **Type:** service / library (self-hostable)
- **Category:** Full-text / typo-tolerant search
- **What it is:** Open-source search engines built for instant, typo-tolerant, relevance-ranked "search-as-you-type" experiences, with faceting and filtering.
- **Problem it solves:** Good product/content search that `LIKE '%query%'` or basic Postgres full-text cannot deliver (typo tolerance, ranking, facets, speed).
- **When to use:** E-commerce catalog search, docs search, any UI with a prominent search box and relevance/filter needs.
- **When NOT to use:** Low volume where Postgres full-text search (`tsvector`) is enough; when you cannot run/host another service and no managed tier is in budget.
- **Works with:** Any backend; official clients and UI component libraries (InstantSearch-compatible for Typesense/Meilisearch).
- **Cost / licensing:** Open-source (MIT-ish); self-host free, or managed cloud tiers (usage-based).
- **Maintenance / status:** Both stable and actively maintained.
- **Alternatives:** Algolia (best-in-class managed, expensive at scale), Elasticsearch/OpenSearch (heavier, also does logs/analytics), Postgres full-text search (no extra infra), pgvector for semantic/vector search.
- **Docs:** https://www.meilisearch.com/docs · https://typesense.org/docs
- **Notes for chef:** Start with Postgres full-text search if search is secondary. Move to Meilisearch/Typesense when search quality is a real feature. Reserve Algolia for when its managed polish is worth the price.
