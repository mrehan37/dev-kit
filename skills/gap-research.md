# Skill: Gap Research

**Purpose:** when capability mapping finds a need the catalog does not cover well,
research current options from authoritative sources, pick one for the recipe, and
draft a catalog entry for later.

**Use when:** [`capability-mapping.md`](capability-mapping.md) flagged a gap — no
fitting catalogued ingredient, or the only fit is niche / experimental /
deprecated / stale.

**Feeds:** the Selection in [`capability-mapping.md`](capability-mapping.md) now
(which then feeds the recipe); the catalog later (via
[`warehouse-feedback.md`](warehouse-feedback.md)).

---

## Rules

1. **Only research real gaps.** Do not re-research capabilities the catalog
   already covers well.
2. **Official sources first.** The project's own site, docs, and repo. Use search
   to find them and to find comparisons, but verify every claim — version,
   pricing, licence, last release, maintenance — against the primary source.
   Treat listicles as leads, not evidence.
3. **Prefer mature and maintained.** Recent commits, real docs, a community,
   visible production use. A tool that is perfect on paper but barely maintained
   is a liability — say so.
4. **Judge against the catalog's own criteria:** fit for the stack and
   constraints, maturity, maintenance, cost and licensing, compatibility, and a
   clear "when NOT to use".
5. **Flag risk honestly.** If the best available option is young, niche, or
   thinly maintained, put that in front of the user in the recipe rather than
   presenting it as a safe default.

## Output

For each gap, a block in the **same field set as a catalog entry** (see
[`../catalog/README.md`](../catalog/README.md)), marked as researched and not yet
catalogued:

```
### <Name>   (researched <date>, not yet in catalog)
- **Type:** …
- **Category:** …
- **What it is:** …
- **Problem it solves:** …
- **When to use:** …
- **When NOT to use:** …
- **Works with:** …
- **Cost / licensing:** …
- **Maintenance / status:** … (note any risk plainly)
- **Alternatives:** … (with the deciding factor)
- **Docs:** <official URL>
- **Notes for chef:** …
```

Then:

1. Add the chosen option as a row in the capability-mapping **Selection** table
   (Source: researched) and add its cost to the cost roll-up.
2. Keep the full block as an appendix to the Selection; the recipe's stack
   section cites it in one line and links to it (the recipe keeps stack items to
   one line each).
3. After the project is approved and built, hand the vetted block to
   `warehouse-feedback.md` to add it to the catalog.

---

## Anti-patterns

- Trusting a "top 10" article over the project's own documentation
- Recording a version number or price without checking the official source
- Adopting an experimental or barely-maintained tool without flagging the risk
- Researching a capability the catalog already answers
- Adding the researched tool to the catalog before it has been used in a real
  project
