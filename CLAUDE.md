# Project — Workshop Starter

> **This file is the single source of truth for the project's stack, structure, and conventions.** Sections marked with `<YOUR DOMAIN>` or other angle-bracket placeholders are filled in by `/init-framework`. The rest is the workshop's standard setup — Next.js + Tailwind v4 + shadcn/ui + Storybook 10 + Vercel.

This is a **proof-of-concept** repository. Favour clarity and speed over premature optimisation. When in doubt, keep it simple.

---

## 1. Product Purpose and Domain Context

> **Run `/init-framework` to fill this section in.** Until then, what follows is a placeholder showing the structure.

`<YOUR DOMAIN>` is `<one-sentence description of the product, who it serves, and what it does>`.

### End-to-end workflow

1. `<Step 1 — who initiates and what happens>`
2. `<Step 2 — what comes next>`
3. `<Step 3 — …>`
4. `<Final step — the outcome that matters>`

### Domain glossary

| Term | Meaning |
|---|---|
| **`<Term 1>`** | `<Plain-English meaning>` |
| **`<Term 2>`** | `<Plain-English meaning>` |
| **`<Term 3>`** | `<Plain-English meaning>` |

---

## 2. Tech Stack and Tooling

| Layer | Choice | Notes |
|---|---|---|
| Framework | **Next.js** (App Router) | Pages Router is not used in this project |
| Language | **TypeScript** (strict mode) | `tsconfig.json` has `"strict": true` — do not relax this |
| Styling | **Tailwind CSS v4** | Utility classes only; design tokens defined via `@theme` blocks in `app/globals.css` (no `tailwind.config.ts` — v4 uses CSS-based config) |
| Components | **shadcn/ui** | Pre-built accessible primitives; installed into `components/ui/` |
| Design docs | **Storybook 10** | Runs alongside the app; every shared component needs a story. ESM-only |
| Deployment | **Vercel** | Preview deploys per branch, production from `main` |
| Validation | **Zod** | Schema validation co-located with Server Actions |
| Package manager | **npm** | Use `npm` — not pnpm or yarn |

### Mock data strategy

There is no real backend in this PoC. Server Actions return mock data from **typed fixture factories** in `lib/mocks/`.

```
lib/mocks/
├── <entity-1>.ts   # Factory: createMock<Entity1>(), mock<Entity1>List()
├── <entity-2>.ts   # Factory: createMock<Entity2>(), …
└── index.ts        # Re-exports all factories
```

Rules:
- Every factory returns **realistic data for your domain** — real-looking names, identifiers, plausible dates — not "Test User 1".
- Factories are typed against the domain interfaces in `types/`.
- Server Actions call these factories directly. When real APIs arrive, swap the factory call inside the action for a real `fetch` — the action signature stays the same.
- **No MSW, no mock service workers, no HTTP interception.** The mock boundary is the Server Action, not the network layer.

### Integrations

> Filled in by `/init-framework` if the project talks to external systems (payment providers, CRMs, address lookup, analytics, etc.). Otherwise leave blank.

### Deliberately absent

- No Redux or Zustand — use React Server Components + Server Actions + `useState` where necessary
- No CSS Modules or styled-components — Tailwind only
- No class-based React components
- No custom Webpack config — use Next.js defaults
- No ORM in this PoC — raw fetch to external APIs or mock data
- No MSW or network-level mocking — mock data lives in typed factories called directly by Server Actions

---

## 3. Repository and Feature Slice Architecture

### Directory layout

There is no `src/` directory — the project uses root-level directories. The `@/*` path alias in `tsconfig.json` maps to `./*`.

```
<project>/
├── app/                            # Next.js App Router (routing only — no business logic here)
│   ├── layout.tsx
│   ├── page.tsx
│   ├── globals.css                 # Tailwind v4 config via @theme blocks + global styles
│   └── (features)/                 # Route groups that map to feature slices
│       ├── <feature-slice-1>/
│       ├── <feature-slice-2>/
│       └── <feature-slice-3>/
├── features/                       # All feature logic lives here
│   ├── <feature-slice-1>/
│   ├── <feature-slice-2>/
│   └── <feature-slice-3>/
├── components/                     # ALL reusable UI components (atoms, molecules, organisms) + Storybook stories
│   └── ui/                         # shadcn/ui primitives (auto-generated)
├── lib/                            # Shared utilities, hooks, and API clients
├── types/                          # Shared TypeScript types and domain interfaces
├── .storybook/
├── public/
└── CLAUDE.md
```

### Feature slice internal structure

A slice under `features/<slice>/` may contain any of these subfolders **as it needs them** — don't create empty ones to satisfy a template:

```
features/<slice>/
├── actions/          # Next.js Server Actions (co-located with Zod schemas)
├── hooks/            # Client-side React hooks
├── api/              # Route handlers (if the slice has its own API routes)
├── utils/            # Pure functions — no side effects, no React
└── types.ts          # TypeScript interfaces for this slice's domain objects
```

In practice most slices start with only `actions/` and grow the rest as needed. The one rule that is **not** optional: there is no `components/` folder inside a feature slice. All UI components — regardless of which slice they were built for — live in the top-level `components/` directory and have a co-located Storybook story.

### A/B variant routing

When a flow needs variant experimentation (e.g. sign-up, onboarding), structure variants as **sibling route segments** under the feature slice:

```
app/(features)/<slice>/signup/
├── page.tsx              # Default variant (or redirect logic)
├── variant-a/
│   └── page.tsx
└── variant-b/
    └── page.tsx
```

For now, variants are toggled manually via URL or a simple query param (`?variant=b`). When Vercel Edge Config + Flags SDK is wired up later, the middleware will resolve the variant and rewrite to the correct segment — the UI components won't need to change.

### Cross-slice rules

- **Slices may not import from each other.** If two slices need the same thing, promote it to `lib/` (logic/hooks) or `components/` (UI).
- The `app/` directory is routing only — it should import from `features/`, never contain business logic directly.
- `lib/` is for utilities that are genuinely cross-cutting (e.g. API client wrappers, date helpers, auth utilities).
- `types/` is for domain interfaces shared across more than one slice.

### Current feature slices

> Filled in by `/init-framework`. Until then, the rows below are placeholders — replace them with your actual slices.

| Slice | Route | What it covers |
|---|---|---|
| `<feature-slice-1>` | `/<feature-slice-1>` | `<one-line description>` |
| `<feature-slice-2>` | `/<feature-slice-2>` | `<one-line description>` |
| `<feature-slice-3>` | `/<feature-slice-3>` | `<one-line description>` |

When ESLint enforces cross-slice imports it reads from this list — `eslint.config.mjs` mirrors it.

---

## 4. TypeScript Conventions

**Strict mode is always on.** Never add `// @ts-ignore` or `// @ts-nocheck` without a comment explaining why and a TODO to remove it.

### Types and interfaces

- Use `interface` for domain objects that may be extended; use `type` for unions, intersections, and mapped types.
- Domain entities use PascalCase nouns (e.g. `Order`, `Customer`, `Subscription` — your specific names come from `agents/guide.md`).
- API request/response shapes are suffixed: `CreateOrderRequest`, `GetCustomerResponse`.
- Prefer **discriminated unions** over optional fields for state modelling:

```typescript
// Preferred
type OrderStatus =
  | { stage: 'pending' }
  | { stage: 'paid'; paidAt: Date }
  | { stage: 'shipped'; trackingNumber: string }
  | { stage: 'delivered'; deliveredAt: Date };

// Avoid
type OrderStatus = {
  stage: string;
  paidAt?: Date;
  trackingNumber?: string;
  deliveredAt?: Date;
};
```

### `any` is banned

Use `unknown` when the type is genuinely unknown, then narrow it:

```typescript
// Correct
} catch (error: unknown) {
  if (error instanceof Error) { ... }
}
```

### JSX text — escape special entities

ESLint enforces `react/no-unescaped-entities`. Literal `"`, `'`, `>`, `}` inside JSX text will fail lint. Either:

- escape: `&quot;`, `&apos;`, `&gt;`, `&#125;`
- or wrap in a JSX expression: `{'"'}`, `` {`"${value}"`} ``

Apply this to dynamic content too — e.g. `<span>"{value}"</span>` must become `` <span>{`"${value}"`}</span> ``.

### Server Actions

Every Server Action must have a co-located Zod schema for its input:

```typescript
// actions/create-order.ts
const CreateOrderSchema = z.object({
  customerId: z.string().uuid(),
  productId: z.string().min(1),
  quantity: z.number().int().positive(),
});

export async function createOrder(input: unknown) {
  const data = CreateOrderSchema.parse(input);
  // ...
}
```

---

## 5. Design System and Component Rules

### Component hierarchy

This project organises UI components by **atomic design** level. Every component lives in one of these subfolders under `components/`:

```
components/
├── ui/              # shadcn/ui primitives — auto-generated, do not rewrite by hand
├── atoms/           # custom atoms — smallest pieces (a badge, a status pill, a single icon button)
├── molecules/       # small groups of atoms working together (a form field with label + error, a search bar with button)
└── organisms/       # larger sections (a full card, a complete form, a navigation panel)
```

Rules:
1. **shadcn/ui primitives** (`components/ui/`) — installed via `npx shadcn add`. Never rewrite these from scratch. Customise via Tailwind classes or the `cn()` utility. Treat these as your atom library; only add files under `components/atoms/` for genuinely custom atoms that shadcn does not provide.
2. **Every component file** under `atoms/`, `molecules/`, or `organisms/` **must** have a co-located `.stories.tsx`. A component without a story is incomplete.
3. **Pick the level by what the component contains, not by where it is used.** A "billing dashboard card" composed of a header, a value, and a list is an organism even though it is only used in one place. A "yes/no toggle with helper text" is a molecule even if only one form uses it.
4. **There is no slice-local component layer.** Components live in atomic subfolders regardless of which feature area they serve. Use the Storybook `title` to group by feature area for browsing.
5. **Page views** (`app/(features)/<slice>/page.tsx`, plus their `*-client.tsx` siblings) are the only UI artifacts that do not belong in `components/`.

### Styling rules

- **Tailwind utility classes only.** No raw CSS files outside of `app/globals.css`.
- **Prefer design tokens over arbitrary values.** Arbitrary values (`text-[11px]`, `tracking-[0.14em]`) are a smell — when a non-token value appears in **two or more places**, promote it to `@theme` in `app/globals.css`. One-off layout arbitraries (e.g. a single `lg:grid-cols-[1fr_320px]`) are tolerated.
- **Inline `style={...}` is allowed only for runtime-computed values that bind to layout** — percentage widths from data (`style={{ width: \`${pct}%\` }}`), background colours from data. Static styling stays in Tailwind utilities. Story decorators are exempt — they're scaffolding, not product UI.
- Use design tokens defined in the `@theme` block in `app/globals.css` for all colours, spacing, and typography. If a value you need isn't in the token set, add it to the `@theme` block — don't use an arbitrary value.
- Use the `cn()` utility (from `lib/utils.ts`) to conditionally merge classes.

### Storybook

Every component in `components/` (except `components/ui/`) **must** have a co-located `.stories.tsx` file. This is a hard requirement — a component without a story is incomplete.

Minimum story requirements:

```typescript
// StatusBadge.stories.tsx
import type { Meta, StoryObj } from '@storybook/nextjs-vite';

export default {
  title: 'Atoms/StatusBadge',
  component: StatusBadge,
} satisfies Meta<typeof StatusBadge>;

export const Default: Story = { args: { status: 'active' } };
export const Inactive: Story = { args: { status: 'inactive' } };
// One story per significant visual variant
```

**Import `Meta`/`StoryObj` from `@storybook/nextjs-vite`, not `@storybook/react`.** Storybook 10 forbids importing renderer packages directly (lint rule `storybook/no-renderer-packages`); the framework package is `@storybook/nextjs-vite` for this project.

Storybook runs at `localhost:6006`. Run it with `npm run storybook`.

The `.storybook/main.ts` stories glob covers `components/` only — all component stories live there:

```typescript
stories: [
  "../components/**/*.stories.@(js|jsx|mjs|ts|tsx)",
]
```

---

## 6. Vercel Deployment Workflow

### Branch naming

```
feature/<slice>/<short-description>
design-system/<short-description>
fix/<short-description>

# Examples:
feature/<slice>/<feature-name>
design-system/status-badge-component
fix/<slice>/loading-state
```

### Preview deployments

Every push to a non-`main` branch automatically creates a Vercel preview deployment. The URL is stable per branch:

`https://<your-vercel-project-slug>-<branch-slug>.vercel.app`

> `<your-vercel-project-slug>` is set by `/init-framework`. Until then, the placeholder is left in.

Share this URL in PR descriptions for design and stakeholder review — no staging environment needed.

### Environment variables

- Set in the **Vercel dashboard** under Project → Settings → Environment Variables.
- `NEXT_PUBLIC_*` variables are exposed to the browser; all others are server-only.
- Never commit `.env` files with real values. `.env.example` documents required variables with placeholder values.
- Required variables must be documented in `.env.example` before a PR is merged.

### Feature flags and A/B testing

The planned stack for feature flags and A/B testing is **Vercel Edge Config + Flags SDK**. This is not yet implemented — it is on the roadmap.

When implemented, the pattern will be:
1. Edge Middleware reads flags from Vercel Edge Config
2. Middleware rewrites the request to the appropriate variant route segment (see "A/B variant routing" in section 3)
3. UI components remain unaware of the flag resolution — they just render their variant

Until then, variant selection is manual (URL-based). Do not install third-party feature flag SDKs (LaunchDarkly, Statsig, etc.) without explicit approval.

### Production

`main` branch deploys to production automatically on merge. There is no manual promotion step. Merging to `main` = shipping.

---

## 7. Working Conventions and PR Standards

### Commit messages — Conventional Commits

```
feat(<slice>): add order list with pagination
fix(<slice>): resolve status not updating
chore(design-system): add StatusBadge component and stories
```

Types: `feat`, `fix`, `chore`, `refactor`, `test`, `docs`. The scope in parentheses should be the slice name or `design-system`.

### PR standards

- **Target size: ≤ 400 lines of diff.** For larger slice work, use stacked PRs.
- PR title follows the same Conventional Commits format.
- Any PR that adds or modifies a component in `components/` must include a **Storybook screenshot** in the PR description.
- PRs must not introduce `eslint-disable` comments without a justification comment.

### CI checks (all must pass before merge)

- `npm run lint` — ESLint with the project's shared config
- `npm run typecheck` — `tsc --noEmit`
- `npm run build` — Next.js production build must succeed

### CLAUDE.md as a contract

This file is versioned alongside the code. Changes to it require a PR review. Do not add slice-specific rules here — those belong in a slice-level `CLAUDE.md` if needed.

### What Claude should never do autonomously

- Modify `@theme` token values in `app/globals.css` (colour palette, spacing scale)
- Install new npm packages without flagging them in the PR description
- Add new environment variables without documenting them in `.env.example`
- Create files outside the established directory structure without explanation
