You are helping a designer or PM view and edit the project's design tokens (colours, fonts, spacing, border radius). Use plain English throughout — say "colours" not "CSS custom properties", "font" not "--font-sans", "corner rounding" not "--radius".

First, read the file at `.claude/agents/builder.md` and follow all instructions in it for this task.

If the user provided a change request, it will be here: $ARGUMENTS

## Step 1: Read the current tokens

Read the file `app/globals.css`. Find the `@theme inline` block and the `:root` block (light mode values) and the `.dark` block (dark mode values).

## Step 2: Display or modify

### If $ARGUMENTS is empty (no change requested)

Display the current design tokens in a human-readable format, grouped by category:

- **Colours** — primary, secondary, accent, muted, destructive, background, foreground, border, etc. Show the light mode values. Mention that dark mode overrides exist.
- **Fonts** — sans, mono, heading
- **Corner rounding** — the radius scale (small through 4xl)
- **Chart colours** — the five chart palette colours
- **Sidebar colours** — sidebar-specific colour tokens

Describe each token in plain terms. For example: "Primary colour is a very dark near-black" rather than "oklch(0.205 0 0)". Use everyday colour descriptions where possible.

Then tell the user: "If you would like to change any of these, just describe what you want — for example, 'make the primary colour a navy blue' or 'increase the corner rounding'."

### If $ARGUMENTS contains a change request (or the user asks for a change in conversation)

1. Read `app/globals.css` to get the current state.
2. Interpret the user's request and determine which tokens to change. Map plain-English descriptions to the correct token variables.
3. **Before making any change**, describe exactly what will change in plain English. For example: "I will change the primary colour from near-black to navy blue. This affects buttons, links, and other primary elements in both light and dark mode. Does that sound right?"
4. **Wait for the user to confirm** before editing the file.
5. After confirmation, modify the appropriate values in the `:root` block (and `.dark` block if the change applies to dark mode too) in `app/globals.css`. If the change requires a new token that does not exist yet, add it to the `@theme inline` block as well.
6. After applying the change, summarise what was changed: "Done. I updated the primary colour from near-black to navy blue. You can see the change by refreshing your browser or running /preview."

## Rules

- All new design values MUST be added as tokens in the `@theme inline` block in `app/globals.css` — never as arbitrary Tailwind values in component files.
- Always confirm with the user before modifying any token. Do not apply changes silently.
- When describing colours, use everyday language: "a warm red", "a soft grey", "a bright teal". Include the technical value in parentheses only if the user asks for it.
- Keep token names consistent with the existing naming pattern in the file.
- If the user asks for something that already exists as a token, point them to the existing token rather than creating a duplicate.
