# Skill: Scaffold Project

**Purpose:** after the user approves a recipe, create the project — a working
foundation with the baseline applied and the chosen ingredients wired in, plus
documentation another developer or AI can pick up.

**Use when:** the user has **explicitly approved** a recipe from
[`recipe.md`](recipe.md).

**Template:** [`../templates/project-docs.md`](../templates/project-docs.md).

---

## Step 1 — Location and name (ask now, not before)

- Ask **where** to create the project (absolute path).
- Check the target: empty directory (good), existing files (confirm what happens),
  inside an existing repo (confirm intent).
- Confirm the exact project name (package name, repo name).

## Step 2 — Scaffold with official tooling

- Use the framework's own create command / official starter as the base. Do not
  hand-assemble what a scaffolder produces correctly.
- Layer the recipe's choices on top (styling, component layer, database client,
  auth, etc.), following each catalog entry's setup guidance.
- Pin versions. Record the exact versions used in the project docs.

## Step 3 — Apply the baseline

Only the items the recipe listed under "Included by default":

- `git init`, a real `.gitignore`, an initial commit
- `.env.example` with keys and dummy values; an env schema validated at startup;
  **no real `.env` committed**
- Lint + format + type-check config, wired to a script and to CI if the recipe
  includes CI
- Project `README.md` (see step 5)
- Error handling scaffolding; loading / empty / error state patterns where there
  is UI
- For public sites: metadata, sitemap, `robots.txt`, semantic layout, 404 page
- Accessibility defaults: semantic elements, visible focus, reduced-motion
  handling

## Step 4 — Wire ingredients, do not build features

For each selected ingredient: install it, configure it, and add **one working
touchpoint** (a health check, a sample query, a protected route, a test email in
dev) so it is proven connected. Stop there. The scaffold is a foundation, not the
finished product — do not build out product features unless the user asks.

## Step 5 — Write the project docs

Fill [`../templates/project-docs.md`](../templates/project-docs.md) into
`docs/` (and a short `README.md` at the root pointing to it):

- What was built and why (the recipe's outcome and key decisions, briefly)
- The stack and the reason for each piece
- How to run it locally (prerequisites, env vars, commands)
- How to deploy it (target, required env vars, steps)
- What is deferred / out of scope for v1
- Where the full recipe lives (`docs/recipe.md` — copy the approved recipe there)

## Step 6 — Verify and report

- Install dependencies; run the dev server and/or build; run lint + type check.
- Fix anything broken in the scaffold.
- Report to the user: what was created, where, how to run it, what each part
  does, and the sensible next steps. Note anything that still needs their input
  (API keys for a paid service, a domain, an account signup).

---

## Anti-patterns

- Scaffolding before explicit recipe approval
- Asking for the location before the recipe is approved
- Committing a real `.env` or any secret
- Hand-rolling a project skeleton a official create-tool would produce
- Building product features beyond a proven wiring touchpoint
- Leaving the project without run/deploy docs
- Not copying the approved recipe into the project
