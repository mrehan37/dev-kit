# Frontend

UI component libraries, styling, animation, forms, client-side state, and icons.
Most projects pick: one styling approach, one component layer, one data/state
library, and add animation only where it earns its place.

---

## Components

### shadcn/ui
- **Type:** template (component registry) — not a dependency
- **Category:** React component layer
- **What it is:** A CLI that copies accessible, unstyled-then-Tailwind-styled component source (built on Radix primitives) directly into your project. You own the code; there is no runtime package to upgrade.
- **Problem it solves:** A good-looking, accessible component set you can fully customize, without being locked to a library's theming system.
- **When to use:** React + Tailwind projects that want control over component code and design tokens; the common default for new React apps.
- **When NOT to use:** Non-React projects; teams that want to `npm update` components rather than maintain copied source; non-Tailwind styling strategies.
- **Works with:** React (Next.js, React Router, Vite), Tailwind, Radix UI. Supports namespaced registries so teams can host their own component sources.
- **Cost / licensing:** Free, open-source (MIT).
- **Maintenance / status:** Stable, actively maintained, very large ecosystem of third-party registries.
- **Alternatives:** MUI (Material Design, comprehensive, heavier), Mantine (batteries-included, own styling), Chakra UI (prop-based styling), Park UI (shadcn-style for multiple frameworks), Radix Themes.
- **Docs:** https://ui.shadcn.com
- **Notes for chef:** Strong default for React UI. Treat added components as project source code (they get committed and maintained), not as a locked dependency.

### Radix UI (primitives)
- **Type:** library
- **Category:** Headless / unstyled React components
- **What it is:** Accessible, unstyled component primitives (dialog, popover, dropdown, tabs, etc.) that handle focus, keyboard, and ARIA correctly.
- **Problem it solves:** Getting hard-to-get-right interactive components accessible by default, while you supply all styling.
- **When to use:** Custom design systems; when shadcn/ui's copied components need to be extended or built from scratch.
- **When NOT to use:** When shadcn/ui already covers the need (it wraps these); non-React.
- **Works with:** React, any styling solution.
- **Cost / licensing:** Free, open-source (MIT).
- **Maintenance / status:** Stable, actively maintained.
- **Alternatives:** React Aria Components (Adobe), Headless UI (Tailwind Labs, smaller scope), Ark UI (multi-framework).
- **Docs:** https://www.radix-ui.com/primitives
- **Notes for chef:** Usually arrives transitively via shadcn/ui. Name it explicitly only when building bespoke components.

## Styling

### Tailwind CSS
- **Type:** library
- **Category:** Utility-first CSS
- **What it is:** A CSS framework where you compose styles from small utility classes in markup. v4+ configures the design system in CSS (`@theme`) and uses a fast Rust engine.
- **Problem it solves:** Consistent styling with design tokens, no naming bikeshedding, no separate CSS files drifting from components, small production CSS.
- **When to use:** The default styling approach for most web projects, especially with shadcn/ui.
- **When NOT to use:** Teams that strongly prefer semantic CSS/SCSS; environments where utility-class markup is unacceptable to reviewers.
- **Works with:** Every framework in `frameworks.md`; shadcn/ui; Nativewind for React Native.
- **Cost / licensing:** Free, open-source (MIT).
- **Maintenance / status:** Stable, actively maintained. v4 changed config from JS to CSS — check which major a template targets.
- **Alternatives:** CSS Modules (scoped plain CSS, framework-native), vanilla-extract (typed CSS-in-TS, zero runtime), Panda CSS (build-time, design-token focused), plain modern CSS.
- **Docs:** https://tailwindcss.com/docs
- **Notes for chef:** Default. If the project inherits a non-Tailwind codebase, match what is there rather than introducing a second system.

## Animation

### Motion (formerly Framer Motion)
- **Type:** library
- **Category:** Animation (React; also vanilla JS / Vue)
- **What it is:** The library previously called Framer Motion. Now an independent project; package name `motion`, React import `motion/react`. Declarative animations, gestures, layout transitions, scroll-linked effects.
- **Problem it solves:** Production-quality UI animation and transitions without hand-writing keyframes and interruption logic.
- **When to use:** React apps that need real motion design — page transitions, shared-layout animation, micro-interactions, `AnimatePresence` exit animations.
- **When NOT to use:** When a CSS transition or `@keyframes` already does the job; APIs and content sites with no interactive motion needs; when bundle size is critical and animation is incidental (use CSS or AutoAnimate).
- **Works with:** React (and framework wrappers), any React framework.
- **Cost / licensing:** Free, open-source (MIT). Optional paid "Motion+" extras exist but are not required.
- **Maintenance / status:** Stable, actively maintained (maintainer: Matt Perry).
- **Alternatives:** GSAP (imperative, timeline-based, framework-agnostic, best for complex sequenced/scroll animation), AutoAnimate (one-line list/layout transitions), CSS transitions (no dependency), React Spring.
- **Docs:** https://motion.dev/docs
- **Notes for chef:** If you see `framer-motion` in older code/tutorials, it is the same library — use the `motion` package for new work. Respect `prefers-reduced-motion` (see `../standards/quality-baseline.md`); do not add entrance animations just because this is installed.

### GSAP
- **Type:** library
- **Category:** Animation (framework-agnostic)
- **What it is:** A mature, high-performance JavaScript animation engine with a timeline model and plugins (ScrollTrigger, SplitText, etc.).
- **Problem it solves:** Complex, precisely sequenced, scroll-driven, or SVG animation that declarative React libraries handle awkwardly.
- **When to use:** Marketing/interactive sites with elaborate motion; scroll-storytelling; SVG/canvas animation; non-React contexts.
- **When NOT to use:** Simple component state transitions in React (Motion is more idiomatic); when no complex sequencing is needed.
- **Works with:** Any framework or vanilla JS.
- **Cost / licensing:** Free, open-source (now fully free including formerly-paid plugins). Check the current license note on the site.
- **Maintenance / status:** Stable, long-established, actively maintained.
- **Alternatives:** Motion (declarative React), Anime.js, native Web Animations API.
- **Docs:** https://gsap.com/docs
- **Notes for chef:** Reach for GSAP when the animation brief is genuinely complex or scroll-heavy. For typical app UI, Motion or CSS is the lighter choice.

## Forms & client state

### React Hook Form
- **Type:** library
- **Category:** Forms (React)
- **What it is:** A performant form state library using uncontrolled inputs and refs, with a resolver system for schema validation.
- **Problem it solves:** Form state, validation wiring, and re-render minimization without hand-rolling it.
- **When to use:** Any non-trivial React form.
- **When NOT to use:** A single input or two where local state is simpler; frameworks with their own form actions where that is sufficient.
- **Works with:** React; Zod/Valibot/Yup via `@hookform/resolvers`; shadcn/ui form components.
- **Cost / licensing:** Free, open-source (MIT).
- **Maintenance / status:** Stable, actively maintained.
- **Alternatives:** TanStack Form (newer, more type-driven, framework-agnostic), Formik (older, heavier), native form actions.
- **Docs:** https://react-hook-form.com
- **Notes for chef:** Pair with a Zod schema so the same schema validates client and server.

### TanStack Query
- **Type:** library
- **Category:** Server-state / data fetching (React; also Vue, Svelte, Solid)
- **What it is:** Async server-state management: caching, background refetching, deduplication, pagination, mutations.
- **Problem it solves:** The "load, cache, invalidate, refetch, handle loading/error" cycle that people otherwise reinvent badly with `useEffect`.
- **When to use:** Client-side data fetching against REST/GraphQL/RPC APIs; anything with cache invalidation needs.
- **When NOT to use:** Apps that fetch entirely through a full-stack framework's server loaders/RSC and have little client fetching; trivial one-shot fetches.
- **Works with:** Any framework; any async function; pairs with fetch/axios/tRPC.
- **Cost / licensing:** Free, open-source (MIT).
- **Maintenance / status:** Stable, actively maintained.
- **Alternatives:** SWR (lighter, simpler, Vercel), RTK Query (if already using Redux Toolkit), framework-native loaders.
- **Docs:** https://tanstack.com/query
- **Notes for chef:** Use for client state that mirrors the server. Do not use it as a general global-state store — that is Zustand's job.

### Zustand
- **Type:** library
- **Category:** Client state (React)
- **What it is:** A small, unopinionated global state store with a hook-based API and no boilerplate.
- **Problem it solves:** Sharing genuine UI/client state across components without Context re-render pain or Redux ceremony.
- **When to use:** Cross-cutting client state (theme, sidebar, wizard progress, cart before checkout, ephemeral selections).
- **When NOT to use:** Server data (use TanStack Query); state that only one subtree needs (use local state / Context).
- **Works with:** React; works outside components too.
- **Cost / licensing:** Free, open-source (MIT).
- **Maintenance / status:** Stable, actively maintained.
- **Alternatives:** Redux Toolkit (large apps, strict conventions, devtools), Jotai (atom-based), Valtio (proxy-based), React Context (no dependency, small scope).
- **Docs:** https://zustand.docs.pmnd.rs
- **Notes for chef:** Default lightweight global store. Most apps need far less global state than they think — check whether local state suffices first.

## Icons

### Lucide
- **Type:** library
- **Category:** Icon set
- **What it is:** A large, consistent open-source icon set with per-framework packages (`lucide-react`, etc.), tree-shakeable.
- **Problem it solves:** A complete, visually consistent icon library with a permissive license.
- **When to use:** Almost any project needing UI icons; it is shadcn/ui's default.
- **When NOT to use:** When a brand system mandates a specific icon set; when you need filled/duotone weights (consider Phosphor).
- **Works with:** React, Vue, Svelte, Solid, plain SVG.
- **Cost / licensing:** Free, open-source (ISC).
- **Maintenance / status:** Stable, actively maintained (fork lineage from Feather).
- **Alternatives:** Phosphor Icons (multiple weights), Heroicons (Tailwind Labs, smaller set), Tabler Icons, Iconify (aggregator of many sets).
- **Docs:** https://lucide.dev
- **Notes for chef:** Default icon set. Import icons individually so the bundle only carries what is used.
