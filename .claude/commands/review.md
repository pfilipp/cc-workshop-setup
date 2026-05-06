First, read the file at `.claude/agents/reviewer.md` and follow all instructions in it for this task. Also read `CLAUDE.md` in the project root (the same folder as `package.json`) — that file is the single source of truth for *this project's* stack, structure, and naming conventions.

If higher-up `CLAUDE.md` files exist (parent workspace, monorepo root), they provide useful context. Read them too if you need that context. Where the higher-up files describe conventions for *this project*, the project-root `CLAUDE.md` wins.

You are reviewing the current branch's changes before merging. Use plain English throughout — no technical jargon. The user is a PM or designer, not an engineer.

If the user provided arguments, use them as context: $ARGUMENTS

## Step 1: Identify what changed

Run `git diff main --name-only` to see which files have changed on this branch compared to main. If the branch is `main` (no changes against itself), check `git status` for uncommitted work instead. Summarise what was changed in plain English grouped by feature area.

## Step 2: Run the audit

Go through every item in the reviewer agent's audit checklist against the changed files (and any files they relate to). Check:

1. **Design system consistency** — Are components built from the standard building blocks? Is styling done properly?
2. **Storybook coverage** — Does every shared component have a story file so it can be previewed in isolation?
3. **Mock data quality** — Is the test data realistic for the domain, not placeholder text?
4. **Cross-slice boundaries** — Are feature areas kept independent, with shared code in the right place?
5. **TypeScript strictness** — Are types used properly, with no unsafe shortcuts?
6. **Directory structure** — Are files in the correct locations per project conventions?
7. **Server actions** — Do server actions validate their input with a schema?

## Step 3: Report findings

Group findings into three severity levels:

**Must fix** — Problems that would break project conventions or cause issues. These need to be resolved before merging.

**Should fix** — Quality improvements that are strongly recommended before merging.

**Nice to have** — Optional polish that would be good but is not blocking.

For each finding, explain in plain English: what the issue is, where it is, and how to fix it. Do not use file paths alone — describe the location in terms the user understands (e.g. "the order list component in the billing section").

If there are "must fix" or "should fix" items, ask the user if they would like help fixing them.

## Step 4: Confirm readiness

If there are no issues (or all issues have been resolved), tell the user:

"Your work follows all project conventions and is ready to merge. Here is how to merge it:

1. Go to the repository on GitHub.
2. You should see a banner offering to create a pull request for your branch — click it.
3. Add a short title describing what you built (e.g. 'Add order list page').
4. Click 'Create pull request', then click 'Merge pull request' and confirm.
5. Your changes will be live on the main site within a few minutes."

## Communication rules

- Plain English only — do not mention TypeScript, React, Next.js, Tailwind, Zod, or other technical terms.
- Describe issues in terms of what the user sees or what the project needs, not in code terms.
- Be encouraging — the user is not an engineer and should feel confident about their work.
