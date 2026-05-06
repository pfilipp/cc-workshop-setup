# Workshop Claude Setup

This is the **From Figma to Deployment** workshop's starter Claude config. It drops a pre-tuned `.claude/` folder and `CLAUDE.md` into your project so Claude Code knows your stack, your conventions, and a small set of friendly slash commands designed for designers and PMs.

## What this gives you

- **`CLAUDE.md`** — the project's brain. Documents the assumed stack (Next.js + Tailwind v4 + shadcn/ui + Storybook 10 + Vercel), file layout, naming conventions, and PR standards. Edit as your project evolves.
- **`.claude/agents/`** — three persona agents:
  - `builder.md` enforces conventions when creating new code
  - `reviewer.md` audits work before merge
  - `guide.md` is the domain expert that translates business intent into platform terms
- **`.claude/commands/`** — slash commands that wrap common tasks in plain English:
  - `/init-framework` — **run this first** to fill in the placeholders for your specific project (domain, feature areas, voice, integrations, deployment)
  - `/brief`, `/spec`, `/page`, `/component`, `/mock` — the build-from-an-idea flow
  - `/preview` — start the dev server or Storybook
  - `/check:validate`, `/check:fix` — run the three quality checks (typecheck, lint, build) and auto-fix what can be fixed
  - `/review`, `/ship` — audit and push your work
  - `/status` — plain-English summary of what's been built
  - `/tokens` — view and edit design tokens
  - `/ux-copy` — write or review UI copy with the right tone

## How to use it

The workshop deck shows the canonical install:

```bash
cd ~/Projects/<your-app>
curl -L https://github.com/<TEMPLATE_REPO_URL>/archive/main.tar.gz \
  | tar xz --strip=1
```

That extracts `.claude/`, `CLAUDE.md`, and this README directly into `~/Projects/<your-app>`. Anything already at those paths is overwritten — run it on a fresh project (or one where you're happy to replace those files).

Then, inside the project:

```bash
claude
```

…and run `/init-framework` as your first command. It will ask 5–7 short questions about your project and fill in the domain-specific placeholders across `CLAUDE.md`, `agents/guide.md`, and `commands/ux-copy.md`.

## Stack assumptions

This template ships pre-configured for **Next.js + Tailwind v4 + shadcn/ui + Storybook 10 + Vercel + npm**. If your stack differs significantly, you'll want to edit `CLAUDE.md` directly — `/init-framework` only fills the domain-specific bits (it doesn't change the assumed stack).

## What `/init-framework` won't do

- It won't guess your domain — you have to describe it.
- It won't install npm packages or scaffold Next.js — that's the workshop's earlier steps.
- It won't push to git or create a Vercel project — those are separate, deliberate actions.

## Editing the template

Everything is plain Markdown. Edit any agent or command file directly to adapt it to your team — they'll be picked up the next time Claude Code reads the project.
