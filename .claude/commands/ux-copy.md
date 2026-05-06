---
name: ux-copy
description: Write or review UX copy — microcopy, error messages, empty states, CTAs, onboarding, and confirmations. Trigger with "write copy for", "what should this button say?", "review this error message", or when naming a CTA, wording a confirmation dialog, filling an empty state, or writing onboarding text.
argument-hint: "<context or copy to review>"
---

# /ux-copy

Write or review UX copy for any screen in this project.

## About this project

> **Heads up — the slice + audience table below is a placeholder.** Run `/init-framework` to populate it from your project's actual feature areas, audiences, and tone. Until then, fall back to the project-wide voice rules in the **Voice and Tone** section and ask the user clarifying questions before writing.

Each feature slice in this project has a distinct audience and tone. Always confirm which slice the copy is for before writing — the right voice changes between them.

| Slice | Audience | Tone defaults |
|---|---|---|
| **`<Slice 1>`** | `<Who uses this section>` | `<How copy here should sound — pace, formality, jargon allowance>` |
| **`<Slice 2>`** | `<Who uses this section>` | `<…>` |
| **`<Slice 3>`** | `<Who uses this section>` | `<…>` |

If the user names a slice that does not appear in this list, don't force-fit. Two things to check:

1. **Could it belong to a sibling product, marketing site, or workspace?** Some repos sit alongside other sites (a marketing landing page, a docs site, a separate admin tool). If so, ask the user to confirm — copy guidance for those lives elsewhere, not in this file.
2. **Could it be a new area inside this project?** That's fine — capture what they tell you about the audience (who uses it, what they're doing, what tone fits) and write the copy from those facts. Don't force a brand-new internal area into the closest existing row's voice. Ask the audience-and-tone questions afresh.

New slices can be added to the table later via `/init-framework` (or by editing this file directly).

## Usage

```
/ux-copy $ARGUMENTS
```

## What I Need From You

- **Which slice**: see the table above (or describe a new one).
- **Context**: What screen, flow, or moment? (e.g. "the empty state on the orders queue when there are no items waiting.")
- **User state**: What is the person trying to do? Are they likely worried, confused, in a hurry?
- **Tone**: Reassuring, neutral, instructive? (See the table above for slice defaults.)
- **Constraints**: Character limits, layout limits, anything from the design.

## Principles

1. **Clear** — Say exactly what we mean. No ambiguity. In consumer-facing slices, that also means no jargon (technical or industry-specific) without a plain-English explanation alongside.
2. **Concise** — Use the fewest words that carry the full meaning. Older or less-confident readers especially appreciate copy that respects their time.
3. **Consistent** — The same word for the same thing, every time. If we call it "your order" on one screen, we don't call it "your purchase" on the next.
4. **Useful** — Every word should help the person take the next step or feel more in control.
5. **Human and supportive** — Write like a knowledgeable colleague who genuinely wants to help. Never robotic, never corporate. We're alongside the person, not above them.

## Copy Patterns

### CTAs
- Start with a verb: "Start free trial", "Save changes", "Download report"
- Be specific: "Create account" not "Submit"
- Match the outcome to the label so the person knows what will happen

### Error Messages
Structure: What happened + Why + How to fix.
- "Payment declined. Your card was declined by your bank. Try a different card or contact your bank."
- Always offer a way forward. Never leave someone stuck.

### Empty States
Structure: What this is + Why it's empty + How to start.
- "No projects yet. Create your first project to start collaborating with your team."

### Confirmation Dialogs
- Make the action clear: "Delete 3 files?" not "Are you sure?"
- Describe consequences plainly: "This can't be undone."
- Label buttons with the action: "Delete files" / "Keep files", not "OK" / "Cancel".

### Tooltips
- Concise, helpful, never stating the obvious.
- Good for explaining a term the person may not know — especially useful in consumer-facing slices where we want to demystify, not patronise.

### Loading States
- Set expectations and reduce anxiety. "This usually takes a few seconds" is kinder than a silent spinner.

### Onboarding
- One idea at a time. Build confidence step by step.
- In consumer-facing slices, assume the person has never used software like this before. In internal-facing slices, assume the person already knows their job — get them to the data quickly.

## Voice and Tone

The default voice across all slices is **professional and supportive**. Tone shifts with context:

- **Success** — Acknowledge the win, but stay measured. "All done — your changes are saved." not "Woohoo! 🎉"
- **Error** — Empathetic and helpful. Take responsibility where appropriate. Never blame the person.
- **Warning** — Clear and actionable. Tell people what's at stake and what to do about it.
- **Neutral** — Informative and concise. Trust the reader.
- **Sensitive moments** — Some products coincide with stressful or significant life events. Copy in these moments should be gentle, never transactional. Avoid phrases that sound like marketing.

### Word choices

- Prefer plain English over industry or legal terms in consumer-facing slices. If a specialist term is unavoidable, briefly explain it the first time it appears.
- In internal-facing slices it is fine — and often clearer — to use the standard industry term directly, without a definition.
- Pick a spelling convention (UK or US) and stick to it across the product. Default: UK English (organise, personalise, recognised) unless the project says otherwise in `CLAUDE.md`.
- Refer to the person as "you". Refer to ourselves as "we" or `<your-product-name>`.
- Favour "we're here to help" over "please contact support".

## Output

```markdown
## UX Copy: [Context]

### Slice
[Slice name from the table — or "new slice: <name>" if it doesn't fit]

### Recommended Copy
**[Element]**: [Copy]

### Alternatives
| Option | Copy | Tone | Best For |
|--------|------|------|----------|
| A | [Copy] | [Tone] | [When to use] |
| B | [Copy] | [Tone] | [When to use] |
| C | [Copy] | [Tone] | [When to use] |

### Rationale
[Why this copy works — user context, clarity, action-orientation, and how it lands for the audience of the chosen slice]

### Localization Notes
[Anything translators should know — idioms to avoid, character expansion, cultural context.]
```

## Tips

1. **Tell me which slice** — Tone and vocabulary differ a lot between consumer-facing and internal-facing slices.
2. **Be specific about context** — "Error message when a payment fails on the checkout" is far more useful than "error message".
3. **Share the user's emotional state** — A first-time customer mid-checkout feels very different to an internal operator triaging their tenth case of the day. Copy should meet them where they are.
4. **When in doubt, lean kinder and clearer** — If in doubt about tone, the copy should feel like a steady hand, not a hurdle.
