---
name: init-framework
description: One-time onboarding for the workshop Claude template. Asks about your domain, feature slices, brand voice, integrations, and deployment, then fills in the placeholders across CLAUDE.md, agents/guide.md, and the slash commands.
argument-hint: "(optional) one-line description of the project"
---

# /init-framework

You are running this project's **one-time onboarding**. The workshop template ships with placeholders so it stays domain-agnostic. Your job is to fill those placeholders by having a short, friendly conversation with the user, then editing the relevant files.

Use plain English throughout. The user is most likely a designer or PM who has just curl'd this template into a fresh Next.js app — they may not know what TypeScript is, let alone what a "feature slice" is. Translate technical terms into everyday words.

If `$ARGUMENTS` is provided, treat it as a one-line description of what the project is and use it to seed the first question.

## Files this command will mutate

When you have all answers, you will edit:

| File | What changes |
|---|---|
| `CLAUDE.md` | Domain summary at the top, feature slice table (section 3), integrations note, deployment URL pattern (section 6) |
| `.claude/agents/guide.md` | Domain summary, glossary table, "areas of the platform" list |
| `.claude/commands/brief.md` | (no static slice list — already reads from CLAUDE.md) |
| `.claude/commands/component.md` | (no static slice list — already reads from CLAUDE.md) |
| `.claude/commands/page.md` | (no static slice list — already reads from CLAUDE.md) |
| `.claude/commands/spec.md` | (no static slice list — already reads from CLAUDE.md) |
| `.claude/commands/status.md` | (no static slice list — already reads from CLAUDE.md) |
| `.claude/commands/ux-copy.md` | Slice/audience/tone table |
| `.claude/commands/ship.md` | (already reads Vercel URL pattern from CLAUDE.md) |
| `.claude/commands/mock.md` | (no static entity list — already reads from agents/guide.md and types/) |

Most commands already defer to `CLAUDE.md` and `agents/guide.md`, so the bulk of the editing happens in those two files plus `ux-copy.md`.

## How to run

Walk the user through the questions below **one at a time**. After each answer, summarise it back to confirm you got it right before moving on. If they say "skip" or seem unsure, accept a sensible default — never block them on a question.

### Question 1 — What is the project?

Ask:
> "In one or two sentences, what is this project for, and who uses it?"

If `$ARGUMENTS` is set, reflect it back: *"You mentioned this is for ___. Can you expand on that — what does it do, and who's the primary user?"*

Capture: a one-sentence product summary + a one-sentence audience description.

### Question 2 — What are the feature areas?

Explain first:
> "A 'feature area' is a broad section of the product — like Billing, Onboarding, Reporting, or Customer Portal. Most products have 3–8 of these. Each area becomes its own folder in the codebase, so picking sensible names now makes everything easier later."

Then ask:
> "What are the main areas of your product? List 3–8 names. For each one, give me a one-sentence description of what it covers."

Capture: an array of `{ name (kebab-case slug), label (plain English), description (one sentence), audience (who uses it), tone-default (one phrase) }`.

If the user can only think of 1–2 areas, that's fine — you can pad the list to 3 with sensible adjacent guesses, but flag them as "starter slices" so the user knows to revise as the product grows.

### Question 3 — What is the project's vocabulary?

Explain:
> "Most products have 5–10 words that show up everywhere — names for things like 'order', 'invoice', 'customer', 'shift', 'lesson'. Using those words consistently across the app makes it feel coherent. What are yours?"

Capture: an array of `{ term, plain-English meaning }`. Aim for 5–10 entries. If the user lists fewer, that's fine.

### Question 4 — Are there external systems involved?

Ask:
> "Does the product talk to any outside systems — payment providers, address lookup, analytics, a CRM? Or is it self-contained for now?"

Capture: a list of integrations with a one-line note on what each is for, or "none".

### Question 5 — What is the brand voice?

Ask:
> "If your product was a person writing the UI text, how would you describe their voice? Two or three adjectives. Examples: 'warm, plain-spoken, never patronising' / 'precise, businesslike, expert' / 'playful, encouraging, never serious'."

Capture: 2–3 adjectives + one example sentence the user thinks captures the voice.

### Question 6 — Where will it be deployed?

Ask:
> "Are you deploying this to Vercel? If yes, what's your Vercel project name? (You'll find it in the Vercel dashboard — it's the slug in your project URL.) Or say 'skip' if you're not sure yet."

Capture: a Vercel project slug (used to construct the preview URL `https://<slug>-<branch>.vercel.app`), or `null`.

### Question 7 — Anything else worth knowing?

Ask:
> "Anything else I should record about your project — constraints, deadlines, sensitive contexts, decisions you've already made? Or 'no' to finish."

Capture as free text. Keep it short.

## Now edit the files

After all answers are gathered, summarise everything back to the user:

> "Here's what I've captured: [one-paragraph summary]. I'm about to update CLAUDE.md, agents/guide.md, and ux-copy.md with this. Type **yes** to apply, or tell me what to change."

Wait for confirmation. Then make the edits below.

### Edit 1: `CLAUDE.md`

- Replace the **Product Purpose and Domain Context** section (top of the file, currently a placeholder) with: a one-paragraph summary derived from Q1, the workflow if the user described one, and the glossary table from Q3.
- In **section 3**, replace the **Current feature slices** table with one row per slice from Q2 (columns: slice slug, route path, plain-English description).
- In **section 6 (Vercel Deployment Workflow)**, replace `<your-vercel-project-slug>` (or similar placeholder) with the slug from Q6, so the preview URL line reads `https://<slug>-<branch>.vercel.app`. If Q6 was skipped, leave the placeholder in but add a comment: `<!-- TODO: set when Vercel project is created -->`.
- If Q4 returned integrations, add a short **Integrations** subsection under section 2 listing them with one-liners each.

### Edit 2: `.claude/agents/guide.md`

- Replace the **Domain Knowledge** placeholder section with: the product summary from Q1, the glossary table from Q3, and the **areas of the platform** list rebuilt from Q2 (one bullet per slice in plain English).
- Remove the leading "**Heads up — this agent has placeholder content**" note now that it's filled.

### Edit 3: `.claude/commands/ux-copy.md`

- Replace the placeholder slice/audience/tone table with one row per slice from Q2, using the audience and tone-default fields.
- Remove the leading "**Heads up — the slice + audience table below is a placeholder**" note.
- If Q5 returned a strong voice direction, add a one-line "Project voice" callout at the top of the **Voice and Tone** section — e.g. *"This product's voice is warm, plain-spoken, never patronising."*

### Edit 4: `README.md`

- Replace the placeholder project summary with the one-line product description from Q1.

## After the edits

Print a one-page summary of what got configured:

```
✅ Project configured.

Domain: <one-line summary>
Feature slices: <slice1>, <slice2>, <slice3>, …
Voice: <adjectives>
Vercel: <slug or "not set">

Files updated:
- CLAUDE.md
- .claude/agents/guide.md
- .claude/commands/ux-copy.md
- README.md

Suggested next steps:
1. Run `/preview` to start the dev server and confirm the app boots.
2. Run `/brief` to capture your first feature in plain English.
3. Run `/spec <name>` to turn that brief into a buildable spec.
4. Run `/page <name>` or `/component` to start building.

If anything in the captured config is wrong, just edit the files directly — they're plain Markdown.
```

## Safety rules

- **Always show the user the planned changes before writing.** No silent file edits.
- **Never run `/init-framework` more than once without warning.** If `CLAUDE.md` has already been customised (no `<YOUR DOMAIN>` placeholders detected), tell the user: *"Looks like this project has already been initialised. Running this again will overwrite the captured config. Want to continue?"*
- **Never delete content that wasn't a placeholder.** If you find unexpected user content where a placeholder should be, ask before touching it.
- **Use `/init-framework` only inside a project directory** (a folder containing `package.json` or where one is about to be created). If `pwd` doesn't look like a project root, warn the user.
