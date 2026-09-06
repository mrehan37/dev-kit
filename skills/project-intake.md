# Skill: Project Intake

**Purpose:** take a person's plain-language description of what they want built
and reach a shared, confirmed understanding of the problem and requirements —
enough to draft a recipe.

**Use when:** someone asks the chef to build, create, or scaffold a new project,
or describes a product/idea they want made.

**Do not use for:** bug fixes, code questions, or requests that are already a
precise engineering task. (An *existing codebase* the user wants extended is in
scope — see step 2b, path 4.)

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
   `../standards/quality-baseline.md`) is stated, not offered as optional.
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

## Step 2b — Establish what the user already has, wants, and has decided

This is core discovery philosophy, not one throwaway question. **Devkit
recommends; it does not dictate.** Before recommending any technology, find out
what the user has already decided and how much of the decision they want to own.

Ask naturally, at a natural point — for a non-technical user this may be as light
as *"Do you have any preferences for how this is built, or shall I choose?"*; for
a technical user, *"Do you already have a stack in mind, or want me to recommend
one?"* Never force this on a user who plainly does not care about the tooling.

Route to one of these paths (a user can be a mix):

1. **"I have a stack in mind."** Ask what it is. Treat every named choice as a
   **user requirement** and build around it. Only challenge a choice when there is
   a real technical, compatibility, security, maintenance, or project-fit
   problem — and then explain the reason *before* suggesting an alternative,
   leaving the decision with the user.
2. **"I know some of it."** Preserve what they have chosen. Recommend only the
   missing pieces.
3. **"I don't know / you choose."** The chef evaluates the requirements, inspects
   the warehouse, researches current options if needed, and recommends a stack
   with reasons.
4. **"I already have a project/codebase."** Treat it as the source of truth.
   Inspect it (languages, framework, dependencies, architecture, conventions).
   Preserve its established choices unless the user explicitly asks to change
   them. New work matches what is there.

### Scope of help requested

A knowledgeable user may not want architecture help at all — e.g. *"I know what
I'm building and my stack; I just need MCPs and AI skills."* Recognise this and
**do not redesign their stack.** Establish which kinds of help they want, through
conversation rather than a rigid menu:

- Technology / architecture
- AI skills · MCPs · libraries/dependencies
- UI/UX resources · APIs / third-party services
- Templates / boilerplates · deployment / DevOps
- Research / references
- All of the above

Record the chosen scope; later stages only work within it.

### Could this need no custom build at all?

If an existing hosted product would meet the need with little or no code (e.g. a
booking product, a form builder, a no-code site), say so. "Recommend a product,
build nothing" is a valid outcome — carry it into the recipe as the proposal.

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
- **Stack decision path** — which of the step 2b paths applies (has a stack /
  knows some / chef chooses / existing codebase), and the choices already fixed
- **Scope of help requested** — what kinds of help are in scope (from step 2b)
- **Existing codebase** — if any: location and the established choices to preserve
- **In scope** — the capabilities the project needs
- **Out of scope for v1**
- **Decisions the user made** — with their choice
- **Decisions deferred to the chef** — each with the default that will be assumed
- **Rough scale / cost drivers** — expected users/volume and anything that will
  drive a usage-based bill (needed for the recipe's cost estimate); mark unknowns
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
- Reconsidering or redesigning a stack the user has already chosen, or that an
  existing codebase already sets, without a real problem to point to
- Pushing architecture help on a user who only asked for resources (MCPs, skills,
  templates, …)
- Recommending a custom build when a hosted product would clearly do

---

## Short example (appointment booking, non-technical user)

**User:** "I run a small barbershop and want customers to book appointments
online instead of calling."

**Chef (step 1, restate):** "You want a site where customers pick a service and a
time slot and book it themselves, so the shop takes fewer phone calls. Have I got
that right?"

**Chef (step 2):** treats the user as non-technical — outcome language, no tech
nouns.

**Chef (step 2b):** "Do you have any preference for how it's built, or shall I
choose the tools and explain them simply?" → User: *"You choose."* → path 3
(chef chooses); scope of help = whole project.

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
