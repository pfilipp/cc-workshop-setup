Run the project's three quality checks and report the results in plain English. Use plain English throughout — no technical jargon. The user is a PM or designer.

These are exactly the same checks that Vercel runs on every push, and the same checks `/ship` runs before sharing your work. Running them locally is the fastest way to catch problems before they reach the preview build.

## What to do

Run the three commands in this order. Capture the output of each so you can summarise it.

1. `npm run typecheck` — checks that all the data shapes in the code line up.
2. `npm run lint` — checks for code style and common mistakes.
3. `npm run build` — does a full production build of the site.

You can run them sequentially with `npm run typecheck && npm run lint && npm run build` if you want a single command.

## How to report the results

### If everything passes

Tell the user:

> ✅ All checks passed. Your work is ready to share with `/ship`.

### If anything fails

For each failure, group the issues by which check produced them and explain in plain English:

- For **typecheck** failures: describe what data shape doesn't match. For example: "In the order list component, a field called `status` expects one of `pending`, `paid`, or `refunded`, but it was given a plain string." Mention the file in friendly terms (e.g. "the order list page", not the raw file path).
- For **lint** failures: describe the style or correctness issue. For example: "There's an unused import in the billing dashboard component."
- For **build** failures: describe what blocked the production build. Often this is a missing dependency or a Server Component using something that only works on the client. Translate the technical message into plain language.

After listing the issues, end with:

> Many of these can be fixed automatically. Run `/check:fix` and I'll fix what I can, then re-run the checks to show what (if anything) still needs your attention.

## Communication rules

- Use plain English — say "data shape", "code style", "production build", not "TypeScript", "ESLint", "Next.js compilation".
- Translate file paths into human-friendly descriptions ("the order list page" rather than `app/(features)/billing/orders/page.tsx`).
- Group issues by area of the platform where possible — the user thinks in those terms.
- Never offer to apply autofixes silently — that's what `/check:fix` is for.
