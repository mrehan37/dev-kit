# Pipeline Dry Run A — Barbershop Online Booking

**Brief:** “I run a barbershop and want online booking so we get fewer phone calls.”

This is a process validation, not an approved project. Because there is no live
user answering the intake, the answers below are explicit simulated assumptions;
a real run would stop for confirmation before capability mapping.

## A. Intake question batches

**Restatement:** You want customers to choose a service and an available time
online, reducing the calls staff must handle. The language signals a
non-technical user, so the questions stay focused on the shop rather than tools.

### Batch 1 — Shape of the shop

1. Is this for one location, and how many barbers need separate calendars?
2. Should customers choose a barber, choose “any available,” or both?
3. Should people book as guests with contact details, or create an account?
4. Do you already use a website, calendar, point-of-sale system, or booking tool
   that this must work with?

**Simulated answer:** One location, three barbers, both staff-choice options,
guest booking, and no existing website or booking system.

### Batch 2 — Booking rules and public information

1. Do services have different prices and durations?
2. Should customers be able to cancel or reschedule themselves, and how late?
3. Do staff need a simple place to manage availability and block time off?
4. Should the site also show services, prices, opening hours, location, and
   contact details?

**Simulated answer:** Services vary; customers may change bookings until 24
hours before; staff manage their own availability; include the small public site.

### Batch 3 — Payment, reminders, and constraints

1. Should customers pay a deposit, pay in full, or pay in the shop?
2. Would email reminders be enough, or are paid text reminders important?
3. Which country is the shop in, and roughly how many bookings do you expect
   each month?
4. Is there a launch date or monthly software budget, and who will update the
   shop information?

**Simulated answer:** A UK shop; pay in the shop; email reminders only; a few
hundred bookings per month; low ongoing cost; no fixed deadline; and the owner
maintains the information.

## B. Confirmed Understanding

For this dry run, assume the hypothetical user confirms the following.

- **Problem and desired outcome:** Let customers self-book so the shop receives
  fewer scheduling calls and staff avoid double bookings.
- **Assumed technical level:** Non-technical.
- **In scope:** A responsive public site; services, prices, hours, location, and
  contact content; three staff calendars; guest booking; barber or first-
  available choice; staff availability management; customer cancellation and
  rescheduling up to 24 hours before; email reminders; public-site quality
  baseline.
- **Out of scope for v1:** Customer accounts, online payments or deposits, SMS,
  loyalty/POS integration, a custom booking engine, and multiple locations.
- **Decisions the user made (simulated):** One UK location, three barbers, guest
  booking, pay in person, email reminders, and low recurring cost.
- **Decisions deferred to the chef:** Framework, hosting, booking provider,
  technical quality tooling, and keeping the small amount of site content in the
  repository rather than adding a CMS.
- **Open risks / unknowns:** Exact services and durations, branding/domain, and
  confirmation that the selected Square plan includes the required reminder and
  cancellation controls at launch.

## Gap research

The catalog has no appointment-booking ingredient. Research therefore produced
this candidate before Selection.

### Square Appointments (researched 2026-09-06, not yet in catalog)

- **Type:** service
- **Category:** Appointment booking and staff scheduling
- **What it is:** A hosted booking site and staff-calendar service that can also
  be linked or embedded from a separate website.
- **Problem it solves:** Customer self-booking, availability, staff calendars,
  reminders, cancellation rules, and optional payments without a custom booking
  backend.
- **When to use / When NOT to use:** Use for a service business that wants a
  managed booking workflow; do not use when the business needs unusual scheduling
  logic or must avoid Square as its customer/POS platform.
- **Works with:** Its own Square Online site or an external site via a booking
  link/button/embed.
- **Cost / licensing:** In the UK, the official page lists Free at £0 for one
  location, Plus at £29/month/location, and Premium at £69/month/location;
  payment-processing fees apply only if online payments are enabled.
- **Maintenance / status:** Established hosted product; current feature and plan
  entitlement must be verified when the project is approved.
- **Alternatives considered:** A custom booking engine (rejected as unnecessary
  cost and risk for this brief); other salon-booking products would need regional
  pricing research if Square is unsuitable.
- **Docs:** [Square Appointments setup](https://squareup.com/help/gb/en/article/5355-set-up-online-booking-with-square-appointments) · [UK pricing](https://squareup.com/gb/en/appointments/pricing)
- **Notes for chef:** Start with a booking link or button. Use an embed only after
  checking its mobile and keyboard behavior. Do not build customer accounts or a
  second calendar database around it.

## C. Selection

| Capability | Decision | Ingredient(s) | Why | Also considered | Source |
|---|---|---|---|---|---|
| Public shop site | use | Astro | A small content-first site does not need an application framework | Next.js; more application machinery than needed | [catalog](../catalog/frameworks.md) |
| Styling | use | Tailwind CSS | Gives a consistent responsive design-token layer without a component suite | Plain CSS; viable but less standardized for the scaffold | [catalog](../catalog/frontend.md) |
| Booking and staff availability | use | Square Appointments | Solves the actual scheduling problem without custom backend code | Custom booking engine; unnecessary | researched |
| Email reminders | use | Square Appointments | Keeps booking and reminder state in one service | Resend; would require owning booking events and email logic | researched / [catalog](../catalog/services.md) |
| Hosting | use | Cloudflare Pages | Fits a static Astro site and can start at low cost | Vercel; its free tier is not for this commercial use | [catalog](../catalog/services.md) |
| Customer database and auth | none | none beyond Square | Guest bookings are managed by the selected service | Custom Postgres/auth; no requirement | none |
| Public-site baseline | build | semantic HTML, metadata, sitemap, `robots.txt`, 404, accessibility and reduced motion | Relevant professional defaults | Heavy animation and app-only states; not relevant | [standard](../standards/quality-baseline.md) |
| Tooling | use | TypeScript + Biome | Basic type, lint, and format checks with little overhead | Full E2E suite; disproportionate for the thin site | [catalog](../catalog/quality.md) |

**Deliberately not included:** No custom database, authentication, Redis, CMS,
payment integration, animation library, or bespoke scheduling engine. No separate
email service is needed while Square owns reminder delivery.

**Cost roll-up:** Square can begin at £0/month for one location, subject to final
feature verification; Plus is £29/month/location if the needed controls require
it. Cloudflare can begin on its free tier. A domain is an additional registrar
cost. There are no card fees because online payment is out of scope.

## D. Filled recipe

# Recipe: Barbershop Online Booking

**Prepared for:** A one-location barbershop owner who wants fewer booking calls  
**Date:** 2026-09-06  
**Assumed technical level:** non-technical  
**Status:** proposed — awaiting approval

---

## 1. Summary

We will create a simple mobile-friendly shop website where customers can see the
services and book an available time with a chosen barber—or the first available
one. Square will handle calendars and reminders so the shop does not have to run
custom scheduling software.

## 2. How it will work

Customers open the site, review services and prices, and select **Book now**.
Square shows live availability and collects their contact details. The booking
appears in the staff calendar, Square sends the agreed reminder, and customers
can use their booking link to cancel or reschedule within the shop's policy.

The owner edits basic site content in the project and manages appointments,
availability, and time off in Square's dashboard.

## 3. The stack

- **Website framework:** Astro — fast and intentionally small for a public
  information site — free/open-source.
- **Styling:** Tailwind CSS — consistent mobile and desktop presentation —
  free/open-source.
- **Booking and reminders:** Square Appointments **[researched — not yet in
  catalog]** — managed calendars and booking instead of custom software — £0,
  £29, or £69 per location/month depending on confirmed plan needs.
- **Hosting:** Cloudflare Pages — suitable for a static site and low initial
  traffic — free tier, then usage-based.
- **Tooling:** TypeScript + Biome — type, lint, and formatting checks —
  free/open-source.

## 4. Key decisions & trade-offs

- **Managed booking:** Choose Square instead of building a booking engine. This
  gives up some custom behavior but sharply reduces cost, risk, and maintenance.
- **Small site, not a web app:** Choose Astro rather than Next.js because the
  custom portion is public content plus a booking handoff.
- **No online payment:** Customers pay in the shop. Deposits can be reconsidered
  if no-shows become a measurable problem.

## 5. Included by default

- Responsive layout and touch-friendly controls
- Semantic HTML, keyboard access, and visible focus states
- Reduced-motion support and no unnecessary entrance animation
- Page titles, descriptions, Open Graph metadata, sitemap, `robots.txt`, and a
  useful 404 page
- Optimized images, clear external-booking handoff, and graceful booking-link
  failure messaging
- Type-check, lint, build, and manual mobile/keyboard booking-flow verification

## 6. Deferred / out of scope for v1

- Customer accounts, deposits/online payment, SMS reminders, loyalty/POS
  integration, multiple locations, and custom booking logic
- A CMS; the owner can request one later if editing repository content is not
  practical

## 7. Rough cost

| Item | Free tier covers | Approx. cost at a few hundred bookings/month |
|---|---|---|
| Square Appointments | One UK location on Free | £0/month initially; £29/month if Plus features are required |
| Cloudflare Pages | Expected initial site traffic | £0 initially; usage-based beyond limits |
| Domain | No | Registrar price, paid separately |

## 8. Build outline

1. Scaffold the Astro site and baseline tooling.
2. Add the service, price, hours, location, and contact pages/content.
3. Configure Square staff, services, availability, and booking policies.
4. Add and verify the booking button or accessible mobile-friendly embed.
5. Apply public-site metadata and accessibility checks, deploy, and document it.

## 9. Assumptions & open questions

- The shop is eligible for Square's UK service and accepts Square as the system
  that stores customer booking details.
- Confirm the exact Square tier after checking reminder and cancellation feature
  entitlements.
- Brand assets, domain, final services/durations, and cancellation wording still
  need to be supplied; safe plain defaults can be shown for approval.

## 10. Approval

Reply **approved** to create the project, or tell me what to change. Nothing is
created until you approve.
