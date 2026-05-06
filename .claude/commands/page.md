First, read the file at `.claude/agents/builder.md` and follow all instructions in it for this task.

You are helping a non-technical user create a new page in the project. Use plain English throughout — no framework jargon, no technical terms.

If the user provided arguments, treat them as a reference to a spec file: first try `docs/specs/$ARGUMENTS.md` (the current location), then fall back to `docs/briefs/$ARGUMENTS-spec.md` (the legacy location) so older specs still resolve. Use whichever exists as the source of truth for what to build. If neither exists, treat the arguments as a plain-English description of what the page should show.

## Step 1: Gather requirements

If a spec file was loaded, extract the feature area, page description, data shown, and interactions from it. Confirm your understanding with the user before building.

If no spec was provided, ask the user these questions one at a time:

1. **Which area of the system is this page for?** Read the current feature areas from `CLAUDE.md` (the "Current feature slices" table) and present them in plain English. Always include a final option:
   - **Something new** — a new area of the system

   If they pick **Something new**, ask: *"What's the new area called, in a short phrase? (e.g. 'Billing', 'Onboarding', 'Reporting'.)"* Slugify their answer for the slice folder name (e.g. "Billing" → `billing`). Then surface the bigger picture: *"Heads up — adding a brand-new area means a new feature slice (`features/<slug>/`), a new route group (`app/(features)/<slug>/`), and an update to the project's setup file (`CLAUDE.md`) and the cross-slice ESLint rule. Want me to set that up properly now, or just create the page in the new area and we'll formalise the rest later?"* If they say carry on, create the page; note the new slice as a follow-up so it can be added to CLAUDE.md and the ESLint config.
2. **What does the user see on this page?** Ask them to describe the layout and content.
3. **What can the user do on this page?** Ask about buttons, forms, filters, or navigation.
4. **What data is shown?** Ask which domain entities appear — use the project's vocabulary (see `CLAUDE.md` and `agents/guide.md`).

## Step 2: Build the page

Using the gathered requirements, create all the necessary files:

1. **Route file** in `app/(features)/<slice>/` — this file only imports and renders from the features folder and the components folder. It must not contain any business logic itself. Create intermediate directories if they do not exist. A `*-client.tsx` sibling next to a route is allowed when client-side state is needed.
2. **Reusable UI components** in the top-level `components/` folder, each with a co-located `.stories.tsx`. There is **no** `features/<slice>/components/` folder in this project — every component (regardless of which feature area it serves) lives in `components/`. Use the `/component` command (or follow `agents/builder.md`) to create them. Compose from shadcn/ui primitives where applicable.
3. **Server action** in `features/<slice>/actions/` — with a co-located Zod validation schema. The action calls mock data factories and returns the data the page needs.
4. **Mock data** in `lib/mocks/` — create or update factory functions so the page has realistic data to display. Re-export any new factories from `lib/mocks/index.ts`. Type factories against the domain interfaces in `types/`.

If any domain types needed for this page are missing from `types/`, create them there following the project conventions (PascalCase names, discriminated unions over optional fields).

## Copy

For every piece of UI text in this page — headings, buttons, labels, form hints, errors, empty states, tooltips, confirmations — follow the `ux-copy` skill. Pass it:
- Which feature area (read from `CLAUDE.md`)
- The screen context and user state
- Any character or layout constraints from the design

Do not write UI text freehand. If there's a non-trivial copy decision (tone, multiple options, sensitive moment), invoke `/ux-copy` explicitly with the context.

## Step 3: Summarise what was created

List every file you created and explain what each one does in one sentence. Then tell the user how to see the result — give them the URL path (e.g. "Open localhost:3000/<slice> to see your new page").

## Step 4: Undo guidance

End with this message:

"**To undo:** if this is not what you wanted, I can remove these files. Here is everything I created:"

Then list every file path created, so they can be cleanly deleted if needed.
