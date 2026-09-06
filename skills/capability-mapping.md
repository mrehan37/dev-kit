# Skill: Capability Mapping & Ingredient Selection

**Purpose:** turn a confirmed Understanding into a concrete set of chosen
ingredients, with reasoning and a list of gaps — ready to assemble into a recipe.

**Use when:** the user has confirmed the Understanding from
[`project-intake.md`](project-intake.md).

**Comes after:** project intake. **Feeds:** gap research (where gaps exist) and
recipe generation.

---

## Step 1 — Derive capabilities

Rewrite each scope item from the Understanding as a **capability statement** —
what the system must be able to do, not how:

- "customers book slots" → needs booking with availability rules
- "we email receipts" → needs transactional email
- "staff log in" → needs authentication with roles
- "shop wants a price list page" → needs a small public content site

Add capabilities that are implied but unstated:

- Any app with user data → sessions / auth, a database
- Any public page → SEO baseline, semantic HTML
- Anything taking user input → validation
- Anything going to production → error monitoring, error/loading/empty states

## Step 2 — Apply the quality-baseline cascade

Walk the four levels from [`../standards/quality-baseline.md`](../standards/quality-baseline.md),
later levels overriding earlier ones:

1. **Global defaults** — always on the table: version control, `.gitignore`,
   environment/secret handling, input validation, error handling, automated
   lint + type check, a project README.
2. **Project-type defaults** — name the archetype and pull its usual baseline:
   - *Marketing / content site*: SEO + Open Graph metadata, sitemap, `robots.txt`,
     semantic headings, structured data where content types warrant it, basic
     analytics, 404 handling.
   - *Web app*: auth, database, error monitoring, form validation, loading/empty/
     error states, responsive layout.
   - *API service*: input validation, auth/authorization, rate limiting where
     exposed, structured logging, OpenAPI/docs, health check.
   - *Mobile app*: offline/empty/error states, store metadata, deep links,
     update strategy.
   - *Internal tool*: auth, audit of sensitive actions; SEO not needed.
   - *CLI / library*: tests, typed API, semver, usage docs; no UI baseline.
3. **Project-specific** — the capabilities from step 1.
4. **Overrides** — the user's explicit constraints, must-use / must-avoid
   choices, and non-goals. These win over the defaults above.

## Step 3 — Match each capability to the catalog

For every capability, consult [`../catalog/`](../catalog/README.md):

- Prefer an entry marked **stable** and **actively maintained** that fits the
  stack and the user's constraints.
- Record when a capability needs **nothing new** (e.g. search satisfied by
  Postgres full-text; no global-state library needed).
- **Available is not necessary.** Select an ingredient only if the capability
  genuinely requires it. Do not add something because it is catalogued.
- When two entries genuinely fit, pick one **primary** and record the runner-up
  plus the deciding factor (cost, simplicity, team familiarity, constraint).
- Honour "When NOT to use" notes and compatibility ("Works with").

## Step 4 — Order the decisions

Decide in dependency order; each choice constrains the next:

1. Primary framework / project archetype
2. Hosting / deploy target (often implied by the framework)
3. Database + data-access layer
4. Authentication approach
5. Everything else (payments, email, storage, search, analytics, …)
6. Tooling baseline (lint/format, tests, CI)

Re-check compatibility at each step against the "Works with" fields.

## Step 5 — Identify gaps

Flag any capability where:

- No catalogued entry fits, or
- The only fit is marked niche / experimental / deprecated, or
- The catalogued entry looks stale (old links, superseded).

Send these to [`gap-research.md`](gap-research.md). Do not guess — research them.

## Step 6 — Produce the Selection

A table plus two short lists:

| Capability | Decision | Ingredient(s) | Why | Also considered | Source |
|---|---|---|---|---|---|
| … | use / build / none | … | one line | runner-up + reason | catalog / researched / none |

- **Deliberately not included** — things a reader might expect that the
  requirements do not call for (e.g. "no Redis — no caching/rate-limit need yet",
  "no CMS — content is developer-maintained MDX").
- **Cost roll-up** — every selected paid or usage-based ingredient with its cost
  model, so the recipe can show a rough monthly figure at the user's expected
  scale.

---

## Anti-patterns

- Bundling in everything discovered in the catalog
- Choosing a bleeding-edge option over a stable one with no stated reason
- Adding infrastructure (cache, queue, search service) with no concrete need
- Ignoring the user's stated constraints or non-goals
- Skipping the "deliberately not included" list — it is how the chef shows
  restraint
- Leaving a gap unresearched and hoping
