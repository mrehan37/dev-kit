<!--
Project documentation template. The chef fills this in during scaffolding and
places it at docs/README.md (or docs/overview.md) in the generated project.
Goal: enough context for another developer or AI to understand what was built
and why, and to run and deploy it. Keep it current as the project changes.
-->

# <Project name>

<One paragraph: what this project is and the outcome it delivers, in plain
language.>

## Why it was built this way

<Two or three short paragraphs, or a bullet list, summarising the recipe's key
decisions: the archetype, the framework choice, the data and auth approach, and
any notable trade-off. Link to the full recipe.>

Full recipe and rationale: [`docs/recipe.md`](./recipe.md)

## Stack

| Purpose | Choice | Version | Why |
|---|---|---|---|
| Framework | <name> | <x.y.z> | <one line> |
| Hosting | <name> | — | <one line> |
| Database | <name> | <x.y.z> | <one line> |
| Auth | <name> | <x.y.z> | <one line> |
| <capability> | <name> | <x.y.z> | <one line> |
| Tooling | <lint/format, tests, CI> | <x.y.z> | <one line> |

## Running locally

**Prerequisites:** <runtime + version, package manager, any services>

```
<install command>
cp .env.example .env   # then fill in the values below
<dev command>
```

**Environment variables:**

| Variable | Required | What it is | Where to get it |
|---|---|---|---|
| <NAME> | yes/no | <purpose> | <source> |

## Testing & checks

```
<test command>
<lint command>
<type-check command>
```

## Deployment

**Target:** <platform>

<Steps to deploy. Required environment variables in the host. Build command and
output. Anything manual (DNS, webhook registration, first-run setup).>

## What is included by default

<The baseline quality items applied during scaffolding — responsive layout,
accessibility defaults, validation, error monitoring, SEO basics, etc. — so a
reader knows what is already handled.>

## Deferred / not built yet

<Explicitly out of scope for this version, from the recipe. What it would take to
add each later.>

## Project structure

<Short orientation: the handful of directories that matter and what lives in
each. Not an exhaustive file tree.>
