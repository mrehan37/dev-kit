# Services

Third-party hosted products: payments, email, analytics, maps, CMS, hosting, and
error monitoring. These carry accounts, API keys, and usually a bill — surface
the cost model to the user before selecting one.

---

## Payments

### Stripe
- **Type:** service + API
- **Category:** Payments (payment processor)
- **What it is:** The default developer payments platform: one-off charges, subscriptions, invoicing, a hosted Checkout, a customer billing portal, and a large API.
- **Problem it solves:** Taking money online — cards, wallets, subscriptions, trials, proration — without building against banks directly.
- **When to use:** Almost any product that charges customers, especially SaaS subscriptions and marketplaces (Stripe Connect).
- **When NOT to use:** When you want a Merchant of Record to handle sales tax/VAT globally for you (see Paddle / Lemon Squeezy); regions Stripe does not serve well.
- **Works with:** Every backend; official SDKs; prebuilt Checkout and Billing Portal reduce PCI scope; webhooks for provisioning.
- **Cost / licensing:** Usage-based per-transaction fee (roughly 2.9% + 30¢ in the US; varies by country/method). No monthly minimum.
- **Maintenance / status:** Stable, industry standard, actively maintained.
- **Alternatives:** Paddle / Lemon Squeezy (Merchant of Record — they handle tax remittance), Polar (MoR for devs), PayPal/Braintree, Adyen (enterprise), regional processors.
- **Docs:** https://docs.stripe.com
- **Notes for chef:** Default. Use hosted Checkout + Billing Portal unless the product truly needs a custom payment UI. If the user is a solo/small seller worried about global sales tax, raise Merchant-of-Record options instead.

### Paddle / Lemon Squeezy
- **Type:** service
- **Category:** Payments (Merchant of Record)
- **What it is:** Payment platforms that act as the seller of record: they collect payment and remit sales tax/VAT worldwide on your behalf. Lemon Squeezy is owned by Stripe but still operates independently; Paddle is independent.
- **Problem it solves:** Selling digital products/SaaS globally without registering for and filing tax in dozens of jurisdictions.
- **When to use:** Indie developers and small teams selling digital goods/SaaS internationally who do not want tax-compliance overhead.
- **When NOT to use:** Marketplaces paying out third parties (use Stripe Connect); businesses that already have tax infrastructure and want the lower raw Stripe fee; physical goods.
- **Works with:** Any stack via checkout links, overlays, and webhooks; SDKs available.
- **Cost / licensing:** Higher blended fee than raw Stripe (around 5% + 50¢) because tax handling is included.
- **Maintenance / status:** Both stable and actively maintained. Watch the Stripe "Managed Payments" direction, which overlaps this space.
- **Alternatives:** Stripe + a tax service (Stripe Tax, Anrok) if you keep MoR responsibility; Polar; Gumroad (simplest, least flexible).
- **Docs:** https://developer.paddle.com · https://docs.lemonsqueezy.com
- **Notes for chef:** Frame the trade to the user: pay a higher fee to make global tax someone else's problem, or take the lower Stripe fee and own compliance.

## Email

### Resend
- **Type:** service + API
- **Category:** Transactional email
- **What it is:** A developer-focused transactional email API with a clean SDK and React Email for building templates as components.
- **Problem it solves:** Reliably delivering signup confirmations, password resets, receipts, and notifications from an app.
- **When to use:** New projects wanting fast setup and component-based templates; typical transactional volumes.
- **When NOT to use:** Large-scale marketing/newsletter sending with advanced segmentation (use an ESP built for that); when an incumbent (SES) is already wired up and cost is the priority.
- **Works with:** Any backend; React Email for templates; domain setup via DNS (SPF/DKIM/DMARC).
- **Cost / licensing:** Free tier (limited monthly volume), then usage-based paid tiers.
- **Maintenance / status:** Stable, actively maintained.
- **Alternatives:** Postmark (excellent deliverability, transactional focus), Amazon SES (cheapest at scale, more setup), SendGrid/Mailgun (established, broader feature set), Loops/Customer.io (product + lifecycle messaging).
- **Docs:** https://resend.com/docs
- **Notes for chef:** Good default for transactional mail. Always set up SPF/DKIM/DMARC on the sending domain. Keep marketing email separate from transactional streams.

## Analytics

### PostHog
- **Type:** service
- **Category:** Product analytics (events, funnels, session replay, flags, experiments)
- **What it is:** An all-in-one product analytics platform: event capture, funnels, retention, session replay, feature flags, A/B tests, and surveys. Open-source, self-hostable, or cloud.
- **Problem it solves:** Understanding how users actually behave in a product, plus feature flagging and experimentation, without stitching several tools together.
- **When to use:** Products where you need behavioral analytics and/or feature flags; teams that want one tool for the whole loop.
- **When NOT to use:** Simple marketing sites that only need privacy-friendly traffic counts (use Plausible/Umami); when a lightweight footprint matters.
- **Works with:** Web and mobile SDKs, server-side capture, reverse proxy to avoid ad-blockers.
- **Cost / licensing:** Generous free tier (per-event), then usage-based; open-source edition self-hostable.
- **Maintenance / status:** Stable, very actively maintained.
- **Alternatives:** Plausible / Umami (simple, privacy-first pageview analytics; Umami self-hostable and MIT), Google Analytics 4 (free, powerful, privacy/consent overhead, complex UI), Mixpanel/Amplitude (deep product analytics), Vercel Analytics (basic, zero-config on Vercel).
- **Docs:** https://posthog.com/docs
- **Notes for chef:** For a marketing site, default to Plausible or Umami. For a product that needs funnels, replay, or flags, PostHog covers the most ground in one place. Respect consent requirements (see `quality.md`).

## Maps & location

### Mapbox / MapLibre
- **Type:** service + library
- **Category:** Maps, geocoding, routing
- **What it is:** Mapbox is a hosted maps/geocoding/navigation platform with heavily customizable vector styling. MapLibre GL is the open-source rendering library forked from Mapbox GL, usable with any vector tile source.
- **Problem it solves:** Interactive maps, address search (geocoding), directions, and custom cartography.
- **When to use:** Custom-styled interactive maps; store locators; delivery/tracking; anything needing geocoding or routing.
- **When NOT to use:** A single static map image or pin (an embed or static image API is enough); when the org is standardized on Google Maps data/places.
- **Works with:** Any frontend; React wrappers (react-map-gl); tile providers for MapLibre.
- **Cost / licensing:** Mapbox: free monthly quota then usage-based per load/request. MapLibre: open-source (BSD), but you still pay a tile host unless self-serving.
- **Maintenance / status:** Both stable and actively maintained.
- **Alternatives:** Google Maps Platform (best places/geocoding data, familiar UX, usage-based), Leaflet (simple raster maps, huge plugin ecosystem), OpenStreetMap tiles, Radar (location + geofencing).
- **Docs:** https://docs.mapbox.com · https://maplibre.org
- **Notes for chef:** Use Google Maps Platform when place/business data quality is the priority. Use Mapbox/MapLibre when custom styling and cost control matter. All the hosted options are usage-based — show the user the pricing calculator.

## Content management

### Sanity
- **Type:** service
- **Category:** Headless CMS
- **What it is:** A headless CMS with a customizable editing environment (Sanity Studio), a real-time content datastore, and a query API (GROQ). Content is structured and delivered to any frontend.
- **Problem it solves:** Letting non-developers edit site/app content without touching code, while developers keep full control of presentation.
- **When to use:** Marketing sites, blogs, docs, and apps where editors need a friendly UI and content is reused across channels; structured/localized content.
- **When NOT to use:** Content that rarely changes (Markdown in the repo is simpler); when a full traditional CMS with themes is what the client actually wants (consider WordPress).
- **Works with:** Astro, Next.js, and others; image CDN included; webhooks for rebuilds.
- **Cost / licensing:** Free tier (usage limits, seats), then paid plans by usage/seats.
- **Maintenance / status:** Stable, actively maintained.
- **Alternatives:** Contentful (enterprise, mature), Payload (open-source, self-hosted, Postgres/Mongo, code-first), Strapi (open-source, self-hosted, Node), Storyblok (visual editing), TinaCMS (Git-backed), WordPress headless (familiar to editors, huge plugin base), Markdown/MDX in-repo (no CMS).
- **Docs:** https://www.sanity.io/docs
- **Notes for chef:** If editors are non-technical and content changes often, a headless CMS earns its keep. If the "content" is a handful of pages maintained by developers, keep it as MDX in the repo and skip the CMS entirely. Payload is the strong pick when self-hosting and owning the data matter.

## Hosting & deployment

### Vercel
- **Type:** service
- **Category:** Frontend / full-stack hosting (PaaS)
- **What it is:** A deployment platform optimized for frontend frameworks (it maintains Next.js). Git-push deploys, preview URLs per PR, serverless/edge functions, CDN, and managed infra.
- **Problem it solves:** Shipping a modern web app with zero server management and automatic preview environments.
- **When to use:** Next.js apps (best-in-class), and most React/Astro/SvelteKit sites; teams that value preview deploys and DX over infra control.
- **When NOT to use:** Long-running processes, websockets at scale, background workers, or heavy backends (use a container host); cost-sensitive high-traffic sites (bandwidth/function pricing can climb).
- **Works with:** Next.js, Astro, SvelteKit, React Router, Vite SPAs; pairs with Neon/Supabase/Upstash.
- **Cost / licensing:** Free hobby tier (non-commercial), then per-seat Pro plus usage (bandwidth, function execution, image optimization).
- **Maintenance / status:** Stable, actively maintained.
- **Alternatives:** Netlify (similar model), Cloudflare Pages/Workers (cheap bandwidth, edge-first), Railway / Render / Fly.io (containers, long-running processes, databases), self-host on a VPS, AWS/GCP (full control, most work).
- **Docs:** https://vercel.com/docs
- **Notes for chef:** Default for framework-driven frontends. For apps with persistent connections, queues, or a real backend, put those on a container host (Railway/Render/Fly) and keep only the frontend on Vercel — or host the whole thing on the container platform.

### Cloudflare (Pages / Workers)
- **Type:** service
- **Category:** Edge hosting + platform
- **What it is:** Edge compute (Workers), static/SSR hosting (Pages), plus R2 storage, D1 (SQLite), KV, Queues, and Durable Objects on one network.
- **Problem it solves:** Globally low-latency apps with cheap bandwidth and an integrated set of storage/queue primitives at the edge.
- **When to use:** Latency-sensitive apps; cost-sensitive high-bandwidth sites; projects that fit the Workers runtime and want storage/queues from the same vendor.
- **When NOT to use:** Apps depending on full Node APIs or libraries incompatible with the Workers runtime; teams wanting the Next.js-on-Vercel happy path.
- **Works with:** Hono (excellent fit), Astro, SvelteKit, React Router, Next.js (via adapter — check current support).
- **Cost / licensing:** Generous free tiers; Workers paid plan is low-cost; R2 has no egress fees.
- **Maintenance / status:** Stable, actively maintained, rapidly expanding.
- **Alternatives:** Vercel/Netlify (framework DX), Deno Deploy (edge, Deno), Fly.io (containers at the edge).
- **Notes for chef:** Great when the app fits the runtime and bandwidth costs matter. Verify the chosen framework's Cloudflare adapter is currently first-class before committing.

## Error monitoring

### Sentry
- **Type:** service
- **Category:** Error tracking + performance + session replay
- **What it is:** Captures unhandled errors and performance data from frontend and backend, with stack traces, release tracking, source maps, and optional session replay.
- **Problem it solves:** Knowing what is breaking in production, for which users, since which release — instead of waiting for bug reports.
- **When to use:** Essentially any production application with real users.
- **When NOT to use:** Static sites with no application logic; very early throwaway prototypes.
- **Works with:** SDKs for every major framework and language; CI integration for source maps and releases.
- **Cost / licensing:** Free tier (limited monthly events), then usage-based paid plans; open-source, self-hostable (operationally heavy).
- **Maintenance / status:** Stable, actively maintained.
- **Alternatives:** Rollbar / Bugsnag (similar), Highlight.io (open-source, session replay focus), Axiom / Better Stack (logs + uptime), console logging + a log drain (minimal).
- **Docs:** https://docs.sentry.io
- **Notes for chef:** Treat error monitoring as part of the baseline for any app going to production (see `standards/quality-baseline.md`). Wire up releases and source maps or the stack traces are far less useful.
