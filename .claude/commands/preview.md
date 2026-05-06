Launch the development preview so the user can see their work in a browser.

## What to do

Check the value of `$ARGUMENTS`.

### If the argument is "storybook"

1. Check whether port 6006 is already in use by running `lsof -ti:6006`.
2. If the port is **free**, start Storybook in the background with `npm run storybook` and wait a few seconds for it to be ready.
3. If the port is **already in use**, skip starting — Storybook is already running.
4. Open the browser with `open http://localhost:6006`.
5. Tell the user: "Storybook is open at localhost:6006. You can browse your components there."

### Otherwise (no argument, or anything other than "storybook")

1. Check whether port 3000 is already in use by running `lsof -ti:3000`.
2. If the port is **free**, start the dev server in the background with `npm run dev` and wait a few seconds for it to be ready.
3. If the port is **already in use**, skip starting — the dev server is already running.
4. Open the browser with `open http://localhost:3000`.
5. Tell the user: "The app is open at localhost:3000. Any changes you make will appear automatically."

## Communication rules

- Use plain English — do not mention Next.js, Node, or port numbers beyond the URL itself.
- If something goes wrong starting the server, explain the problem simply and suggest running the command again.
