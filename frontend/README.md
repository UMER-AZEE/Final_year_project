# Sentinel AI frontend — local development

1. Open a terminal in `frontend` and use Node.js 22, the version pinned in `.node-version`.
2. Install dependencies with `npm ci` and start Vite with `npm run dev`.
3. Start the backend on port 8000 in a second terminal. Vite forwards `/api` requests to `http://127.0.0.1:8000`. [Backend setup](../backend/README.md)

Run `npm run lint` and `npm run build` before handing off changes.

Administration pages manage users, groups, policies, integrations, and organization settings. Prompt Studio enforces applicable input and output policies. Its history stores conversations, with multiple messages per chat, and supports continuing a chat after reload. Each follow-up rechecks recent allowed conversation context against the current policies; blocked, failed, withheld, and expired messages are excluded. Earlier standalone runs remain available as separate chats.

Enter sends a message on desktop; Shift + Enter adds a line. On mobile, Enter adds a line and the Send button submits. Each message retains its own security decision and usage details inside the chat. Context is bounded to 20 recent allowed exchanges and 24,000 characters; it is not unlimited chat memory.

The conversation browser regression is `node tests/conversations.browser.cjs`, with a Vite server running. It requires Playwright and Chrome; configure `PLAYWRIGHT_MODULE`, `CHROME_PATH`, and `UI_TEST_URL` if using an external test installation. All API calls in this regression are mocked.

The browser stores only the bearer token in local storage; organization membership and policy scope come from the server. A protected request returning 401 clears the token and returns to sign-in. Creating accounts or inviting users requires a working SMTP sink on the backend; no verification code or invitation token is displayed in the browser as a delivery fallback.

## Netlify deployment

See [DEPLOYMENT.md](DEPLOYMENT.md). `netlify.toml` configures the build; set
`VITE_API_BASE_URL` to the deployed Heroku HTTPS URL ending in `/api` before building.
