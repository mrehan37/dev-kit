# Quality

Testing, validation, type checking, linting/formatting, security, SEO,
accessibility, and performance. These support the cascade in
`../standards/quality-baseline.md`: they are defaults applied where relevant, not
mandatory for every project.

---

## Testing

### Vitest
- **Type:** library
- **Category:** Unit / integration test runner (JS/TS)
- **What it is:** A fast test runner with a Jest-compatible API, native ESM and TypeScript support, and tight Vite integration.
- **Problem it solves:** Running unit and integration tests quickly, with modern module support and little config.
- **When to use:** The default test runner for new Vite-based and most TS/JS projects.
- **When NOT to use:** Codebases already standardized on Jest with no reason to migrate; non-JS stacks.
- **Works with:** Vite, React/Vue/Svelte, Testing Library, jsdom/happy-dom, CI.
- **Cost / licensing:** Free, open-source (MIT).
- **Maintenance / status:** Stable, actively maintained.
- **Alternatives:** Jest (huge ecosystem, established), Node's built-in `node:test` (no dependency), Bun test (if on Bun).
- **Docs:** https://vitest.dev
- **Notes for chef:** Default for unit/integration tests. Pair with Testing Library for component tests.

### Playwright
- **Type:** library
- **Category:** End-to-end / browser testing
- **What it is:** A cross-browser automation and E2E testing framework (Chromium, Firefox, WebKit) with auto-waiting, tracing, parallelism, and a test runner.
- **Problem it solves:** Verifying real user flows through the actual UI across browsers, reliably enough to run in CI.
- **When to use:** Critical-path E2E tests (signup, checkout, core workflows); visual and accessibility snapshot checks; cross-browser verification.
- **When NOT to use:** As a substitute for unit tests (slow, broader); tiny projects where a few manual checks suffice.
- **Works with:** Any web app regardless of framework; CI; also powers Playwright MCP (see `ai-assisted-dev.md`).
- **Cost / licensing:** Free, open-source (Apache-2.0).
- **Maintenance / status:** Stable, actively maintained (Microsoft).
- **Alternatives:** Cypress (mature DX, component testing, weaker multi-browser story), WebdriverIO, Puppeteer (Chromium automation, not a full test framework).
- **Docs:** https://playwright.dev
- **Notes for chef:** Default E2E choice. Keep the E2E suite small and focused on money paths; push detail down to unit tests.

### Testing Library
- **Type:** library
- **Category:** Component testing utilities
- **What it is:** A family of helpers (`@testing-library/react`, etc.) for testing components the way a user interacts with them — by role, label, and text rather than implementation details.
- **Problem it solves:** Component tests that survive refactors because they assert on behavior and accessible output, not internal structure.
- **When to use:** Testing React/Vue/Svelte components alongside Vitest or Jest.
- **When NOT to use:** Pure logic modules (test them directly); full end-to-end flows (use Playwright).
- **Works with:** Vitest/Jest, jsdom/happy-dom, every major UI framework.
- **Cost / licensing:** Free, open-source (MIT).
- **Maintenance / status:** Stable, actively maintained.
- **Alternatives:** Enzyme (legacy, avoid for new work), framework-specific test utils.
- **Docs:** https://testing-library.com/docs
- **Notes for chef:** Query by accessible role/name — doing so doubles as a lightweight accessibility check.

## Validation & types

### Zod
- **Type:** library
- **Category:** Schema validation (TypeScript)
- **What it is:** A TypeScript-first schema library: declare a schema once, get runtime validation and a static type inferred from it.
- **Problem it solves:** Trusting external data (form input, API responses, env vars, webhooks) and keeping runtime checks in sync with types.
- **When to use:** Validating anything crossing a boundary — forms, API/route inputs, `process.env`, third-party payloads, LLM output.
- **When NOT to use:** Hot paths where validation overhead matters and a smaller validator fits (consider Valibot); non-TS projects.
- **Works with:** React Hook Form, tRPC, Drizzle (`drizzle-zod`), most frameworks; OpenAPI generators.
- **Cost / licensing:** Free, open-source (MIT).
- **Maintenance / status:** Stable, actively maintained, de facto standard.
- **Alternatives:** Valibot (modular, tiny bundle), ArkType (fast, type-syntax schemas), Yup (older), TypeBox (JSON Schema output).
- **Docs:** https://zod.dev
- **Notes for chef:** Default validator. Define each schema once and reuse it on both client and server. Validate environment variables at startup.

### TypeScript
- **Type:** language / tooling
- **Category:** Static typing for JavaScript
- **What it is:** A typed superset of JavaScript that compiles to plain JS and catches a large class of errors before runtime.
- **Problem it solves:** Refactoring safety, editor autocomplete, and self-documenting interfaces on any non-trivial JS codebase.
- **When to use:** Essentially every JS/Node project beyond a throwaway script.
- **When NOT to use:** One-file scripts; teams with a hard constraint against a build step (rare now — types can be stripped).
- **Works with:** Every framework and runtime here; `strict` mode recommended.
- **Cost / licensing:** Free, open-source (Apache-2.0).
- **Maintenance / status:** Stable, actively maintained (Microsoft).
- **Alternatives:** JSDoc type annotations (types without syntax change), plain JS (not recommended for shared codebases).
- **Docs:** https://www.typescriptlang.org/docs
- **Notes for chef:** Default to TypeScript with `strict: true`. Treat type errors as build failures in CI.

## Linting & formatting

### Biome
- **Type:** library / CLI
- **Category:** Linter + formatter (unified, Rust)
- **What it is:** A single fast Rust binary that formats and lints JS/TS/JSX/JSON (and more), replacing the ESLint + Prettier pair with no plugin wiring.
- **Problem it solves:** Slow lint/format runs and the maintenance cost of keeping two tools and their plugins in sync.
- **When to use:** New projects; teams that want speed and one config file; CI where lint/format time matters.
- **When NOT to use:** Projects that depend on specific ESLint plugins Biome does not yet cover (some framework/a11y/security rulesets); large existing ESLint setups where migration cost outweighs the gain.
- **Works with:** Any JS/TS project; editor extensions; CI.
- **Cost / licensing:** Free, open-source (MIT/Apache).
- **Maintenance / status:** Stable, actively maintained, growing adoption.
- **Alternatives:** ESLint + Prettier (largest rule/plugin ecosystem, still the safest default for React/Next-heavy or security-sensitive code), Oxlint (very fast linter only), dprint (formatter only).
- **Docs:** https://biomejs.dev
- **Notes for chef:** Reasonable default for greenfield. If the project leans on ESLint plugins for framework or accessibility rules, keep ESLint (optionally Biome for formatting only). Do not run both formatters.

## Security

### OWASP resources (ASVS, Cheat Sheets, Top 10)
- **Type:** reference / standard
- **Category:** Application security guidance
- **What it is:** Vendor-neutral security standards and practical guides: the Top 10 risk list, the Application Security Verification Standard (ASVS), and topic Cheat Sheets (auth, input validation, headers, secrets, etc.).
- **Problem it solves:** Knowing what "secure enough" means for a web app without a security background — a checklist grounded in real risk.
- **When to use:** Any app handling accounts, payments, personal data, or file uploads; security review checklists; threat-modeling a feature.
- **When NOT to use:** As a substitute for a real audit on high-risk systems; do not treat the Top 10 as exhaustive.
- **Works with:** Any stack; feeds directly into a project's `standards/` entry.
- **Cost / licensing:** Free, open (Creative Commons).
- **Maintenance / status:** Actively maintained by the OWASP Foundation.
- **Alternatives:** CWE/SANS lists, cloud provider well-architected security pillars, platform-specific hardening guides.
- **Docs:** https://cheatsheetseries.owasp.org · https://owasp.org/www-project-application-security-verification-standard
- **Notes for chef:** Use the Cheat Sheets when implementing auth, sessions, uploads, or headers. Pull the relevant items into the project's quality standards rather than pasting the whole list.

### Dependency & secret scanning (npm audit / OSV / Dependabot / gitleaks)
- **Type:** tooling
- **Category:** Supply-chain and secret hygiene
- **What it is:** Tools that flag known-vulnerable dependencies (`npm audit`, `pnpm audit`, OSV-Scanner, GitHub Dependabot/`dependabot.yml`) and detect committed secrets (`gitleaks`, GitHub secret scanning, `trufflehog`).
- **Problem it solves:** Shipping known CVEs via transitive dependencies, and leaking API keys/tokens into git history.
- **When to use:** Every project with dependencies and a git remote; run in CI on pull requests.
- **When NOT to use:** No exceptions worth noting — the cost is near zero.
- **Works with:** Any package ecosystem; GitHub/GitLab native features; pre-commit hooks.
- **Cost / licensing:** Free/open-source; native platform features free on public repos and most plans.
- **Maintenance / status:** Stable, actively maintained.
- **Alternatives:** Snyk (commercial, deeper remediation), Socket (supply-chain attack detection), Renovate (dependency updates, more configurable than Dependabot).
- **Notes for chef:** Enable Dependabot/Renovate + secret scanning at project creation. Add a pre-commit secret scan. Keep secrets in env vars / a secrets manager, never in the repo (see below).

## Config & secrets

### Environment variable handling (dotenv + schema validation)
- **Type:** standard / pattern
- **Category:** Configuration management
- **What it is:** Loading configuration from the environment (`.env` files locally, real env vars in deploy) and validating it against a schema at startup so misconfiguration fails fast and loud.
- **Problem it solves:** Runtime crashes deep in the app from a missing or malformed config value; secrets accidentally hard-coded.
- **When to use:** Every app with any configuration or credentials.
- **When NOT to use:** N/A — this is baseline.
- **Works with:** Zod/Valibot for the schema; `@t3-oss/env` for typed env in TS apps; framework-native env loading; platform secret stores (Vercel/Cloudflare/Doppler/1Password/AWS Secrets Manager).
- **Cost / licensing:** Free (patterns and open-source libraries); hosted secret managers are usage/seat-priced.
- **Maintenance / status:** Stable practice.
- **Notes for chef:** Commit a `.env.example` with keys and dummy values, never real `.env`. Validate `process.env` through a schema in one module and import typed config from there. Use the host's secret store for production.

## SEO, accessibility, performance

### Lighthouse / axe / Pa11y
- **Type:** tooling / reference
- **Category:** SEO, accessibility, and performance auditing
- **What it is:** Lighthouse audits performance, SEO, best practices, and basic accessibility (built into Chrome DevTools and available as CI). axe-core is the accessibility engine behind most a11y checkers (`@axe-core/playwright`, browser extensions). Pa11y runs accessibility checks from the command line/CI.
- **Problem it solves:** Catching missing meta tags, poor Core Web Vitals, unlabeled controls, and contrast failures before users (and search engines) do.
- **When to use:** Any user-facing site or app; wire Lighthouse CI and an axe pass into the pipeline for public sites.
- **When NOT to use:** Headless APIs and internal CLI tools (no UI to audit); automated checks never fully replace manual keyboard/screen-reader testing.
- **Works with:** Chrome, Playwright, CI; frameworks' own metadata/SEO helpers on top.
- **Cost / licensing:** Free, open-source.
- **Maintenance / status:** Stable, actively maintained.
- **Alternatives:** WebPageTest (deep performance analysis), PageSpeed Insights (field + lab data), Unlighthouse (site-wide Lighthouse crawl), WAVE.
- **Docs:** https://developer.chrome.com/docs/lighthouse · https://www.deque.com/axe
- **Notes for chef:** For public sites, treat SEO metadata (title, description, Open Graph), a sitemap, `robots.txt`, semantic headings, and a passing axe run as baseline (see `../standards/quality-baseline.md`). Automated a11y catches maybe half of issues — still do a manual keyboard pass on key flows.

### Structured data (Schema.org / JSON-LD)
- **Type:** reference / standard
- **Category:** SEO — machine-readable content
- **What it is:** A vocabulary (Schema.org) embedded as JSON-LD in pages so search engines and other tools understand entities: articles, products, events, breadcrumbs, organizations, FAQs.
- **Problem it solves:** Eligibility for rich results in search and clearer machine understanding of page content.
- **When to use:** Public content sites and e-commerce — articles, product pages, events, local business info, recipes, FAQs.
- **When NOT to use:** Authenticated app screens; internal tools; pages with no public search value.
- **Works with:** Any framework (inject a `<script type="application/ld+json">`); validate with Google's Rich Results Test / Schema.org validator.
- **Cost / licensing:** Free, open standard.
- **Maintenance / status:** Stable, maintained by Schema.org and consuming search engines.
- **Docs:** https://schema.org · https://developers.google.com/search/docs/appearance/structured-data
- **Notes for chef:** Add structured data for public content types only, and validate it. Do not add markup that does not match visible page content.
