# Skill: Capability Mapping & Ingredient Selection

**Purpose:** turn a confirmed Understanding into a concrete set of chosen
ingredients, with reasoning and a list of gaps — ready to assemble into a recipe.

**Use when:** the user has confirmed the Understanding from
[`project-intake.md`](project-intake.md).

**Comes after:** project intake. **Feeds:** gap research (where gaps exist) and
recipe generation.

---

## Decision hierarchy

Every selection in this skill follows this order of authority. A higher item
always wins; Devkit only fills what the items above it leave open.

1. **Explicit user requirements** — what they said they need.
2. **Existing project / codebase constraints** — an established codebase's
   languages, framework, and dependencies are preserved unless the user asks to
   change them.
3. **The user's chosen technology stack** — choices they named in intake
   (step 2b). Treated as requirements.
4. **Organization / project standards** — any standards the user or project
   mandates.
5. **Devkit recommendations** — the catalog and `standards/`.
6. **Current official documentation / research** — for gaps and freshness checks.
7. **The chef's own recommendation** — last, and only where nothing above decides.

**Never replace a choice from levels 1–4 just because the catalog holds a
different or newer option.** If a user-chosen or codebase-set technology has a
real technical, compatibility, security, maintenance, or project-fit problem,
name the problem and the reason first, then offer an alternative — the decision
stays with the user. Devkit is a recommendation system, not a technology
dictator.

## Step 0 — Scope and starting point

Read the Understanding's *stack decision path* and *scope of help requested*
first:

- **Scope of help** limits this skill. If the user asked only for resources
  (MCPs, AI skills, templates, UI/UX, references, …), do **not** select or
  reconsider a framework/data/auth stack. Map only the requested resource types
  to the catalog and stop.
- **Existing codebase** → inspect it and record the fixed choices as decided
  (level 2). Map only the *new* capabilities.
- **User has a full stack** → record all of it as decided (level 3). Selection is
  then mostly additive.
- **User has some / none** → proceed through all steps below for the open parts.
- **No custom build needed** (a hosted product covers it) → the Selection is that
  product plus any glue; skip archetype/framework selection.

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
- For any capability already decided at hierarchy levels 1–4, the "decision" is
  fixed; use the catalog only to find things that **work with** it, not to
  second-guess it.

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
  model, and a rough monthly figure at the Understanding's stated scale. Where a
  cost driver is unknown (region, volume, storage size, egress), state the
  assumption you used so the recipe can carry it as an assumption.

When gap research returns a chosen option, fold it into this table (Source:
researched) and into the cost roll-up before handing off to the recipe.

---

## Anti-patterns

- Bundling in everything discovered in the catalog
- Choosing a bleeding-edge option over a stable one with no stated reason
- Adding infrastructure (cache, queue, search service) with no concrete need
- Ignoring the user's stated constraints or non-goals
- Overriding a user-chosen or codebase-set technology because the catalog lists
  something else — that is a level 1–4 decision, not the chef's to change
- Doing a full stack selection when the user only asked for resources
- Skipping the "deliberately not included" list — it is how the chef shows
  restraint
- Leaving a gap unresearched and hoping
