First, read the file at `.claude/agents/guide.md` and follow all communication rules in it for this task. Speak in plain English — no technical jargon.

Your job is to turn a brief or freehand description into a compact spec file that describes what to build in terms a PM or designer understands.

## Determine the input

Check the value of `$ARGUMENTS`.

**If a name is provided** (e.g. the user ran `/spec order-list`):

1. Read the brief from `docs/briefs/$ARGUMENTS.md`.
2. If the file does not exist, tell the user: "I could not find a brief called '$ARGUMENTS'. You can create one with /brief, or just describe what you want to build and I will write the spec from that."
3. Use the brief content as the basis for the spec.

**If no argument is provided:**

1. Ask the user to describe what they want to build in a few sentences — what should appear on screen, what the user can do, and what data is involved.
2. After they describe it, summarise your understanding back to them and ask if you got it right.
3. Once confirmed, ask: "What short name should I use for this spec?" (this becomes the filename).

## Write the spec

Produce a spec file at `docs/specs/<name>.md` (create the `docs/specs/` directory if it does not exist). Specs live in `docs/specs/` and briefs live in `docs/briefs/` — keep them separate so a brief and a spec with similar names cannot overwrite each other.

Use exactly this structure:

```
# <Feature Name>
Slice: <which area of the platform this belongs to>
Created: <today's date>

## What the user sees
- [Describe each screen or layout the user will encounter]

## What the user does
- [Describe each interaction — buttons they click, forms they fill in, things they upload or navigate to]

## Data shown
- [List the domain entities displayed and their key details, with example values]

## Components needed
- [Name and briefly describe each reusable piece of the interface]

## Pages needed
- [List each page — its path in the app and what it shows]

## Notes
- [Any additional context, edge cases, or constraints worth remembering]
```

### Rules for the spec

- Keep the total file under 200 lines.
- Write everything in plain English — describe what users see and do, not how the code works.
- Map the feature to one of the existing platform areas listed in `CLAUDE.md` (the "Current feature slices" table). If none of these fit, capture the spec's area as the user's own short phrase (e.g. "Billing", "Onboarding") and note in the spec's "Notes" section that this is a new area — we'll formalise it as a slice when the work begins, not at spec time.
- Use the project's domain terms consistently (see `agents/guide.md` for the glossary).
- Do NOT include: architecture decisions, implementation tasks, dependency graphs, technical patterns (Server Components, Client Components, Zod schemas, TypeScript interfaces), or file paths.

## After saving

Tell the user where the spec was saved (`docs/specs/<name>.md`) and suggest next steps: "You can now use `/page <name>` or `/component` to start building from this spec."
