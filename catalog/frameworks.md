# Frameworks

App and site frameworks. Pick one primary framework per project; the choice
drives most later decisions. Verify current major version from the official docs
at planning time.

---

### Next.js
- **Type:** framework
- **Category:** Full-stack React (web)
- **What it is:** The most widely used React framework, from Vercel. Renders on the server, ships a file-based router, and supports static, server-rendered, and hybrid pages in one app.
- **Problem it solves:** Building a production React app (marketing + app + API) without wiring up SSR, routing, bundling, and data loading yourself.
- **When to use:** Product apps and content sites that need SEO, a large ecosystem, lots of hiring availability, or first-class hosting on Vercel.
- **When NOT to use:** Purely static content sites where a lighter output is wanted (use Astro); teams that want to avoid Vercel-centric patterns or React Server Components complexity.
- **Works with:** React, Vercel (best), also Netlify/Cloudflare/Node self-host; Tailwind, shadcn/ui, Prisma/Drizzle, Auth.js/Clerk.
- **Cost / licensing:** Open-source (MIT). Hosting is where cost appears.
- **Maintenance / status:** Stable, very actively maintained, fast release cadence (App Router, Turbopack, caching model change often — read the upgrade guide).
- **Alternatives:** React Router v7 (framework mode) for a lighter, less opinionated full-stack React; TanStack Start (newer); Remix v3 (non-React, beta — avoid for production).
- **Docs:** https://nextjs.org/docs
- **Notes for chef:** Default choice for "a web app in React" unless a reason points elsewhere. Warn the user that Next.js caching/rendering defaults shift between majors; pin the version and follow the official upgrade guide.

### Astro
- **Type:** framework
- **Category:** Content-focused sites (web)
- **What it is:** A framework that ships zero JavaScript by default and lets you drop in React/Vue/Svelte components ("islands") only where interactivity is needed.
- **Problem it solves:** Fast, SEO-strong marketing sites, blogs, docs, and landing pages without a heavy client bundle.
- **When to use:** Marketing sites, blogs, documentation, portfolios, mostly-static content with a few interactive widgets.
- **When NOT to use:** Highly interactive app-like products with lots of client state (use Next.js or React Router).
- **Works with:** Any UI framework as islands; MDX; Tailwind; Vercel/Netlify/Cloudflare Pages; headless CMS.
- **Cost / licensing:** Open-source (MIT).
- **Maintenance / status:** Stable, actively maintained.
- **Alternatives:** Next.js (if the site will grow into an app), Eleventy (simpler, no component islands), Hugo (Go, very fast builds, template-based).
- **Docs:** https://docs.astro.build
- **Notes for chef:** Strong default for "a website, not an app". Pair with a headless CMS (see `services.md`) when non-developers need to edit content.

### React Router (v7, framework mode)
- **Type:** framework
- **Category:** Full-stack React (web)
- **What it is:** React Router v7 absorbed the Remix v2 framework features. Used as a library it is just routing; in "framework mode" it is a full-stack framework with loaders, actions, and SSR.
- **Problem it solves:** A full-stack React app with less framework opinion and less Vercel-specific behavior than Next.js.
- **When to use:** Teams that want Remix-style data loading, or a full-stack React app deployable anywhere (Node, Cloudflare Workers, etc.).
- **When NOT to use:** When you want the largest ecosystem and template supply (that is Next.js); when you only need client-side routing in an existing SPA (use it as a plain library instead).
- **Works with:** React, Vite, any Node/edge host, Tailwind.
- **Cost / licensing:** Open-source (MIT).
- **Maintenance / status:** Stable, actively maintained (React Router team / Shopify).
- **Alternatives:** Next.js (more ecosystem), TanStack Start (newer, type-heavy).
- **Docs:** https://reactrouter.com
- **Notes for chef:** Note the naming history to the user: "Remix" as a React framework effectively continued here. Remix v3 is a separate, non-React project still in beta.

### SvelteKit
- **Type:** framework
- **Category:** Full-stack (web), non-React
- **What it is:** The official application framework for Svelte, a compiler-based UI library with very little runtime.
- **Problem it solves:** Fast, small apps and sites with a concise authoring model and no virtual DOM overhead.
- **When to use:** Teams that prefer Svelte's ergonomics; performance-sensitive UIs; smaller projects where React's ecosystem is not needed.
- **When NOT to use:** When the project depends on React-only libraries or React hiring availability.
- **Works with:** Vite, adapters for Vercel/Netlify/Cloudflare/Node, Tailwind.
- **Cost / licensing:** Open-source (MIT).
- **Maintenance / status:** Stable, actively maintained.
- **Alternatives:** Nuxt (Vue), SolidStart (Solid), Next.js/React Router (React).
- **Docs:** https://svelte.dev/docs/kit
- **Notes for chef:** Pick only if the user or team actively wants Svelte. Otherwise default to the React options for ecosystem reach.

### Nuxt
- **Type:** framework
- **Category:** Full-stack Vue (web)
- **What it is:** The full-stack framework for Vue, comparable in scope to Next.js.
- **Problem it solves:** Server-rendered Vue apps with routing, data fetching, and deployment presets handled.
- **When to use:** Teams standardized on Vue.
- **When NOT to use:** No existing Vue preference — the React ecosystem is larger.
- **Works with:** Vue, Vite, Nitro server, most hosts, Tailwind.
- **Cost / licensing:** Open-source (MIT).
- **Maintenance / status:** Stable, actively maintained.
- **Alternatives:** SvelteKit, Next.js.
- **Docs:** https://nuxt.com/docs
- **Notes for chef:** Vue-team choice only. No reason to move a non-Vue team here.

### Expo (React Native)
- **Type:** framework
- **Category:** Mobile (iOS + Android), also web
- **What it is:** The standard toolchain for React Native. Expo Router gives file-based navigation; EAS builds and submits the apps.
- **Problem it solves:** Building and shipping a real iOS/Android app from one React codebase without managing native toolchains by hand.
- **When to use:** Cross-platform mobile apps; teams with React skills who need to be in the app stores.
- **When NOT to use:** A single-platform app that needs deep native/platform-specific work (consider native Swift/Kotlin); when a responsive web app or PWA is actually enough.
- **Works with:** React Native, EAS (build/submit/update), Nativewind (Tailwind syntax), most JS backends.
- **Cost / licensing:** Open-source SDK (MIT). EAS build/submit/update has a free tier then usage-based paid plans.
- **Maintenance / status:** Stable, actively maintained; the recommended way to start React Native.
- **Alternatives:** Bare React Native, Flutter (Dart, non-JS), native iOS/Android, or a PWA if stores are not required.
- **Docs:** https://docs.expo.dev
- **Notes for chef:** Ask early whether the user truly needs a store-distributed app or whether a responsive web app covers the need — it often does and is far cheaper to build.

### Hono
- **Type:** framework
- **Category:** Backend / API (JavaScript, edge-friendly)
- **What it is:** A small, fast web framework for building APIs that runs on Node, Bun, Deno, Cloudflare Workers, and more.
- **Problem it solves:** A standalone HTTP API or backend-for-frontend without pulling in a full app framework.
- **When to use:** Separate API services, edge functions, lightweight backends, webhooks.
- **When NOT to use:** When the frontend framework's own API routes are sufficient; large services that would benefit from NestJS's structure.
- **Works with:** Any JS runtime, Drizzle, Zod (via middleware), Cloudflare Workers.
- **Cost / licensing:** Open-source (MIT).
- **Maintenance / status:** Stable, actively maintained, rising adoption.
- **Alternatives:** Express (ubiquitous, older), Fastify (fast, plugin-rich, Node-focused), NestJS (structured/enterprise), Elysia (Bun-focused).
- **Docs:** https://hono.dev/docs
- **Notes for chef:** Good default for a new standalone JS API, especially on the edge. Use Express only when matching an existing codebase or tutorial base.

### FastAPI
- **Type:** framework
- **Category:** Backend / API (Python)
- **What it is:** A modern Python framework for building APIs, with type hints driving validation and automatic OpenAPI docs.
- **Problem it solves:** A well-documented, typed HTTP API in Python, commonly in front of ML/data code.
- **When to use:** Python teams; APIs that sit next to data science, ML, or scientific libraries.
- **When NOT to use:** JS/TS teams with no Python reason; heavy server-rendered sites (use Django).
- **Works with:** Pydantic, SQLAlchemy/SQLModel, Uvicorn, any host that runs Python.
- **Cost / licensing:** Open-source (MIT).
- **Maintenance / status:** Stable, actively maintained.
- **Alternatives:** Django REST Framework (batteries included, ORM + admin), Flask (minimal), Litestar.
- **Docs:** https://fastapi.tiangolo.com
- **Notes for chef:** Default Python API choice. Choose Django instead when the project also wants an ORM, admin UI, and server-rendered pages in one package.
