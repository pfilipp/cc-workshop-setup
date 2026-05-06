Give the user a clear, plain-English overview of what has been built in this project. First, read the file at `.claude/agents/reviewer.md` and follow its communication and audit conventions throughout.

If the user provided an argument, treat it as a spec name to check progress against: $ARGUMENTS

## What to scan

Examine the project directories and report what exists across each area of the platform. Use the plain-English names — never show raw file paths or technical identifiers in your output.

For each slice listed in `CLAUDE.md` (the "Current feature slices" table), look at three places. A slice may not have every subfolder — only `actions/` is common; `hooks/`, `api/`, `utils/`, and `types.ts` are added as needed.

For every slice in the table, check:
- **Routes** — under `app/(features)/<slice>/**/page.tsx` (or whichever route group the slice uses).
- **Slice logic** — under `features/<slice>/`.
- **Shared UI used by this area** — components in `components/` whose Storybook `title` references the slice's PascalCase name, or which are imported by the slice's routes.

Also scan once across the whole project:
- **Shared design components** — every `.tsx` file under `components/atoms/`, `components/molecules/`, `components/organisms/` (and any legacy file still sitting flat in `components/`) and whether each has a co-located `.stories.tsx`. Components in `components/ui/` are auto-generated shadcn primitives — count them but don't audit them for stories.
- **Mock data** — every factory file in `lib/mocks/` (and whether it is re-exported from `lib/mocks/index.ts`).

> Note: this project does **not** use a `features/<slice>/components/` folder. All UI components live under the atomic subfolders of the top-level `components/` folder regardless of which area they serve. Do not look for components inside `features/`.

## How to report

For each area, state in one or two sentences what pages exist, what components are available for that area (whether shared with other areas or specific to it), whether visual previews exist, and whether mock data supports it. If an area has nothing built yet, say so plainly (e.g. "The billing section has not been started yet.").

After the per-area summary, add:
- **Shared design components** — list any shared components and flag any missing visual previews (`.stories.tsx`).
- **Mock data** — which domain entities have mock data factories ready.
- **Overall progress** — a brief sentence like "5 of 8 areas have pages; 6 have visual previews."

## If a spec name was provided

If `$ARGUMENTS` is not empty, read `docs/specs/$ARGUMENTS.md` first; if it does not exist, fall back to `docs/briefs/$ARGUMENTS-spec.md` (legacy location) so old specs still work. Compare the "Pages needed" and "Components needed" sections of the spec against what actually exists in the project. Report what has been built and what still remains, in plain English. For example: "The spec calls for an order list page and an order detail page. The list page exists but the detail page has not been created yet."

## Communication rules

- Use plain English throughout. Say "pages" not "route files", "visual previews" not "Storybook stories", "mock data" not "factory functions".
- Do not display raw file paths. Translate every path into a human-friendly description.
- Keep the report concise — aim for a summary someone can read in under a minute.
