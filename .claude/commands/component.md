Create a UI component and its Storybook story file. Use plain English throughout — no framework jargon.

First, read the file at `.claude/agents/builder.md` and follow all instructions in it for this task.

If `$ARGUMENTS` is provided, treat it as the description of the component. Otherwise, ask the questions below.

## Step 1: Gather information

Ask the user three things (skip any already answered via `$ARGUMENTS`):

### Question 1 — What kind of component is this?

This decides which folder it lives in. Pick one of three levels — these come from a design system idea called **atomic design**.

- **Atom** — the smallest possible piece. Examples: a single coloured status badge, a custom toggle switch, an icon button. If shadcn/ui already provides what you need (Button, Input, Label, Checkbox, etc.), you don't need a custom atom — those primitives already live in `components/ui/`.
- **Molecule** — a small group of pieces working together. Examples: a labelled form field with helper text and an error message, a search bar made of an input and a button, a row showing a name + a status pill.
- **Organism** — a larger section. Examples: a complete card with header / body / actions, a full form, a sidebar, a navigation panel, a dashboard tile.

Rule of thumb: *"What does it contain?"* — if it's just one thing, it's an atom; if it's a few atoms grouped, it's a molecule; if it's a self-contained section of a page, it's an organism.

### Question 2 — Which feature area is this for?

This only affects how the component is grouped in Storybook — the file location is decided by Question 1, not by this. Read the current feature areas from `CLAUDE.md` (the "Current feature slices" table) and present them in plain English. Always offer two extra options:

- **Shared** (used across multiple areas of the system)
- **Something new** — a feature area that doesn't fit the list

If the user picks "Something new", ask: *"What's the new area called, in a short phrase? (For example: 'Billing', 'Onboarding', 'Reporting'.)"* Use their answer as the PascalCase area name in the Storybook title.

Then gently surface the bigger picture in plain English: *"Heads up — if this is the first piece of a brand-new area of the system, the project's setup file (`CLAUDE.md`) lists the official areas. We can add the new one there so it's properly recognised, and so the project's safety checks know about it. Would you like me to do that now, or carry on with just the component for now and we can add the new area later?"*

If they say carry on, just create the component. If they say yes, after the component is built note the new area as a follow-up (don't make config edits silently).

### Question 3 — What does this component display or do?

Ask for a short description of what appears on screen and any interactions.

## Step 2: Derive the component name

From the user's description, derive a PascalCase component name (e.g. "a card showing order details" becomes `OrderDetailCard`). Tell the user the proposed name and ask them to confirm or suggest a different one.

## Step 3: Create the files

The folder is decided by the atomic level from Question 1:

| Atomic level (Q1) | Folder |
|---|---|
| Atom | `components/atoms/` |
| Molecule | `components/molecules/` |
| Organism | `components/organisms/` |

Create:
- `components/<atomic-level-folder>/<ComponentName>.tsx`
- `components/<atomic-level-folder>/<ComponentName>.stories.tsx`

> Existing components written under the older convention may still sit at the top level of `components/` (i.e. directly inside `components/` rather than in a subfolder). That's tolerated for now but new components must always go in an atomic subfolder.

If the component would benefit from a shadcn/ui primitive that is not yet installed, install it with `npx shadcn@latest add <primitive>`.

The component must:
- Compose from shadcn/ui primitives (`components/ui/`) where applicable
- Use Tailwind utility classes only — no arbitrary values, no inline styles
- Use the `cn()` utility from `lib/utils` for conditional class merging
- Accept props with a typed interface
- Export as the default or named export (match existing patterns in the codebase)

The story file must:
- Use Storybook 10 ESM format with `satisfies Meta<typeof ComponentName>`
- Import `Meta`/`StoryObj` from `@storybook/nextjs-vite` (never `@storybook/react`) — Storybook 10 forbids importing renderer packages directly (lint rule `storybook/no-renderer-packages`)
- Include a `Default` story and one story per significant visual variant
- Set the `title` from the atomic level (Q1) plus the feature area (Q2):
  - **Shared** components: `<AtomicLevel>s/<ComponentName>` — e.g. `Atoms/StatusBadge`, `Molecules/SearchBar`, `Organisms/OrderProgressCard`.
  - **Area-specific** components: `<AtomicLevel>s/<FeatureArea>/<ComponentName>` — e.g. `Atoms/Billing/InvoiceBadge`, `Organisms/Onboarding/SignupWizard`.
  - Use the plural of the atomic level as the top group: `Atoms`, `Molecules`, `Organisms`.

If a spec file was referenced, use its "Components needed" section as guidance for what to build.

## Step 4: Summarise what was created

Tell the user in plain English:
- What files were created and where they live (use friendly descriptions — say "a new molecule in the billing section" rather than the raw path)
- What the component does in one sentence
- How to see it: "Run `/preview storybook` to view it in Storybook"

## Step 5: Undo guidance

End with: "**To undo:** if this is not what you wanted, the following files were created and can be safely deleted:" then list the full file paths of every file that was created or modified. This gives the user a clear rollback path.
