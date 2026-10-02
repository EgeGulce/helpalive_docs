# Documentation project instructions

This is the public Mintlify site for HelpAlive (docs.helpalive.com). Mintlify publishes `main`, so a merge to `main` is a production publish. Only Ege merges or approves a merge.

- Pages are MDX files with YAML frontmatter. Navigation, tabs and redirects are in `docs.json`.
- Run `npx mint dev` to preview and `npx mint broken-links` before every push.
- Mintlify generates `/llms.txt` and `/llms-full.txt` from the pages, so don't add a hand-written one.
- `skill.md` at the root is hand-written and replaces the one Mintlify would generate. Update it when the install, `identify()`, verify-users or CSP pages change.
- `title` is the search-engine title (about 35-48 characters, naming HelpAlive); `sidebarTitle` stays short. Keep `description` at 120-160 characters.
- This file and anything else for writers stays in `.mintignore`, so it is never published.

## The four tabs
- **Product:** what end users get (chat, assists, what it won't do), key concepts and the FAQ.
- **Dashboard:** one page per sidebar item and Settings tab, using the dashboard's exact labels. HelpAlive's own agent answers dashboard users from these pages.
- **Developers:** install, identify, verify users, GTM, CSP, consent and reference.
  - `/installation` and `/sdk/verify-users` are linked from the dashboard's AI-agent prompts. Keep those paths and keep each page self-contained.
- **Trust:** security, data, AI and your data, and subprocessors. Data facts come only from the privacy and terms brief.

## Rules
- The product code (EgeGulce/HelpAlive_tracker) is the source of truth. Check every claim against it.
- Never name models or vendors outside Subprocessors. Never describe prompts, ranking, thresholds, internal limits or costs.
- No prices. Prices are shown in the dashboard under Settings → Billing and usage.
- Never document what doesn't exist (roles, an enterprise plan, a trial before it ships, Studio, integrations, export, citations).
- Never promise in-browser redaction, or cleaning of addresses or chat text.
- "Assist" is the word for a task the agent does or shows, as the dashboard, billing and packs say it. Where a page describes what end users see inside the widget, it is a "walkthrough", with the widget's own labels (the start button for Shows how is **Show me how**). Keep "guide" only as a search keyword. "Tenant" is the word for a customer's customer (`tenantId`).
- Style: active voice, second person, sentence-case headings, sentences under 25 words, bold dashboard labels with paths (**Settings → Team & access**), at most two callouts per page, and "Next" cards at the end.
