# Skill: Recipe

**Purpose:** assemble the Selection into a proposed architecture and
implementation plan, explained in the user's language, and get **explicit
approval** before any project is created.

**Inputs:** the confirmed **Understanding** from
[`project-intake.md`](project-intake.md) (outcome, technical level, scope,
assumptions, non-goals, scale) **and** the **Selection** from
[`capability-mapping.md`](capability-mapping.md) (chosen ingredients, cost
roll-up, any researched-option appendix from
[`gap-research.md`](gap-research.md)).

**Use when:** both inputs exist and any gaps are resolved.

**Feeds:** [`scaffold-project.md`](scaffold-project.md) — but only after the user
approves.

**Template:** [`../templates/recipe.md`](../templates/recipe.md).

---

## What a recipe is

A recipe is **project-specific**. It is the one place the warehouse's ingredients
are combined for a particular problem. It is not stored in the warehouse; it
ships inside the generated project (`docs/recipe.md`).

## Rules

- **Human-readable first, technical detail after.** Every section leads with what
  it means for the user; specifics follow.
- **Match the user's technical level** (from intake). Non-technical: describe what
  they will be able to do, use analogies, keep stack names in a labelled list
  with plain reasons. Technical: component architecture, named choices,
  trade-offs up front.
- **Do not bury the decision.** The approval ask is explicit and unmissable.
- **As short as the project allows.** A landing page recipe is a page. A
  multi-role app recipe is longer but still scannable.
- **No hidden defaults.** Anything the chef decided on the user's behalf is
  listed, with the default chosen and why.

## Sections (see the template)

1. **Summary** — what we will build, restating the outcome in the user's words.
2. **How it will work** — the shape of the system at the user's level.
3. **The stack** — grouped by purpose (framework, data, auth, payments, …), each
   item **one line**: what it is for + why this one + cost note. Mark anything
   researched / not yet in the catalog, and anything experimental or thinly
   maintained. A researched option's full detail goes in an appendix, not inline.
   Note which choices came from the user or an existing codebase (not the chef).
4. **Key decisions & trade-offs** — only the handful that matter. Each: what we
   chose, what else was considered, why. Include any conflict between a
   user-prescribed choice and the requirements, flagged here rather than
   silently overridden.
5. **Included by default** — the baseline quality items being applied (from the
   cascade), so the user sees they are covered.
6. **Deferred / out of scope for v1** — explicitly, so expectations are set.
7. **Rough cost** — free, or approximate monthly bands at the user's expected
   scale, from the Selection's cost roll-up.
8. **Build outline** — the phases of implementation as a short list (shape of the
   work, not a schedule).
9. **Assumptions & open questions** — anything still to confirm.
10. **Approval** — "Reply *approved* to create the project, or tell me what to
    change." Nothing is scaffolded before this.

## After presenting

- **Approved** → proceed to `scaffold-project.md`.
- **Changes requested** → revise the affected step (intake, mapping, or research)
  and re-present the recipe. Do not scaffold a half-agreed recipe.
- **Silence / "looks fine, go"** from a non-technical user → confirm once,
  plainly ("I'll create the project now with everything above — OK?"), then
  proceed.

---

## Anti-patterns

- A wall of framework names with no plain-language explanation
- Hiding the approval ask at the bottom of a long document with no signpost
- Presenting decisions the chef made as if the user made them
- Omitting cost, or stating cost only as "usage-based" with no figure
- Scaffolding before explicit approval
- Recommending a stack that fights the user's stated constraints without saying so
- Presenting a user-chosen or codebase-set technology as if the chef selected it
- Pasting a full researched-option block into the stack instead of one line + an
  appendix entry
