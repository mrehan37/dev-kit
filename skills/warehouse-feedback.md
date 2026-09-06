# Skill: Warehouse Feedback

**Purpose:** after a project is built, feed genuinely reusable lessons back into
the warehouse, so it grows from real work rather than speculation.

**Use when:** a project scaffolded via [`scaffold-project.md`](scaffold-project.md)
is complete, or a later piece of work surfaced something reusable.

**This is how the warehouse evolves.** Keep it small and deliberate.

---

## What to feed back

1. **Catalog additions** — ingredients used that were not catalogued (including
   anything from [`gap-research.md`](gap-research.md)). Add them now, using the
   catalog entry format, verified against official sources. Include a real
   "When NOT to use".
2. **Catalog corrections** — entries that were wrong, stale, or missing a
   trade-off the project exposed. Fix them.
3. **Patterns worth a template or skill** — only if genuinely reused. One
   occurrence is not a pattern: note it, and add the template/skill when it
   recurs. When it does, put a minimal, project-agnostic version in
   `templates/` or `skills/`.
4. **Standards that proved their worth** — a checklist or rule the project
   benefited from goes in `standards/`, phrased as adaptable guidance, not a
   mandate.

## What NOT to feed back

- Project-specific code, config, or the recipe itself. **Ingredients, not
  recipes.**
- Secrets, credentials, client details.
- A technology added "for completeness" that the project did not actually use.
- Large speculative additions. The warehouse grows one proven piece at a time.

## Process

1. Review what the project used against the catalog. List the deltas.
2. For each delta, decide: add / correct / note-for-later / ignore.
3. Make the change using the relevant entry format; verify external facts.
4. Check for duplicates — fold close matches into an existing entry's
   "Alternatives" rather than adding a near-duplicate.
5. Keep entries to what helps a future selection decision.

---

## Anti-patterns

- Copying project code or the recipe into the warehouse
- Adding a template or skill after a single use
- Bulk-importing a technology's whole ecosystem because one part was used
- Recording unverified version numbers or pricing
- Letting the catalog accumulate near-duplicate entries
