<!--
Recipe template. Filled in by the chef during the recipe phase; the completed
file ships in the generated project as docs/recipe.md.
Adapt length to the project. Delete sections that genuinely do not apply.
Human-readable explanation first in every section; technical detail after.
-->

# Recipe: <project name — working title, confirmed at scaffold>

**Prepared for:** <who / what they asked for>
**Date:** <date>
**Assumed technical level:** non-technical | somewhat technical | technical
**Stack decision path:** chef chooses | user has full stack | user has some |
existing codebase | resources only | no custom build
**Status:** proposed — awaiting approval

---

## 1. Summary

<Two or three sentences, in the user's own terms: what we will build and the
outcome it delivers.>

## 2. How it will work

<The shape of the system at the user's level.
 Non-technical: what they and their users will be able to do, step by step; an
 analogy if it helps.
 Technical: components and how they fit — frontend, backend/API, database,
 external services, hosting.>

## 3. The stack

<Grouped by purpose. One line each: what it is for — why this choice — cost note.
 Tag each: [user choice] / [from existing codebase] / [chef recommendation] /
 [researched — not yet in catalog]. Also mark [experimental / thinly maintained]
 where it applies. Researched items get a full entry in the appendix, not here.>

- **Framework:** <name> — <why> — <cost> — [tag]
- **Hosting:** <name> — <why> — <cost> — [tag]
- **Database:** <name> — <why> — <cost> — [tag]
- **Authentication:** <name> — <why> — <cost> — [tag]
- **<Other capability>:** <name> — <why> — <cost> — [tag]
- **Tooling:** <lint/format, tests, CI> — <why> — [tag]

## 4. Key decisions & trade-offs

<Only the decisions that materially shape the project.>

- **<Decision>:** chose <X>. Also considered <Y> (<reason it lost>). <Why X fits
  here.>
- **<Prescribed-choice conflict, if any>:** you asked for <Z>; it works against
  <requirement>. Options: <A> or <B>. Recommendation: <…>.

## 5. Included by default

<Baseline quality items being applied, from the quality-baseline cascade — so the
user can see they are covered without having asked.>

- <e.g. responsive layout; accessible components + keyboard nav; reduced-motion
  support; loading / empty / error states; input validation; environment/secret
  handling; error monitoring; SEO metadata + sitemap + robots (public sites);
  lint + type check in CI>

## 6. Deferred / out of scope for v1

- <Explicitly not being built now, and why it can wait.>

## 7. Rough cost

<Free, or approximate monthly bands at the expected scale. List the paid /
usage-based pieces.>

| Item | Free tier covers | Approx. cost at <scale> |
|---|---|---|
| <service> | <yes / limits> | <$ / mo> |

## 8. Build outline

<The phases of implementation — shape of the work, not a schedule.>

1. Scaffold + baseline setup
2. <core capability>
3. <next capability>
4. <integrations>
5. Deploy + project docs

## 9. Assumptions & open questions

- <Assumption the plan rests on.>
- <Question still to confirm — safe to default as <…> if unanswered.>

## 10. Approval

Reply **approved** to create the project, or tell me what to change. Nothing is
created until you approve.

---

## Appendix: researched options

<Full entry (catalog field set) for anything in the stack tagged
[researched — not yet in catalog]. Delete this section if there were none.
After the project is built, these are handed to warehouse-feedback for the
catalog.>

### <Name>   (researched <date>, not yet in catalog)
- **Type / Category:** …
- **What it is / Problem it solves:** …
- **When to use / When NOT to use:** …
- **Works with / Cost / licensing / Maintenance:** …
- **Alternatives:** …
- **Docs:** <official URL>
- **Notes for chef:** …
