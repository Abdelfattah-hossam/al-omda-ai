# AL OMDA AI

An Arabic-first marketplace and workspace for reviewing work plans from specialist agents. The current MVP is a browser-only preview: it includes Business, Development, Design, Marketing, Research, and Personal Assistant specialties, with per-step approval controls and local task history.

## Current MVP boundaries

- Arabic is the default language; English is available from the language switch.
- Requests create specialty-specific example plans in the browser.
- Users can edit, approve, or reject each step, and pause, resume, or stop a task.
- Tasks and profile preferences are stored in that browser's `localStorage`.
- No AI provider, external actions, sign-in, shared database, or remote persistence is connected. Plan approval is not task execution.
- No API keys or credentials are needed to run the current preview.

## Run locally

Use Node.js 24 and pnpm from the repository root:

```sh
pnpm install
pnpm --filter @workspace/al-omda-ai run dev
```

Create a static production build:

```sh
pnpm --filter @workspace/al-omda-ai run build
```

Build output: `artifacts/al-omda-ai/dist/public`.

## GitHub and Cloudflare Pages

The repository is already initialized for Git. To publish it through GitHub and Cloudflare:

1. Create or choose a GitHub repository and connect it through Replit's Git controls. Review the staged files before the first push; do not commit credentials or generated build output.
2. In Cloudflare Pages, create a project connected to that repository. The repository root is the project root.
3. Set the build command to `pnpm --filter @workspace/al-omda-ai run build`.
4. Set the output directory to `artifacts/al-omda-ai/dist/public` and use Node.js 24 with pnpm.
5. Deploy the static site. No application secrets are needed for this preview.

The Vite build defaults to the domain root when `BASE_PATH` is not supplied, which matches the current Cloudflare Pages setup. The Pages deployment hosts the UI only; it does not deploy this workspace's separate API server or provide shared user data.

## Supabase integration — not connected

The current app does not require a Supabase account. Before adding accounts or shared task history:

1. Create a Supabase project and choose the authentication methods and redirect URLs.
2. Agree on the account and task data model, then create migrations and enable row-level security so each user can access only their own data.
3. Decide where authenticated API requests will run. Cloudflare Pages currently serves only the static UI; shared persistence requires a server-side API or an intentionally designed Cloudflare runtime.
4. Configure credentials only after explaining where each value is stored and how it is used. Never put a Supabase service-role key or AI-provider secret in browser code or the built JavaScript. Supabase's anon/publishable key is designed to be visible in browser apps, but must be protected by row-level security; if no key may appear in the browser at all, use a server-side auth/API design instead.
5. Replace browser-only storage with authenticated persistence and verify data isolation before enabling real accounts.

No Supabase Replit integration is connected in this project. Connecting Supabase directly will require a Supabase account and an explicit setup decision; the preview continues to work without either.

## Project structure

- `artifacts/al-omda-ai/` — AL OMDA AI web app
- `artifacts/api-server/` — shared API service scaffold; not used by this MVP
- `lib/` — shared API, database, and validation packages from the workspace scaffold

