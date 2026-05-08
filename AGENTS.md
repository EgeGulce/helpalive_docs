# Documentation project instructions

## About this project

- This is the public Mintlify documentation site for HelpAlive.
- Pages are MDX files with YAML frontmatter; navigation is configured in `docs.json`.
- Run `mint dev` from this directory to preview locally.
- Run `mint broken-links` to check internal references.
- Brand assets:
  - Logos: `logo/light.svg`, `logo/dark.svg`, `logo/icon.svg` (sourced from `trackSdk/apps/app/public/logo/`).
  - Favicon: `favicon.svg`.
  - Brand colors: primary `#3FACF8`, light `#68cbe9`, dark `#1494f0`.

## About the product (so docs stay accurate)

HelpAlive is a guidance layer for B2B SaaS, NOT a session-replay or general analytics tool. Three shipped components:

1. **The SDK** (`@helpalive/sdk`, ~11 KB) — script tag at `https://cdn.helpalive.com/sdk/helpalive.js`, `data-api-key` attribute. Public API: `init`, `identify`, `track`, `flush`, `reset`, `getContext`, `setRealTimeMode`, `setConsent`, `notify`, `startGuide`, `getSuggestions`, `shutdown`. **`userId` and `tenantId` are both required by `identify()`**.
2. **The AI chatbot** — Preact widget in Shadow DOM (~25 KB), hybrid retrieval (vector + BM25 + RRF) with cross-encoder rerank, Claude streaming over SSE. Knowledge sources: crawled URLs and uploaded files. Draft/publish workflow.
3. **Adoption analytics dashboard** — per-user/tenant/page views, coverage-gaps inbox (HDBSCAN-clustered).

**In progress, not shipped:** dashboard guides editor, pattern mining, automatic feature discovery, churn risk, segmentation, custom reports/exports, native integrations with help-desk tools.

**Never shipped (ignore old docs that imply otherwise):** Intercom/Zendesk/Salesforce/HubSpot/Slack native integrations. The only "integration" surface today is webhooks (talk to product before documenting specifics).

When writing or editing docs, check `trackSdk/docs/product.md`, `trackSdk/docs/features.md`, and `trackSdk/docs/helpalive-integration-guide.md` for the source of truth on what ships.

## Style preferences

- Active voice, second person ("you").
- One idea per sentence; concise paragraphs.
- Sentence case for headings.
- Bold for UI elements: Click **Settings**.
- Code formatting for file names, commands, paths, and code references.
- Honest status flags — don't claim features ship if they don't. Use Note callouts to mark in-progress areas.
- Prefer Mintlify components (`Card`, `CardGroup`, `Steps`, `Tabs`, `AccordionGroup`, `Note`, `Tip`) for visual hierarchy.

## Content boundaries

- Don't document internal architecture (`apps/drainer`, `apps/chat-service` internals, etc.). The audience is customers integrating HelpAlive, not engineers contributing to the codebase.
- Don't document features that exist only as UI shells in the dashboard — until they're wired up, they don't ship.
- Don't fabricate pricing. The plans pages keep the structure but defer numbers to "talk to sales" while pricing is being calibrated with design partners.
- Don't reference integrations that don't exist in code (Intercom, Zendesk, Salesforce, HubSpot, Slack native integrations).

## Terminology

- Use "AI chatbot" or "AI product assistant" for the widget. Avoid "bot" alone.
- Use "tenant" for workspace/organization (matches the `tenantId` field). Use "tenant" not "workspace" or "org" in user-facing copy where the API field name is exposed.
- Use "user" for end users of the customer's product. Use "team member" or "dashboard user" for the customer's own internal users.
- Use "knowledge base" for the chatbot's docs index, "sources" for individual URLs/files inside it.
- Use "coverage gaps" for the question-quality inbox.
- Use "DSAR" — spell out "data subject access request" on first use per page.
