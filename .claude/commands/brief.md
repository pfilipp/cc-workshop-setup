You are helping a non-technical user capture what they want to build. First, read the file at `.claude/agents/guide.md` and follow all communication rules and domain knowledge in it for this entire conversation.

If the user provided an argument, treat it as the starting topic: $ARGUMENTS

## How this works

You are going to have a short guided conversation to understand what the user wants to build. Ask questions one at a time, wait for each answer, and summarise what you heard before moving on. Use only plain English and the project's domain terms — never mention TypeScript, React, components, routes, server actions, or any framework concepts.

## Questions to ask (one at a time)

**1. What do you want to build?**
Ask the user to describe their idea in their own words. If they gave a starting topic via $ARGUMENTS, reflect it back and ask them to expand. Summarise what you understood before continuing.

**2. Who is this for?**
Ask who will use this screen or feature. Use the audience labels listed in `CLAUDE.md` and `agents/guide.md` for the project's domain (e.g. internal staff, external customers, partner organisations). Summarise.

**3. What should appear on screen?**
Ask them to describe what someone would see — lists, cards, forms, status indicators, charts, buttons, messages. Encourage them to think about what it looks like, not how it works technically. Summarise.

**4. What can the user do?**
Ask what actions or interactions are available — clicking, filtering, searching, filling in details, uploading documents, navigating to another screen. Summarise.

**5. What data is involved?**
Ask which domain entities appear on this screen. Offer the project's domain terms (see `CLAUDE.md` and `agents/guide.md` for the current vocabulary). Summarise.

**6. Which part of the system does this belong to?**
Present the project's current feature areas in plain English — read them from `CLAUDE.md` (the "Current feature slices" table) and present them with friendly descriptions. Always include a final option:
- **Something new** — an area not on the existing list

If the user picks **Something new** (or says it doesn't fit any of the above), ask: *"What would you call this area, in a short phrase? (For example: 'Billing', 'Onboarding', 'Reporting'.)"* Capture their answer in the brief as the area name. Mention gently that this might be a new area of the system — when the brief becomes a spec and then real pages, we may need to add it formally — but for the brief itself, just record the name they gave.

**7. Anything else?**
Ask if there are edge cases, stakeholder requirements, or constraints they want to capture. If not, that is fine — move on.

## When the conversation is complete

1. Ask the user for a short name for this brief (a few words, like "order-list" or "settings-page"). This will be used as the filename. Avoid names that end in `-spec` to keep briefs clearly separate from specs (specs live in `docs/specs/`, briefs live in `docs/briefs/`).
2. Create the directory `docs/briefs/` if it does not already exist.
3. Save the structured brief to `docs/briefs/<name>.md` using this format:

```
# Brief: <Title derived from the conversation>

## Who is this for?
<Their answer>

## What problem does it solve?
<Synthesised from their goals in question 1>

## What should appear on screen?
<Their answer from question 3>

## What can the user do?
<Their answer from question 4>

## What data is involved?
<Their answer from question 5>

## Which part of the system is this?
<Their chosen area from question 6>

## Anything else?
<Their answer from question 7, or "Nothing additional.">
```

4. Confirm the saved location: "Your brief is saved at `docs/briefs/<name>.md`. When you are ready to turn it into a detailed spec, run `/spec <name>`."
