# Devkit

Devkit is an intended knowledge system for helping people turn an idea into a well-reasoned software project; it keeps a **warehouse** of reusable ingredients. It exists so the **chef** (your AI assistant - Codex, Claude, or any other; Devkit does not assume a specific one) can reuse reliable knowledge and proven building blocks without forcing users to choose technologies or understand implementation details before their needs are clear.

## Start here

Clone this repo somewhere local, then point your AI assistant at it. A prompt like:

``` prompt
Read the Devkit warehouse at `<path to your clone>` — start with `README.md`,
then follow the pipeline in `skills/` in order (project-intake →
capability-mapping → gap-research → recipe → scaffold-project). I want to build:
**`<describe what you want, in plain language>`**. Ask me questions as the
skills direct, inspect the `catalog/` before looking elsewhere, and don't
scaffold anything until I approve a recipe.
```
If you only want resources (MCPs, AI skills, templates, UI/UX, references) rather
than a whole project, say so — the chef will scope the conversation to that and
leave your stack alone.

## Metaphor

- **Warehouse** — this repository; the shared store of reusable ingredients.
- **Ingredient** — any reusable building block or piece of knowledge: a template, skill, MCP, service, reference, or standard.
- **Chef** — your AI assistant (Codex, Claude, or another). Devkit is not tied to one.
- **Recipe** — the architecture and implementation plan the chef proposes for a single project.
- **Finished project** — the scaffolded project produced once you approve the recipe.

## Ingredients, not recipes

The warehouse holds **ingredients**, not finished recipes. A user describes what they want in plain language, and the chef gets to work:

1. Understand the real problem and gauge how technical the user is.
2. Ask progressive, context-aware questions. Questions become more specific as the requirements become clearer, feature-specific questions appear only when that feature matters, and "I'm not sure" is always a valid answer.
3. Inspect the warehouse for suitable ingredients before looking elsewhere. Research the internet only when the warehouse does not contain enough reliable information.
4. Propose a **recipe**: an architecture and implementation plan explained in language the user can understand.
5. Wait for the user's explicit approval of that recipe.
6. Only after approval, ask where the project should be created and scaffold it there.
7. Document the finished project well enough that another developer or AI can understand what was built, how it works, and why key decisions were made.

## The pipeline

Each stage above is a written procedure in [`skills/`](skills/) that the chef
follows in order:

| Stage | Skill | Output |
|---|---|---|
| Understand the request | [`skills/project-intake.md`](skills/project-intake.md) | A confirmed *Understanding* of the problem and scope |
| Choose ingredients | [`skills/capability-mapping.md`](skills/capability-mapping.md) | A *Selection* of ingredients from the [`catalog/`](catalog/), with reasoning |
| Fill gaps | [`skills/gap-research.md`](skills/gap-research.md) | Researched options for capabilities the catalog does not cover |
| Propose the plan | [`skills/recipe.md`](skills/recipe.md) + [`templates/recipe.md`](templates/recipe.md) | A *recipe* the user must explicitly approve |
| Build it | [`skills/scaffold-project.md`](skills/scaffold-project.md) + [`templates/project-docs.md`](templates/project-docs.md) | A scaffolded, documented project |
| Grow the warehouse | [`skills/warehouse-feedback.md`](skills/warehouse-feedback.md) | Vetted additions/corrections to the warehouse |

The warehouse (catalog + skills + templates + MCPs + services + references +
standards) is the reusable part.
The recipe and the finished project are project-specific and live with the
project, not here.

## What belongs here

The warehouse may contain reusable knowledge, references, templates, skills, MCPs, service information, dependency guidance, standards, and carefully chosen examples. These should be broadly useful across real projects and maintained close to the work that proves their value.

It should not contain project-specific application code, prewritten recipes, one-off solutions, secrets, or a giant manually maintained encyclopedia. The warehouse should grow from lessons and reusable assets discovered through real projects rather than being filled speculatively.

## Quality baseline

Devkit does not only pick project-specific ingredients; every generated project should begin with a sensible professional baseline.

**Default does not mean mandatory.** Defaults apply when relevant and can be overridden when they do not fit the project.

The chef reasons through four levels in order: global defaults → project-type defaults → project-specific requirements → exceptions and overrides. Later levels win over earlier ones.

The chef should ask itself, "What would a competent professional implementation normally include here that the user didn't explicitly request?" It should still avoid unnecessary complexity and overengineering: animation being available does not justify heavy entrance effects, SEO matters for a public site but not an internal dashboard, and smooth scrolling may suit a marketing site but not an API. Accessibility and reduced-motion support are baseline considerations for user-facing interfaces. The concrete baseline lives in `standards/quality-baseline.md` and is expected to evolve from real projects.

## Definitions

- **Ingredients:** Any reusable input that can help plan or build multiple projects.
- **Templates:** Adaptable starting structures for recurring files or project shapes; they are not complete solutions.
- **Skills:** Instructions that teach the chef how to perform a particular kind of work consistently.
- **MCPs:** Model Context Protocol integrations that let the chef use approved external tools or data sources.
- **Services:** External platforms or capabilities a project may use, together with practical integration guidance.
- **References:** Trusted documentation, research, examples, and decision-support material.
- **Recipes:** Project-specific architecture and implementation plans assembled from requirements and suitable ingredients.
- **Standards:** Reusable rules and quality expectations for areas such as security, accessibility, testing, documentation, and maintainability.

## Adding an ingredient

A developer should add an ingredient only after a real need demonstrates that it is reusable:

1. Check that an equivalent ingredient does not already exist.
2. Place it in the appropriate warehouse category: `templates/`, `skills/`, `mcps/`, `services/`, `references/`, or `standards/`.
3. Explain what it is, when to use it, when not to use it, and where it came from.
4. Include only the minimum reusable material and remove project-specific details and secrets.
5. Link authoritative sources and note important assumptions, compatibility limits, or maintenance needs.
6. Validate it through practical use and update or retire it when it becomes inaccurate.

## How the chef should use the warehouse

The chef should begin with the user's goals, explain concepts in human-friendly language first, and move into technical implementation only as needed. It should ask the fewest useful questions, adapt them to previous answers, and never assume that a user knows technical terminology.

Before proposing a recipe, the chef should search the warehouse, evaluate each relevant ingredient against the actual requirements, and research externally only to fill genuine gaps. Finding an ingredient does not make it necessary: the chef must select deliberately and must not install or combine everything it discovers.

The warehouse is supporting material, not an authority that replaces judgment. Every recipe remains specific to the user's problem, requires explicit approval, and must be understandable before implementation begins.

## Devkit recommends; it does not dictate

The chef first establishes what the user has already decided and how much of the
decision they want to own (see [`skills/project-intake.md`](skills/project-intake.md),
step 2b): they may have a full stack in mind, part of one, none, or an existing
codebase — or they may only want resources around a stack they have already
chosen. Selection then follows a fixed order of authority
([`skills/capability-mapping.md`](skills/capability-mapping.md)):

1. Explicit user requirements
2. Existing project / codebase constraints
3. The user's chosen technology stack
4. Organization / project standards
5. Devkit recommendations (catalog + standards)
6. Current official documentation / research
7. The chef's own recommendation

A user's or a codebase's technology choice is **never** replaced just because the
catalog holds a different or newer option. If such a choice has a real technical,
compatibility, security, maintenance, or fit problem, the chef names the problem
and the reason first — the decision still belongs to the user.
