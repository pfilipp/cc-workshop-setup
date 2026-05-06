You are helping a non-technical user save and share their work. Use plain English throughout — no git jargon, no technical terms. Follow these steps exactly.

If the user provided arguments, treat them as the description of their changes: $ARGUMENTS

## Step 1: Check for changes

Run `git status --porcelain` in the project root. If the output is empty, tell the user: "There is nothing new to save — your project is already up to date." Then stop.

## Step 2: Show what changed

Analyse the output of `git status` and `git diff --stat`. Summarise the changed files in plain English grouped by feature area. For example: "You are about to save: 3 new files in the billing section, 1 updated mock data file, and 1 changed design token." Do NOT list raw file paths — translate them into language the user understands (e.g. `features/billing/InvoicePanel.tsx` becomes "a new component in the billing section").

Internally keep the *list of paths* you summarised — you'll need it again in Step 4 for explicit staging.

Ask the user: "Does this look right? Type **yes** to continue or tell me what to change."

Wait for confirmation before proceeding.

## Step 3: Determine the branch

Run `git branch --show-current` to find the current branch.

**If on `main`:**
- If the user has not already provided a description via $ARGUMENTS, ask: "In a few words, what did you work on?" Use their answer as the description.
- Look at which feature slices the changes touch. Read the slice list from `CLAUDE.md` (the "Current feature slices" table). If the changes are inside a slice folder you don't recognise (i.e. a new folder under `features/` or `app/(features)/`), treat that folder name as the slice — don't force-fit into the existing list.
- If changes span multiple slices, ask: "Your changes touch several areas. Which one is the main focus?" and list the options in plain English.
- Create a branch name following the project convention:
  - Feature slice work: `feature/<slice>/<short-description>` (e.g. `feature/billing/add-invoice-list`)
  - Design system work: `design-system/<short-description>`
  - Bug fixes: `fix/<short-description>`
- Slugify the description: lowercase, hyphens instead of spaces, no special characters.
- Run `git checkout -b <branch-name>` to create and switch to the new branch.
- Tell the user: "I created a new working branch for this."

**If on any other branch (not `main`):**
- Tell the user: "You are working on '<branch-name>'. I will save your changes there." and ask them to confirm or say if they want a different branch.
- Wait for confirmation.

## Step 4: Stage changes safely

Stage **only** the files you summarised in Step 2.

1. Modified and deleted files: run `git add -u` (this stages tracked changes only — it never adds new untracked files).
2. New (untracked) files: list them explicitly to the user with `git status --porcelain | grep '^??'`. For each one, decide:
   - If it is clearly part of the work (a new component, story, mock factory, page, action, type, etc.), add it by name: `git add <path>`.
   - If anything looks suspicious — `.env*`, `*.pem`, `*.key`, `*.p12`, `*.zip`, `*.sqlite`, anything inside `node_modules/` (shouldn't happen but check), files outside the slices being worked on, or anything you do not recognise — **stop and ask the user**: "I'm not sure whether to include `<path>`. Can you confirm?" Do not stage it without confirmation.
3. **Never** run `git add -A` or `git add .`. Those stage everything including potentially-secret files.

Show the user the final list of staged paths and ask once more: "Ready to save these? Type **yes** to commit."

## Step 5: Commit

- Determine the commit type: `feat` for new features or pages, `fix` for bug fixes, `chore` for cleanup or config changes.
- Determine the scope from the primary slice name (read from `CLAUDE.md`).
- Compose a conventional commit message: `feat|fix|chore(<scope>): <description>`
- Commit with the composed message (the files were staged in Step 4).

## Step 6: Quality checks

Run these three checks before pushing. They are the same checks Vercel will run on the preview build — running them here means the user will not get a confusing red preview.

1. `npm run typecheck`
2. `npm run lint`
3. `npm run build`

For any failure:
- Read the error output and explain each issue in plain English. For example: "There is a type mismatch in the order list component — a field called 'status' expects specific values but got a plain string." or "The build failed because a page component imports something that doesn't exist."
- Do NOT push.
- Tell the user: "I found some issues that need fixing before I can share your work. Would you like me to try fixing them?"
- Stop here and wait for the user to respond.

If all three pass, tell the user: "All checks passed."

## Step 7: Push and share

- Run `git push -u origin <current-branch>` to push the branch.
- Read the Vercel preview URL pattern from `CLAUDE.md` (section 6 — Vercel Deployment Workflow). If a pattern is recorded there (e.g. `https://<project>-<branch-slug>.vercel.app`), substitute the branch slug (replace `/` with `-`) and display the URL. If no pattern is recorded, tell the user: "Pushed. The Vercel preview URL will appear on your branch's page on GitHub once the build kicks off."
- Tell the user: "Your work is live! Share this link with anyone who needs to review it. The URL is up immediately, but the preview may take a minute or two to finish building — wait for it to go green before sharing widely."

## Step 8: Undo guidance

Always end with:

> "**To undo:** if this is not what you wanted:
> - To undo just the commit but keep all your file changes, run `git reset --soft HEAD~1`.
> - To unsave the work back to how it was before this command (this throws away the commit; the files stay), run `git reset HEAD~1`.
> - To remove the remote branch as well, tell me which branch and I'll help you delete it.
> Tell me what you wanted differently and we'll fix it together."

## Safety rules — follow these absolutely

- NEVER run `git push --force` or any force-push variant.
- NEVER push directly to the `main` branch. If on `main`, always create a new branch first.
- NEVER use `git add -A` or `git add .`. Always stage explicitly (see Step 4).
- NEVER skip the quality checks in Step 6.
- NEVER commit without showing the user what will be saved and getting confirmation.
- NEVER stage a file matching `.env*`, `*.pem`, `*.key`, or `*.p12` without explicit user confirmation.
