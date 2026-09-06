# Skill: Project Intake

**Purpose:** take a person's plain-language description of what they want built
and reach a shared, confirmed understanding of the problem and requirements —
enough to draft a recipe.

**Use when:** someone asks the chef to build, create, or scaffold a new project,
or describes a product/idea they want made.

**Do not use for:** changes to an existing codebase, bug fixes, code questions, or
requests that are already a precise engineering task.

**Comes before:** capability mapping and recipe generation (later phases).
**Feeds:** the "Understanding" summary in step 6, which the recipe is built from.

---

## Principles

1. **Human language first.** Understand the problem in the user's terms before any
   technology is named.
2. **Progressive.** Start broad; get specific only as the picture clears.
3. **Every question must matter.** If the answer would not change what gets built,
   do not ask it.
4. **"I'm not sure" is always valid.** Record it as a chef decision with a default
   and move on. Never make the user feel they must know something.
5. **Ask about a feature only when it is in scope.** Payments questions only if
   there is money; reminder questions only if there is scheduling; and so on.
6. **Do not ask what a sensible default already answers.** Baseline quality
   (responsive, accessible, sensible errors/loading states — see
   `standards/quality-baseline.md`) is stated, not offered as optional.
7. **Small batches.** 2–4 questions at a time on one theme, not a form.

---

## Step 1 — Understand the problem, not the solution

Before anything else:

- Restate, in plain language, what you believe they want and why. Ask them to
  correct it.
- Establish: who it is for, what outcome they want, what "it worked" looks like,
  and any signal of timeline, budget, or who will maintain it.
- Do **not** discuss frameworks, languages, or hosting yet.

If the request is phrased as a solution ("build me a Next.js app with Postgres"),
still confirm the underlying problem — but a clearly technical user may genuinely
just want that; treat their choices as constraints (see step 5), don't lecture.

## Step 2 — Gauge how technical the user is

Read it from how they speak; never quiz them.

| Level | Signals | How to run intake |
|---|---|---|
| Non-technical | Describes outcomes and features in business/user terms; no tech nouns; may say "I don't understand any of this" | Ask only about outcomes, users, features, content, budget. Translate everything. Never ask them to choose technology. Offer defaults with a one-line plain reason. |
| Somewhat technical | Knows some terms; has opinions ("I heard WordPress is slow"); can follow a short explanation | Mix outcome and light technical questions. Confirm preferences. Explain trade-offs briefly. |
| Technical | Names stacks, hosting, constraints, trade-offs | Go straight to constraints, existing systems, must-use / must-avoid choices, scale, non-negotiables. |

Treat the level as a dial you keep adjusting, not a fixed label.

## Step 3 — Progressive questioning

- Open with the broad shape (steps 1–2). Then narrow.
- One theme per batch. Keep batches short.
- For non-technical users, prefer concrete multiple-choice over open questions:
  *"When someone books, should they pay a deposit, pay in full, or pay nothing
  online?"* beats *"How do you want payments to work?"*
- When the user says "I'm not sure" / "you decide": reply with the default you
  will assume, in one line, and continue. Log it for the Understanding summary.
- Stop asking when **all** of these are true:
  - You can list the project's capabilities and rough scale.
  - You know the handful of decisions that materially change what gets built.
  - Every remaining unknown is safe to default without surprising the user on
    cost or shape.

Then produce the Understanding (step 6).

## Step 4 — Capability checklist

A memory aid so nothing large is missed. Raise an item **only if plausibly
relevant** to what you heard in step 1.

- Core purpose and the primary action a user takes
- Audience and rough scale: a handful / hundreds / many; public or internal-only
- Content: who creates and edits it, and how often
- Accounts: do end users log in? Staff or admin users? Roles and permissions?
- Data: what is stored; anything sensitive or regulated (health, payments,
  children, personal data)
- Money: one-off payments, subscriptions, deposits, refunds, invoicing, payouts
  to third parties, tax/VAT handling
- Communication: email, SMS / WhatsApp, push, in-app; transactional vs marketing
- Scheduling / booking: availability rules, calendars, cancellation and
  rescheduling, reminders, time zones
- Integrations: existing tools the user already relies on that must connect
- Search, maps / location, file or media uploads — only if hinted
- Branding and design: existing brand assets? A design provided? Or "make it look
  professional" with no specifics?
- Platforms: web, a real mobile app (confirm they need the app stores), offline
  use
- Hosting and operations: existing accounts or preferences (technical users)
- Constraints: deadline, budget, who will maintain it, must-use or must-avoid
  technology
- Non-goals: what is explicitly out of scope for the first version

## Step 5 — Solution-shaped requests

If the user prescribes technology:

- Record each choice as a **constraint or preference**, not a debate.
- Still confirm the problem and scope.
- If a prescribed choice works against the stated requirements, note it — raise it
  in the recipe as a flagged trade-off, not mid-intake.

## Step 6 — Output: the Understanding

Present this back and ask for confirmation or corrections before moving to a
recipe:

- **Problem and desired outcome** — plain language
- **Assumed technical level**
- **In scope** — the capabilities the project needs
- **Out of scope for v1**
- **Decisions the user made** — with their choice
- **Decisions deferred to the chef** — each with the default that will be assumed
- **Open risks / unknowns**

On confirmation, proceed to capability mapping and recipe generation. On
correction, revise and re-present.

---

## Anti-patterns

- Interrogating the user with a long list of questions up front
- Asking the user to explain why a question matters, or answering that yourself at
  length
- Technical jargon aimed at a non-technical user
- Presenting baseline quality (responsive, accessible, error/loading states) as
  optional extras
- Continuing to ask questions after you already have enough to draft a recipe
- Silently defaulting a decision that materially changes cost or scope instead of
  surfacing it
- Naming frameworks or hosting before the problem is understood

---

## Short example (appointment booking, non-technical user)

**User:** "I run a small barbershop and want customers to book appointments
online instead of calling."

**Chef (step 1, restate):** "You want a site where customers pick a service and a
time slot and book it themselves, so the shop takes fewer phone calls. Have I got
that right?"

**Chef (step 2):** treats the user as non-technical — outcome language, no tech
nouns.

**Chef (step 3, batch 1 — the shape):**
- "Roughly how many barbers' calendars need to be bookable — just you, or a few
  staff?"
- "Do you want customers to create an account, or book with just a name, email,
  and phone number?"
- "Should the site also show your prices and services, like a small website, or
  only handle bookings?"

**Chef (batch 2 — payments, only because bookings often involve them):**
- "When someone books, should they pay a deposit online, pay the full amount, or
  pay nothing until they're in the chair?"
- User: *"Not sure."* → Chef: "I'll assume no online payment for now — customers
  book for free and pay in person. We can add deposits later." (logged as a chef
  decision)

**Chef (batch 3 — reminders, because no-shows are the real problem):**
- "Would you like automatic reminders sent before appointments? If so, email is
  simplest and free; text messages cost a small amount per message."

**Not asked:** which framework, whether it works on phones (baseline), where it's
hosted (chef decides), what database (chef decides). Those go in the recipe with
defaults, explained simply.

**Chef (step 6):** presents the Understanding — problem, "non-technical",
in scope (services list, multi-staff calendar, guest booking, email reminders),
out of scope for v1 (online payment, customer accounts, SMS), deferred decisions
with defaults, and open questions — for the user to confirm.
