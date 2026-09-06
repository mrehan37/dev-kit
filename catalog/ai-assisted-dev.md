# AI-Assisted Development

Resources that specifically help the chef (any AI coding assistant) plan, build,
research, and review software: MCP servers, AI skills, and supporting tools.

Two rules for this section:

- **Do not install everything.** Each entry says what a resource does and *when
  the chef should consider connecting or using it* — not "always on".
- Prefer **official** MCP servers and well-maintained skills. Treat community
  entries as useful but unvetted; check the source before relying on them.

---

## MCP servers

An MCP server gives the chef a direct, structured connection to a tool or data
source. Connect the few that match the project; disconnect the rest.

### Context7
- **Type:** MCP
- **Category:** Documentation retrieval
- **What it is:** An MCP server that fetches current, version-specific documentation and code examples for libraries and frameworks on demand.
- **Problem it solves:** Model knowledge of fast-moving libraries goes stale; Context7 pulls the real current docs into context instead of guessing.
- **When to use:** Any time the chef is writing code against a specific library/framework/CLI, migrating versions, or unsure of current API syntax.
- **When NOT to use:** General programming concepts, business-logic debugging, or writing scripts from scratch — it adds noise there.
- **Works with:** Claude Code, Cursor, and other MCP clients; complements web search.
- **Cost / licensing:** Free tier; hosted service (higher limits paid).
- **Maintenance / status:** Actively maintained, widely used.
- **Alternatives:** A framework's own docs MCP where one exists; plain web search/fetch (slower, less targeted).
- **Docs:** https://github.com/upstash/context7
- **Notes for chef:** Prefer this over web search for library API questions. Resolve the library id first, then query a single concept at a time.

### GitHub MCP
- **Type:** MCP
- **Category:** Source control / project management
- **What it is:** The official GitHub MCP server: repository search and file access, issues, pull requests, branches, Actions/workflow status, and code review operations.
- **Problem it solves:** Letting the chef work with issues, PRs, and CI directly instead of pasting output back and forth.
- **When to use:** Projects hosted on GitHub where the chef needs to open/read issues and PRs, check CI, or navigate a large repo.
- **When NOT to use:** Local-only projects; when the `gh` CLI already covers the need with less surface area; when repo write access should stay manual.
- **Works with:** GitHub.com and GitHub Enterprise; official remote (OAuth) endpoint available.
- **Cost / licensing:** Free (uses your GitHub account/permissions).
- **Maintenance / status:** Official, actively maintained.
- **Alternatives:** `gh` CLI (scriptable, no MCP), GitLab's MCP/CLI for GitLab-hosted projects.
- **Docs:** https://github.com/github/github-mcp-server
- **Notes for chef:** Scope the token to the repos in play. Prefer read operations; keep merges and releases human-approved.

### Playwright MCP
- **Type:** MCP
- **Category:** Browser automation / verification
- **What it is:** Microsoft's official MCP server that drives a real browser (Chromium/Firefox/WebKit) via structured tools, using accessibility-tree snapshots rather than screenshots.
- **Problem it solves:** Letting the chef actually load the app, click through a flow, and confirm a change works — not just assume it does.
- **When to use:** Verifying UI changes end-to-end, reproducing a browser bug, scripted smoke checks, scraping a page's structure during research.
- **When NOT to use:** Pure backend/CLI work; when the project's own Playwright test suite already covers the flow; performance profiling (use Chrome DevTools MCP).
- **Works with:** Any web app; pairs with the Playwright test framework in `quality.md`.
- **Cost / licensing:** Free, open-source.
- **Maintenance / status:** Official (Microsoft), actively maintained.
- **Alternatives:** Chrome DevTools MCP (debugging/performance, Chromium only), Puppeteer-based servers (narrower).
- **Docs:** https://github.com/microsoft/playwright-mcp
- **Notes for chef:** Default browser-driving server. Use it to check real behavior before reporting a UI task done.

### Chrome DevTools MCP
- **Type:** MCP
- **Category:** Web performance & debugging
- **What it is:** An MCP server from the Chrome DevTools team exposing DevTools Protocol capabilities: performance traces, network inspection, console, and DOM debugging.
- **Problem it solves:** Diagnosing *why* a page is slow or a request is failing, with real profiling data instead of guesswork.
- **When to use:** Investigating Core Web Vitals regressions, network/waterfall problems, memory issues, hard-to-reproduce runtime errors.
- **When NOT to use:** Cross-browser E2E testing (use Playwright MCP); routine click-through verification.
- **Works with:** Chromium only.
- **Cost / licensing:** Free, open-source (Apache-2.0).
- **Maintenance / status:** Official (Chrome DevTools), actively maintained.
- **Alternatives:** Playwright MCP (automation, not deep profiling), Lighthouse CI (see `quality.md`).
- **Docs:** https://github.com/ChromeDevTools/chrome-devtools-mcp
- **Notes for chef:** Add only when there is a concrete performance or debugging question. Not an always-on server.

### Filesystem MCP
- **Type:** MCP
- **Category:** Local file access (sandboxed)
- **What it is:** A reference MCP server that exposes read/write access to explicitly allowed directories.
- **Problem it solves:** Giving an MCP client structured, permissioned file access to specific folders outside the current project root.
- **When to use:** Assistants without their own file tools; working across a set of sibling repos or a shared assets directory.
- **When NOT to use:** Inside Claude Code / Codex, which already have native, safer file tooling — redundant there.
- **Works with:** Any MCP client; you pass the allowed paths.
- **Cost / licensing:** Free, open-source (reference server).
- **Maintenance / status:** Maintained as part of the MCP reference servers.
- **Alternatives:** The assistant's native file tools; a Git MCP for repo-scoped history operations.
- **Docs:** https://github.com/modelcontextprotocol/servers
- **Notes for chef:** Skip in environments that already have file tools. Where used, allow the narrowest set of directories.

### Database MCP (Postgres / SQLite, and vendor servers)
- **Type:** MCP
- **Category:** Database inspection / querying
- **What it is:** MCP servers that let the chef inspect schemas and run queries against a database. Includes the reference Postgres/SQLite servers and official vendor servers (e.g. Supabase's) with project-aware tooling.
- **Problem it solves:** Understanding an unfamiliar schema, checking data shape, and drafting/validating queries without a round-trip through the user.
- **When to use:** Working against an existing database whose schema needs exploring; validating migrations; debugging data issues.
- **When NOT to use:** Production databases without a read-only role and clear guardrails; greenfield projects where the schema lives in the repo already (read the migrations).
- **Works with:** Postgres, SQLite/libSQL, and vendor platforms; use a read-only connection where possible.
- **Cost / licensing:** Free/open-source (reference and most vendor servers).
- **Maintenance / status:** Reference servers maintained; vendor servers vary — prefer official ones.
- **Alternatives:** ORM schema files + migrations in the repo; a SQL client the user drives.
- **Docs:** https://github.com/modelcontextprotocol/servers
- **Notes for chef:** Connect with least privilege — read-only against anything that matters. Never point a write-capable DB MCP at production without explicit approval.

### Fetch MCP
- **Type:** MCP
- **Category:** Web content retrieval
- **What it is:** A reference MCP server that fetches a URL and converts it to clean markdown for the model.
- **Problem it solves:** Pulling a specific documentation page, changelog, or article into context without a browser.
- **When to use:** Reading a known URL during research; checking a release note or a spec page.
- **When NOT to use:** Authenticated pages; when the assistant already has a web-fetch tool; broad open-ended research (use search first).
- **Works with:** Any MCP client.
- **Cost / licensing:** Free, open-source (reference server).
- **Maintenance / status:** Maintained as part of the MCP reference servers.
- **Alternatives:** The assistant's native fetch/search; Context7 for library docs specifically.
- **Docs:** https://github.com/modelcontextprotocol/servers
- **Notes for chef:** Redundant where a native fetch tool exists. Useful to give a bare MCP client basic research reach.

### Vendor MCP servers (Stripe, Sentry, Vercel, Figma, Linear, Notion, …)
- **Type:** MCP
- **Category:** Service-specific tooling
- **What it is:** Official MCP servers published by individual platforms, exposing their API as structured tools (e.g. Stripe: create test objects and search docs; Sentry: pull issue details; Figma Dev Mode: read selected layer structure and tokens).
- **Problem it solves:** Working with a specific service's data/config during development without hand-writing API calls.
- **When to use:** Only when the project actively integrates that service and the task needs its data (e.g. building Stripe billing, triaging Sentry issues, translating a Figma frame to code).
- **When NOT to use:** Services the project does not use; tasks that do not touch that service. Do not connect a shelf of vendor servers "just in case".
- **Works with:** Each vendor's account/permissions; many offer hosted OAuth endpoints.
- **Cost / licensing:** Generally free (billed via normal service usage).
- **Maintenance / status:** Varies by vendor; prefer servers the vendor officially publishes and maintains.
- **Notes for chef:** Connect per project, scoped to the integration in hand, and disconnect when done. Keep credentials least-privilege.

## AI skills

Skills package instructions or domain knowledge that steer an AI assistant.
Format and install mechanism differ by assistant (Claude Code plugins/skills,
Cursor rules, etc.); the value is portable even when the packaging is not.

### UI/UX Pro Max Skill
- **Type:** AI skill
- **Category:** UI/UX design intelligence
- **What it is:** An open-source skill (`nextlevelbuilder/ui-ux-pro-max-skill`) that gives an assistant a structured design knowledge base — many UI styles, color palettes, font pairings, UX guidelines, chart types, and per-stack rules — plus a recommendation flow for generating a coherent design system by product type.
- **Problem it solves:** AI-generated UIs that are generic or inconsistent; it pushes toward considered typography, palette, spacing, and layout decisions instead of default-bootstrap looks.
- **When to use:** Greenfield user-facing projects where visual quality matters and there is no existing design system to follow; when the user wants "make it look professional" and cannot supply specifics.
- **When NOT to use:** Projects with an established brand/design system (follow that instead); backends, CLIs, and internal tools where visual polish is not the point; when a designer is already providing specs/Figma.
- **Works with:** Claude Code (plugin/marketplace install), and other assistants that can load skill files; output targets web (HTML+Tailwind, React, Vue), mobile, and cross-platform.
- **Cost / licensing:** Free, open-source; community-maintained (not from Anthropic or a framework team).
- **Maintenance / status:** Actively maintained and popular; treat as a useful opinionated aid, not an authority — verify its choices against the project's real constraints and `../standards/quality-baseline.md`.
- **Alternatives:** An assistant's built-in design/"frontend" skill where available; a real design system or component kit (shadcn/ui + a chosen theme); a human designer.
- **Docs:** https://github.com/nextlevelbuilder/ui-ux-pro-max-skill
- **Notes for chef:** Objective verdict — worth keeping in the catalog as a design aid for greenfield UI work. Use it to propose a design direction for the user to approve; do not let it override an existing brand or add visual complexity the project does not need.

### Code review skills
- **Type:** AI skill
- **Category:** Code quality / review
- **What it is:** Skills that run a structured review pass over a diff — checking correctness, security, edge cases, and simplification opportunities — and report findings by severity. Claude Code ships one (`/code-review`, `/security-review`); comparable community skills exist.
- **Problem it solves:** Catching bugs and risky patterns before a human review, with consistent coverage.
- **When to use:** Before opening a PR; after a large change; as a pre-merge gate on important paths.
- **When NOT to use:** As a replacement for human review on security-critical changes; trivial one-line edits.
- **Works with:** Git diffs, PR branches; the assistant's own tooling.
- **Cost / licensing:** Free where bundled with the assistant; community skills vary.
- **Maintenance / status:** The first-party ones are maintained with the assistant.
- **Alternatives:** Traditional linters/scanners (see `quality.md`), human review, CI-based review bots.
- **Notes for chef:** Prefer the assistant's built-in review command when it has one. Treat findings as input to judgment, not automatic fixes.

### Repository / workflow skills
- **Type:** AI skill
- **Category:** Developer workflow
- **What it is:** Skills that standardize common chores — conventional commit messages, PR descriptions, changelog generation from history, test-driven implementation loops, spec-writing before coding.
- **Problem it solves:** Inconsistent commit/PR hygiene and unstructured "just start coding" approaches.
- **When to use:** Team repos with commit/PR conventions; larger features that benefit from a written spec first.
- **When NOT to use:** Solo throwaway work; when the repo has its own conventions the skill would fight.
- **Works with:** Git, the hosting platform, the assistant's task tooling.
- **Cost / licensing:** Varies; many are free/open.
- **Maintenance / status:** Varies by author — check before adopting.
- **Notes for chef:** Adopt these to match a project's existing conventions, not to impose new ones. Confirm the repo's commit/PR style first.

## Supporting research tools

### Web search + fetch (built-in)
- **Type:** tooling
- **Category:** Current-information research
- **What it is:** The assistant's own web search and page-fetch capabilities.
- **Problem it solves:** Verifying current versions, pricing, release status, and comparisons that model training data may not reflect.
- **When to use:** Confirming anything time-sensitive before putting it in a recipe or the catalog; filling catalog gaps.
- **When NOT to use:** Library API specifics where Context7 is more precise; questions answerable from the repo itself.
- **Works with:** Everything; pair search (find sources) with fetch (read the official one).
- **Cost / licensing:** Included with the assistant.
- **Notes for chef:** Prefer official sites and repos over listicles. Cross-check any surprising claim (especially version numbers and pricing) against the primary source before recording it.
