# Pipeline Dry Run B — iOS Feed REST API

**Brief:** “I need a REST API backing an iOS app — user accounts, a feed of
posts, image uploads, push notifications. We deploy on AWS, team knows
TypeScript.”

This is a process validation, not an approved project. The technical decisions
below use explicit simulated answers because no live stakeholder is present; a
real run would stop for confirmation before capability mapping.

## A. Intake question batches

**Restatement:** You need the backend API only: authenticated users create and
read feed posts with images, and the system sends iOS push notifications. AWS and
TypeScript are constraints. The user is technical, so intake goes directly to
contracts, scale, and operational choices.

### Batch 1 — Boundary, scale, and AWS constraints

1. Is this project only the API and infrastructure, with the iOS client outside
   scope?
2. What launch scale, traffic, AWS Region, and availability target should shape
   the first version?
3. Are Lambda/API Gateway acceptable, or must this run in containers? Is AWS CDK
   in TypeScript the preferred infrastructure-as-code tool?
4. Is there existing AWS networking, CI, data, or identity infrastructure to
   integrate, or a required database?

**Simulated answer:** API and infrastructure only; up to 10,000 monthly active
users and one million requests/month initially; one US Region and standard
single-region availability; Lambda/API Gateway and TypeScript CDK are acceptable;
there is no existing application infrastructure, and PostgreSQL is acceptable.

### Batch 2 — Accounts and feed behavior

1. Which sign-in methods and roles are required?
2. Is the feed global or based on followed accounts, and is it chronological or
   ranked?
3. Who can create, edit, and delete posts, and are comments, likes, moderation,
   or reporting in v1?
4. Is cursor pagination acceptable, and is real-time feed delivery required?

**Simulated answer:** Email/password accounts with user and admin roles; users
follow other users; a reverse-chronological cursor-paginated feed; authors manage
their own posts; admins may remove posts; no comments, likes, ranking, reporting,
or real-time updates in v1.

### Batch 3 — Images and notifications

1. How many images may a post contain, and what size/types should the API accept?
2. Should uploads pass through the API or go directly to object storage using a
   short-lived upload URL?
3. Which event sends a push, and can delivery be asynchronous/best effort?
4. Does the team already have the Apple developer/APNs credentials, and will the
   iOS app register and refresh device tokens?

**Simulated answer:** Up to four JPEG/PNG images of 10 MB each; direct presigned
uploads; best-effort push when a followed account publishes; APNs credentials
will be supplied and the client will manage device-token registration.

### Batch 4 — Operations and non-goals

1. What retention/deletion rules apply to accounts, posts, images, and device
   tokens?
2. Are automated tests, generated OpenAPI documentation, structured logs, and CI
   expected in the scaffold?
3. Is multi-region disaster recovery or a formal uptime target required now?

**Simulated answer:** Account deletion removes owned content and device tokens;
standard backups are enough; tests, OpenAPI, logs, and CI are required; no
multi-region deployment or formal SLA in v1.

## B. Confirmed Understanding

For this dry run, assume the hypothetical user confirms the following.

- **Problem and desired outcome:** Provide a documented REST API that an iOS app
  can use for accounts, followed-user feeds, post/image creation, and APNs push
  notifications, operated within the team's AWS and TypeScript constraints.
- **Assumed technical level:** Technical.
- **In scope:** Email/password accounts; user/admin authorization; follow graph;
  reverse-chronological cursor feed; post create/read/edit/delete; up to four
  direct-uploaded JPEG/PNG images per post; device-token registration; best-
  effort push on followed-user posts; account/content deletion; OpenAPI; health
  check; tests, CI, logs, and single-region AWS deployment.
- **Out of scope for v1:** iOS client code, web/admin UI, social login, comments,
  likes, feed ranking, search, content-reporting workflow, real-time feed updates,
  multi-region failover, and a formal SLA.
- **Decisions the user made (simulated):** AWS, TypeScript, REST, Lambda/API
  Gateway allowed, CDK preferred, 10,000 MAU/one million requests per month,
  PostgreSQL acceptable, email/password, followed-user chronological feed,
  presigned uploads, and asynchronous push.
- **Decisions deferred to the chef:** Hono, Drizzle, Cognito, the exact AWS
  service composition, schema/validation tooling, rate limits, log format, and
  test runner.
- **Open risks / unknowns:** Region-specific cost; Lambda-to-RDS connection
  management and networking; APNs certificate/key ownership; image moderation
  and malware scanning; exact backup/retention targets.

## Gap research

The catalog covers Hono, PostgreSQL, Drizzle, S3, Zod, and quality tooling, but
not the required AWS deploy pattern, Cognito, mobile push, or Hono OpenAPI
integration.

### AWS Lambda + API Gateway (researched 2026-09-06, not yet in catalog)

- **Type:** service / reference
- **Category:** AWS serverless API hosting
- **What it is:** API Gateway HTTP API in front of a Hono handler running on AWS
  Lambda, provisioned with TypeScript CDK and logged to CloudWatch.
- **Problem it solves:** A managed, auto-scaling REST entry point inside the
  team's required cloud without running application servers.
- **When to use / When NOT to use:** Use for bursty or early-stage HTTP APIs;
  prefer containers for sustained high utilization, long-running work, or
  runtime requirements Lambda cannot satisfy.
- **Works with:** Hono's AWS Lambda adapter, Cognito authorizers, S3, RDS, CDK,
  and CloudWatch.
- **Cost / licensing:** Usage-based. AWS lists Lambda free allocation of one
  million requests and 400,000 GB-seconds/month; API Gateway lists one million
  HTTP API calls/month for 12 months for eligible new accounts. Database,
  transfer, and logging are separate.
- **Maintenance / status:** Official AWS services and a documented Hono adapter;
  stable, with region/runtime compatibility checked at scaffold time.
- **Alternatives considered:** ECS/Fargate (better for steady containers but more
  operational and baseline cost); Lambda Function URLs (less API management).
- **Docs:** [Hono on AWS Lambda](https://hono.dev/docs/getting-started/aws-lambda) · [Lambda pricing](https://aws.amazon.com/lambda/pricing/) · [API Gateway pricing](https://aws.amazon.com/api-gateway/pricing/)
- **Notes for chef:** Add route throttles, a health endpoint, JSON structured
  logs, least-privilege IAM, and an explicit RDS connection strategy.

### Amazon Cognito User Pools (researched 2026-09-06, not yet in catalog)

- **Type:** service
- **Category:** Managed mobile authentication
- **What it is:** AWS's user directory and OAuth/OIDC token issuer for web and
  mobile applications; API Gateway can validate its access tokens.
- **Problem it solves:** Account lifecycle and token issuance without building
  password/session security inside the API.
- **When to use / When NOT to use:** Use when managed auth and AWS-native API
  authorization fit; avoid when portability, fully custom auth UX, or simple
  self-hosted auth is more important than AWS integration.
- **Works with:** iOS OAuth/OIDC clients, API Gateway authorizers, and Lambda.
- **Cost / licensing:** MAU-based with feature-plan and federation differences;
  confirm the current plan and account eligibility against AWS pricing.
- **Maintenance / status:** Official AWS service, stable and actively operated.
- **Alternatives considered:** Better Auth (catalogued and self-owned, but adds
  auth operations and native-client session design); Clerk (managed, non-AWS,
  per-MAU).
- **Docs:** [Cognito overview](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html) · [API Gateway access after sign-in](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-accessing-resources-api-gateway-and-lambda.html) · [pricing](https://aws.amazon.com/cognito/pricing/)
- **Notes for chef:** Keep authorization checks in the application even when the
  gateway validates tokens; define admin/user policy and account deletion.

### Amazon SNS Mobile Push (researched 2026-09-06, not yet in catalog)

- **Type:** service
- **Category:** Mobile push delivery
- **What it is:** An AWS service that maps APNs device tokens to mobile endpoints
  and publishes alerts, badges, or sounds to iOS devices through APNs.
- **Problem it solves:** Server-side device registration and push delivery while
  keeping the backend on AWS.
- **When to use / When NOT to use:** Use when the project wants AWS-managed APNs
  integration; use APNs directly when one-platform simplicity outweighs the SNS
  abstraction, or another push provider is already standard.
- **Works with:** APNs credentials and device tokens supplied by the iOS client;
  Lambda may publish directly or from an asynchronous event.
- **Cost / licensing:** Usage-based AWS service; confirm region and message volume
  in the AWS pricing calculator.
- **Maintenance / status:** Official AWS service, stable.
- **Alternatives considered:** Direct APNs (fewer layers for iOS only but more
  provider-specific backend code); Firebase Cloud Messaging or OneSignal
  (cross-platform services outside the stated AWS preference).
- **Docs:** [AWS mobile push guide](https://docs.aws.amazon.com/sns/latest/dg/sns-mobile-application-as-subscriber.html) · [SNS pricing](https://aws.amazon.com/sns/pricing/)
- **Notes for chef:** Treat tokens as rotating data, remove disabled endpoints,
  avoid sensitive payloads, and make publishing retry-safe.

### @hono/zod-openapi (researched 2026-09-06, not yet in catalog)

- **Type:** package
- **Category:** REST validation and OpenAPI generation
- **What it is:** A Hono extension that uses Zod route schemas for runtime
  validation, TypeScript types, and generated OpenAPI documentation.
- **Problem it solves:** Prevents request validation and API documentation from
  becoming two hand-maintained contracts.
- **When to use / When NOT to use:** Use for a Hono REST API that commits to
  OpenAPI; avoid if the service uses a different schema system or hand-authored
  specifications are an explicit requirement.
- **Works with:** Hono and Zod.
- **Cost / licensing:** Open-source package; no hosted cost.
- **Maintenance / status:** Maintained in Hono's middleware repository and shown
  in Hono's official examples; verify the current release when scaffolding.
- **Alternatives considered:** Hand-written OpenAPI (duplicates schemas) and
  `hono-openapi` with another validator (less aligned with selected Zod).
- **Docs:** [Hono Zod OpenAPI example](https://hono.dev/examples/zod-openapi) · [repository](https://github.com/honojs/middleware/tree/main/packages/zod-openapi)
- **Notes for chef:** Declare required request bodies explicitly and standardize
  validation-error responses before adding routes.

## C. Selection

| Capability | Decision | Ingredient(s) | Why | Also considered | Source |
|---|---|---|---|---|---|
| REST API framework | use | Hono + TypeScript | Small standalone TS API with a documented Lambda adapter | FastAPI; conflicts with team language. Next.js; unnecessary UI/full-stack layer | [catalog](../catalog/frameworks.md) |
| AWS hosting and IaC | use | API Gateway HTTP API + Lambda + TypeScript CDK | Matches AWS constraint and assumed early scale | ECS/Fargate; more fixed operations | researched |
| Relational data | use | PostgreSQL on RDS + Drizzle | Feed, follows, ownership, and deletion fit relational data; Drizzle stays close to SQL | DynamoDB; uncatalogued and not required by the access pattern | [catalog](../catalog/backend-data.md) |
| Accounts | use | Cognito User Pools | Managed mobile tokens and API Gateway authorization within AWS | Better Auth; more auth operations | researched |
| Feed | build | Indexed SQL queries with opaque cursor pagination | Requirements do not justify a search/cache/feed service | Redis or search engine; no concrete need | none |
| Image uploads | use | Amazon S3 presigned uploads | Keeps large files out of API Gateway/Lambda and the database | Upload through API; unnecessary compute and payload limits | [catalog](../catalog/backend-data.md) |
| Push notifications | use | SNS Mobile Push → APNs | AWS-native device endpoints and delivery | Direct APNs; simpler layers but more provider-specific code | researched |
| Validation and API contract | use | Zod + `@hono/zod-openapi` | One route schema validates requests and emits OpenAPI | Hand-written spec; drift risk | [catalog](../catalog/quality.md) + researched |
| Rate limiting | use | API Gateway route throttling | Baseline protection without Redis at assumed scale | Redis limiter; extra service | researched |
| Logging and errors | use | JSON logs in CloudWatch | Lambda already integrates with CloudWatch; no second vendor needed initially | Sentry; useful later if CloudWatch is insufficient | researched / [catalog](../catalog/services.md) |
| Tests and checks | use | Vitest + Biome + strict TypeScript in CI | Fits the team and quality baseline | Playwright; no browser UI exists | [catalog](../catalog/quality.md) |

**Deliberately not included:** No frontend/iOS code, GraphQL, Redis, search
service, ranked-feed system, real-time transport, CMS, custom auth server, social
login, image-transformation pipeline, or second observability vendor.

**Cost roll-up:** All selected AWS services are usage-based. At the simulated
10,000 MAU / one million requests per month, use a provisional **$50–$200/month**
architecture band; RDS topology is likely the main fixed cost. Before approval,
the chef must run the AWS Pricing Calculator with the actual Region, RDS class,
storage/egress, Cognito plan, log retention, and push volume.

## D. Filled recipe

# Recipe: iOS Feed REST API

**Prepared for:** A TypeScript team deploying an iOS backend on AWS  
**Date:** 2026-09-06  
**Assumed technical level:** technical  
**Status:** proposed — awaiting approval

---

## 1. Summary

Build a typed, documented REST API for iOS accounts, followed-user feeds, posts,
direct image uploads, and push notifications. Deploy it as a single-region AWS
serverless service, keeping the first version operationally small while leaving
clear seams for later scale work.

## 2. How it will work

The iOS app authenticates with Cognito and sends its access token to API Gateway.
API Gateway validates the token and invokes a Hono Lambda handler. Hono applies
authorization and Zod validation, then reads or writes PostgreSQL through
Drizzle. Feed queries use indexed follow/post tables and opaque cursors.

For images, the API creates a short-lived S3 upload URL; the app uploads directly
and then attaches the validated object key to a post. After a post is created,
the API makes an idempotent publish request and SNS handles best-effort delivery
to the followers' current APNs endpoints. Lambda emits structured logs to
CloudWatch.

## 3. The stack

- **Framework:** Hono + strict TypeScript — compact REST service with AWS Lambda
  support — free/open-source.
- **Hosting/IaC:** API Gateway HTTP API + Lambda + TypeScript CDK **[researched —
  not yet in catalog]** — AWS-native, usage-based deployment.
- **Database:** PostgreSQL on RDS + Drizzle — relational feed/ownership model and
  typed SQL — RDS usage-based; libraries free.
- **Authentication:** Cognito User Pools **[researched — not yet in catalog]** —
  managed mobile identity and JWTs — MAU-based.
- **Object storage:** Amazon S3 — presigned direct uploads — storage, request,
  and egress charges.
- **Push:** SNS Mobile Push to APNs **[researched — not yet in catalog]** — AWS-
  managed endpoint delivery — usage-based.
- **Validation/docs:** Zod + `@hono/zod-openapi` **[package researched — not yet
  in catalog]** — shared validation, types, and OpenAPI — free/open-source.
- **Operations:** API Gateway throttling + CloudWatch JSON logs — baseline
  protection and diagnosis — usage-based.
- **Tooling:** Vitest, Biome, strict TypeScript, and CI — automated tests and
  checks — free/open-source apart from the chosen CI allowance.

## 4. Key decisions & trade-offs

- **Serverless AWS API:** Choose Lambda/API Gateway over ECS/Fargate for the
  assumed early load. This minimizes server operations but requires deliberate
  database connection management and is less suitable for long-running work.
- **Managed AWS auth:** Choose Cognito over Better Auth. This reduces password and
  token operations but increases AWS coupling and requires plan/MAU cost review.
- **Relational feed:** Choose PostgreSQL/Drizzle over DynamoDB because follows,
  ownership, pagination, and deletion are naturally relational and the catalog
  already covers the choice.
- **Direct uploads:** Use presigned S3 URLs rather than sending image bytes through
  the REST API; the API still validates ownership, key, size, and content type
  before publishing.

## 5. Included by default

- Strict TypeScript, pinned versions, lint/type-check/test/build scripts, and CI
- Zod boundary validation, generated OpenAPI, consistent JSON errors, and a
  health endpoint
- JWT verification plus route-level ownership/admin authorization
- API Gateway throttling, request-size limits, least-privilege IAM, and secure
  environment/secret handling with no credentials committed
- Cursor pagination and database indexes verified against feed queries
- Private S3 bucket, short-lived presigned URLs, content-type/size checks, and
  cleanup of abandoned uploads
- Structured CloudWatch logs with request correlation and bounded retention;
  retry-safe push publishing and stale-device cleanup
- Database backups, migration documentation, and account/content deletion tests
- No UI accessibility/reduced-motion work because this project is a headless API

## 6. Deferred / out of scope for v1

- iOS/UI implementation, social login, comments, likes, ranking, search,
  moderation/reporting workflows, live feed updates, and multi-region failover
- Image resizing, transcoding, malware scanning, and content moderation pending
  product requirements and risk review
- Redis/cache/queue infrastructure until measured load or delivery guarantees
  justify it

## 7. Rough cost

| Item | Free tier covers | Approx. cost at 10k MAU / 1m API calls monthly |
|---|---|---|
| Lambda + API Gateway | May cover much of initial use for an eligible new account | $0–$20, workload/account dependent |
| RDS PostgreSQL and connection layer | New-account eligibility varies | Roughly $40–$150, driven by Region and availability choice |
| Cognito | Depends on feature plan and account eligibility | $0–$20 provisional; calculate from active users and plan |
| S3 + SNS + CloudWatch | Small usage may be low cost | $1–$30, driven by image egress, push volume, and log retention |
| **Provisional total** | — | **$50–$200/month; calculator required before approval** |

## 8. Build outline

1. Scaffold the Hono/CDK workspace, CI, environment schema, and baseline checks.
2. Provision API Gateway/Lambda, RDS connectivity/migrations, Cognito, S3, SNS,
   IAM, throttles, and logs.
3. Implement account/profile, follow, post, feed, and deletion endpoints from
   shared Zod/OpenAPI schemas.
4. Add presigned upload registration/finalization and APNs device lifecycle plus
   retry-safe publishing.
5. Run unit/integration/deployed smoke tests, verify cost/security assumptions,
   deploy, and write project documentation.

## 9. Assumptions & open questions

- The selected Region supports all services and is close enough to the initial
  users; exact Region still needs confirmation.
- Validate Lambda-to-RDS networking, connection limits, and whether RDS Proxy is
  required before locking the cost estimate.
- The team will supply APNs credentials and implement device-token refresh in the
  iOS client.
- Confirm backup retention, account-deletion timing, image moderation/security
  requirements, and acceptable push delay before implementation.

## 10. Approval

Reply **approved** to create the project, or tell me what to change. Nothing is
created until you approve.
