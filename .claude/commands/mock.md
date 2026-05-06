Create or update mock data factories so prototypes look realistic. Use plain English throughout — no TypeScript jargon.

First, read the file at `.claude/agents/builder.md` and follow all instructions in it for this task.

If the user provided details, they will be here: $ARGUMENTS

## Step 1: Gather information

Ask the user two things (skip any already answered via `$ARGUMENTS`):

1. **Which domain entity?** Present the entities defined in this project's domain (read them from `agents/guide.md` and `types/`). If the user describes something that doesn't fit any existing entity, treat it as a new entity and continue.
2. **What kind of data?** Ask what they need. Examples:
   - A single record (e.g. "one order")
   - A list of records (e.g. "five customers at different lifecycle stages")
   - A specific scenario (e.g. "an order that failed payment", "a customer at the renewal stage", "an overdue task with no assignee")

## Step 2: Check existing types and factories

- Read the domain interfaces in `types/` to find the matching entity type. If the entity type does not exist yet, create it following the conventions in CLAUDE.md section 4 (PascalCase, discriminated unions, no `any`).
- Read `lib/mocks/` to see if a factory file already exists for this entity.

## Step 3: Create or update the factory

**If no factory file exists for this entity:**
- Create `lib/mocks/<entity-plural>.ts` (e.g. `orders.ts`, `customers.ts`, `tasks.ts`).
- Export a factory function that creates a single record: `createMock<Entity>()` (e.g. `createMockOrder()`).
- Export a list factory if the user asked for multiple records: `mock<Entity>List()` (e.g. `mockOrderList()`).
- If the user requested a specific scenario, export a named scenario factory: `createMock<Entity><Scenario>()` (e.g. `createMockOrderFailedPayment()`).
- Add re-exports to `lib/mocks/index.ts`. Create this file if it does not exist.

**If a factory file already exists:**
- Add the new scenario factory or update the existing factory to support the requested data.
- Do not break existing factory functions that other parts of the codebase may use.

All mock data must:
- Use realistic values for your domain — proper-looking names, identifiers, addresses, plausible dates. The exact shape of "realistic" depends on the domain (see `agents/guide.md`).
- Never use obviously fake placeholders like "Test User 1", "123 Test Street", "ID-0000", or "user@example.com"
- Be typed against the domain interfaces in `types/`

## Step 4: Summarise what was created

Tell the user in plain English:
- What mock data was created and what it looks like (give a brief example of the data — e.g. "an order from Sam Whitfield for £124.50, placed on 12 March")
- Which files were created or changed
- How to use it: "This data is available to any page or component through server actions. If you create a page with `/page`, it can use this mock data automatically."

## Step 5: Undo guidance

End with: "**To undo:** if this is not what you wanted, the following files were created or modified and can be safely reverted:" then list the full file paths of every file that was created or modified. If an existing file was modified, note that it was changed (not created) so the user knows a full deletion would be wrong — they should ask you to revert the specific change instead.
