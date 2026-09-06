# Catalog

The catalog is Devkit's curated inventory of reusable development resources: the
things the chef inspects when planning a project, before searching the wider
internet. It is a **map of what exists and when to reach for it**, not a set of
recipes and not a copy of any documentation.

## What lives here

Each file groups resources by area:

- `frameworks.md` — app and site frameworks (frontend, full-stack, mobile, backend)
- `frontend.md` — UI component libraries, styling, animation, forms, client state, icons
- `backend-data.md` — databases, ORMs, auth, caching, object storage, search, backend platforms
- `services.md` — payments, email, analytics, maps, CMS, hosting/deploy, error monitoring
- `quality.md` — testing, validation, type checking, linting/formatting, security, SEO, accessibility, performance
- `ai-assisted-dev.md` — MCP servers, AI skills, and resources that specifically help AI-assisted development

The sibling folders (`skills/`, `mcps/`, `services/`, `templates/`, `references/`,
`standards/`) hold **concrete assets** we author over time (a written skill, an
MCP setup note, a project template). A catalog entry may point to one of those
assets once it exists; until then the catalog entry stands alone.

## Resource types

Devkit does not treat everything as "a technology". Every entry carries a **Type**:

| Type | Meaning |
|---|---|
| `framework` | Defines the shape of an app or site (Next.js, Astro, Expo) |
| `library` / `package` | A dependency you add to a codebase (Zod, TanStack Query) |
| `service` | A hosted product with an account and usually a bill (Stripe, Resend) |
| `API` | A remote interface consumed over HTTP (as distinct from an SDK/library) |
| `MCP` | A Model Context Protocol server the chef can connect to directly |
| `AI skill` | Packaged instructions/knowledge that guide an AI assistant |
| `template` / `boilerplate` | A starting project structure |
| `reference` | Authoritative documentation or specification worth citing |
| `standard` | A best-practice rule set (lives in `standards/`, indexed here when useful) |

## Entry format

Keep entries compact and consistent. Human-readable explanation first, technical
detail after.

```
### <Name>
- **Type:** framework | library | package | service | API | MCP | AI skill | template | reference | standard
- **Category:** <short area label>
- **What it is:** <plain language, one or two sentences>
- **Problem it solves:** <the job it does>
- **When to use:** <concrete situations>
- **When NOT to use:** <concrete situations, alternatives implied>
- **Works with:** <frameworks, runtimes, dependencies that matter>
- **Cost / licensing:** <free / open-source / paid / usage-based; note free tier limits>
- **Maintenance / status:** <stable / actively maintained / niche / experimental / deprecated>
- **Alternatives:** <meaningful competitors, with the trade-off in a few words>
- **Docs:** <official URL>
- **Notes for chef:** <selection guidance, gotchas, defaults>
```

## How the chef uses the catalog

1. Translate the project's needs into capabilities (e.g. "needs recurring billing",
   "needs full-text search", "needs a marketing site with a blog").
2. Look for catalog entries that cover those capabilities.
3. Prefer entries marked stable and actively maintained. Treat `niche`,
   `experimental`, and `deprecated` entries as warnings, not equal options.
4. An entry existing here does not mean it belongs in the project. Select only
   what the requirements call for; do not combine everything discovered.
5. If the catalog has a gap, or an entry looks out of date, research current
   official sources and propose adding or updating an entry.

## Adding or updating an entry

- Check the resource is not already listed (including as an alternative on another entry).
- Confirm current status and cost from the **official** site or repo, not third-party listicles.
- Use the entry format above. Keep it to what helps a selection decision.
- Prefer folding a close competitor into an existing entry's **Alternatives** over
  creating a near-duplicate entry.
- Note explicitly when something should *not* be used, and flag anything
  experimental, niche, or poorly maintained.
- The catalog grows from real projects. Do not pad categories for completeness.

## Freshness

Versions and pricing move. Entries deliberately avoid pinning exact version
numbers; the chef should verify the current version and pricing at planning time
from the linked official docs.
