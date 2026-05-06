Auto-fix everything that can be safely auto-fixed, then re-run the checks and report what (if anything) still needs human attention. Use plain English throughout.

## What this command does

There are three checks (typecheck, lint, build). Some issues can be fixed by tools automatically; others need a human or a code change. This command:

1. Runs the auto-fixers.
2. Re-runs the three checks.
3. Reports remaining issues in plain English.

## Step 1: Auto-fix what tooling can fix

Run these in order. Each will silently fix what it can and leave the rest untouched.

1. `npm run lint -- --fix` — fixes auto-fixable code-style and import issues.

Other issues (type mismatches, broken imports, missing files, build errors) cannot be safely auto-fixed by tools — they need a real code change. We'll surface those in Step 3.

## Step 2: Re-run all three checks

Run the same three checks `/check:validate` runs:

1. `npm run typecheck`
2. `npm run lint`
3. `npm run build`

Capture the output of each.

## Step 3: Report results

### If everything passes after the auto-fix

Tell the user:

> ✅ All issues fixed automatically. Your work passes all three checks now. Run `/ship` when you're ready to share it.

Then list (briefly) what was auto-fixed — for example: "I removed 3 unused imports and reordered some code in the billing section."

### If issues remain

The remaining issues are ones tooling cannot safely fix. For each one, explain in plain English:

- **What** the issue is, in human terms (not framework terms).
- **Where** it is — translate file paths into friendly descriptions ("the order list page", not `app/(features)/billing/orders/page.tsx`).
- **Why** it matters (one sentence).
- **What you suggest doing** about it — and offer to apply the change.

End with a clear question:

> Would you like me to fix these for you? I'll explain each change before I make it.

If the user says yes, fix the issues one by one (each as a small, focused edit), then re-run `/check:validate` to confirm everything is green.

## Safety rules

- Never apply non-trivial code changes without describing them first to the user. Auto-fix from tooling is fine; refactors are not.
- If a fix would change behaviour the user might not expect (e.g. removing what looks like dead code, deleting a file, changing a function signature), stop and ask first.
- If `npm install` is needed to fix a missing dependency, tell the user what package is missing and ask before installing — adding new packages is something `CLAUDE.md` says to flag explicitly.

## Communication rules

- Plain English throughout — say "data shape doesn't match", not "type error".
- Group remaining issues by area of the platform where possible.
- Be encouraging — the user is non-technical and should feel the project is in good shape, not in trouble.
